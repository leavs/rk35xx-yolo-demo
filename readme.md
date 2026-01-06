# The Demo comes from the following Github
https://github.com/kaylorchen/rk3588-yolo-demo

# Yolov8/v10 Demo for RK3588/RK3576
The project is a multi-threaded inference demo of Yolov8 running on the RK3588 platform, which has been adapted for reading video files and camera feeds. The demo uses the Yolov8n model for file inference, with a maximum inference frame rate of up to 100 frames per second.

# Prepare

## Build the Cross-Compilation Environment
Set up a cross-compilation environment based on the following [link](https://github.com/kaylorchen/rk3588_dev_rootfs).

## Install Runtime Libraries in Your RK3588/RK3576 Target Board
```bash
cat << 'EOF' | sudo tee /etc/apt/sources.list.d/kaylordut.list 
deb [signed-by=/etc/apt/keyrings/kaylor-keyring.gpg] http://apt.kaylordut.cn/kaylordut/ kaylordut main
EOF
sudo mkdir /etc/apt/keyrings -pv
sudo wget -O /etc/apt/keyrings/kaylor-keyring.gpg http://apt.kaylordut.cn/kaylor-keyring.gpg
sudo apt update
sudo apt install kaylordut-dev libbytetrack
```
> If your OS is not Ubuntu22.04, and find [kaylordut-dev](https://github.com/kaylorchen/kaylordut) and [libbytetrack](https://github.com/kaylorchen/ByteTrack) sources in my github.

## Build the Project on Your RK3588/RK3576 Target Board

- Compile

```bash
git clone https://github.com/leavs/rk35xx-yolo-demo.git 
cd rk35xx-yolo-demo/src/yolov8
mkdir build
cd build
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON ..
make 
```

- Run
  
``` bash
Usage: ./videofile_demo [--model_path|-m model_path] [--input_filename|-i input_filename] [--threads|-t thread_count] [--framerate|-f framerate] [--label_path|-l label_path]  

Usage: ./camera_demo [--model_path|-m model_path] [--camera_index|-i index] [--width|-w width] [--height|-h height][--threads|-t thread_count] [--fps|-f framerate] [--label_path|-l label_path]

Usage: ./imagefile_demo [--model_path|-m model_path] [--input_filename|-i input_filename] [--label_path|-l label_path]
```

> you can run the above command in your rk3588/rk3576 target board. 
