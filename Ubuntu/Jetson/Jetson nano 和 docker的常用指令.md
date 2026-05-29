# PC
## 1. 允许容器调用屏幕
xhost +local:docker

## 2. 加载专属环境并进入容器
docker run -it --rm --name foxy_gui --net=host --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" my_foxy_rviz

## 3. 激活协议并启动软渲染 RViz2
#export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
LIBGL_ALWAYS_SOFTWARE=1 rviz2

export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp # 强行把 PC 的通信也绑死在 Wi-Fi 网卡上，不许走 docker0 虚拟网卡！ export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlp3s0</NetworkInterfaceAddress></General></Domain></CycloneDDS>" source /opt/ros/foxy/setup.bash


## 1. 进入正在运行的 foxy_gui 容器
docker exec -it foxy_gui bash

## 2. 激活协议并启动键盘遥控节点

export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
#### 强行把 PC 的通信也绑死在 Wi-Fi 网卡上，不许走 docker0 虚拟网卡！
export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlp3s0</NetworkInterfaceAddress></General></Domain></CycloneDDS>"
source /opt/ros/foxy/setup.bash
#### 再次启动键盘遥控
ros2 run teleop_twist_keyboard teleop_twist_keyboard
### 废弃版本 
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
source /opt/ros/foxy/setup.bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard




# NANO

ssh jetson@172.20.10.2

sudo sh -c 'echo 155 > /sys/devices/pwm-fan/target_pwm'&sudo systemctl stop gdm3



# 启动 SLAM 和 PC 时的起手式 
以后只要你在 **Nano 新开终端跑 SLAM**，或者在 **PC 端打开 RViz2** 之前，**必须**先念下面这套新的唤醒咒语（可以把它加到你的 `.bashrc` 里一劳永逸，或者每次手动复制粘贴）：

### **Nano 的 ===SLAM终端===起手式：**

export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlan0</NetworkInterfaceAddress></General></Domain></CycloneDDS>"
source ~/ros2_ws/install/setup.bash
ros2 launch slam_toolbox online_async_launch.py use_sim_time:=false

### ===所有Nano终端===
echo 'export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp' >> ~/.bashrc echo 'export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlan0</NetworkInterfaceAddress></General></Domain></CycloneDDS>"' >> ~/.bashrc source ~/.bashrc

### ===所有PC docker===





### 2. 所有**新开的终端**必须也加这两行环境变量

**超级大坑！** xstart.sh 里的环境变量**只对 xstart 自己启动的节点生效**，你单独开的终端不会继承！

也就是说：

- 你新开终端跑 `ros2 topic hz /map`
- 你新开终端手动启动 SLAM
- 你新开终端跑任何 ROS2 命令

默认还是用 FastRTPS，和 xstart 里的 CycloneDDS 节点**完全通信不上**，你会看到 "no new messages"，以为还是没修好。

**解决方法：**

每次新开终端跑 ROS2 命令之前，先复制这两行：

bash

运行

```bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlan0</NetworkInterfaceAddress></General></Domain></CycloneDDS>"
```

**一劳永逸方法（推荐）：**

把这两行加到 `~/.bashrc` 最底部，以后新开终端自动生效：

bash

运行

```bash
echo 'export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp' >> ~/.bashrc
echo 'export CYCLONEDDS_URI="<CycloneDDS><Domain><General><NetworkInterfaceAddress>wlan0</NetworkInterfaceAddress></General></Domain></CycloneDDS>"' >> ~/.bashrc
source ~/.bashrc
```


**PC 端 (Docker) 的起手式：** PC 端不需要锁 `wlan0`，但**必须**对齐协议：

Bash

```
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
source /opt/ros/foxy/setup.bash
# 然后再开 rviz2 或者 ros2 topic hz /map
```