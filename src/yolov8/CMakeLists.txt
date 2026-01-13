cmake_minimum_required(VERSION 3.22)
project(yolov8)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Release) # None, Debug, Release, RelWithDebInfo, MinSizeRel
#SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -Wno-unused-parameter -O0 -g -Wall")
#SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS}  -Wno-unused-parameter -O0 -g -Wall")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -Wno-unused-parameter -O3 -g -Wall")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS}  -Wno-unused-parameter -O3 -g -Wall")

include_directories(include)

add_subdirectory(utils)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
find_package(kaylordut REQUIRED)
find_package(bytetrack REQUIRED)


add_executable(videofile_demo videofile_demo.cpp)
target_link_libraries(videofile_demo ${kaylordut_LIBS} ${OpenCV_LIBS} yolov8-kaylordut ${bytetrack_LIBS})

add_executable(camera_demo camera_demo.cpp)
target_link_libraries(camera_demo ${kaylordut_LIBS} ${OpenCV_LIBS} yolov8-kaylordut ${bytetrack_LIBS})

add_executable(imagefile_demo imagefile_demo.cpp)
target_link_libraries(imagefile_demo ${kaylordut_LIBS} ${OpenCV_LIBS} yolov8-kaylordut ${bytetrack_LIBS})

# Install binary
install(TARGETS videofile_demo DESTINATION bin)
install(TARGETS imagefile_demo DESTINATION bin)
install(TARGETS camera_demo DESTINATION bin)

# Install model
install(DIRECTORY ${CMAKE_SOURCE_DIR}/model DESTINATION ${CMAKE_INSTALL_PREFIX})


# Check if CHIPS is defined, otherwise set default value
# If user does not specify -DCHIPS=RK3576 or -DCHIPS=RK3588,
# the default will be CHIPS=RK3588
if(NOT DEFINED CHIPS)
    set(CHIPS "RK3588")
endif()

# Print which chip is selected
message(STATUS "Selected CHIPS=${CHIPS}")

# Install files based on the selected chip
if(CHIPS STREQUAL "RK3576")
    # Installation rules for RK3576
    install(FILES ${CMAKE_SOURCE_DIR}/res/rk3576/NPUCamDet.desktop DESTINATION /usr/share/applications)
elseif(CHIPS STREQUAL "RK3588")
    # Installation rules for RK3588
    install(FILES ${CMAKE_SOURCE_DIR}/res/rk3588/NPUCamDet.desktop DESTINATION /usr/share/applications)
else()
    message(FATAL_ERROR "Unknown CHIPS value: ${CHIPS}")
endif()

# Install Icons
install(FILES ${CMAKE_SOURCE_DIR}/res/npu.png DESTINATION /usr/share/pixmaps)
