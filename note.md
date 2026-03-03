# livox_ros_driver2 빌드

- 상위경로로 가서 뒤적거리는거 주석화 해서 방지해야함
```bash
# cd ../..
```

- pyenv 관련 있으면 아래처럼 빌드 바꿔야함
```bash
colcon build --cmake-args \
    -DPYTHON_EXECUTABLE=/usr/bin/python3 \
    -DROS_EDITION=${VERSION_ROS2} \
    -DHUMBLE_ROS=${ROS_HUMBLE}
```

- 이제 빌드
```bash
./build.sh humble
```

- livox SDK 깔아야함
```bash
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd Livox-SDK2/
mkdir build
cd build
cmake .. && make -j
sudo make install
```





# ouster-ros 빌드

```bash
git clone -b ros2 --recurse-submodules https://github.com/ouster-lidar/ouster-ros.git
cd ouster-ros
colcon build \
--symlink-install \
--cmake-args -DCMAKE_BUILD_TYPE=Release \
-DPYTHON_EXECUTABLE=/usr/bin/python3
```





# FAST_LIO_ROS2 빌드

- PCL 깔아야함
```bash
sudo apt update
sudo apt install ros-humble-pcl-ros
```

- livox_ros_driver2 source 해줘야함
```bash
source ../livox_ros_driver2/install/setup.bash
```

- 그러고 이제 빌드 (pyenv 무시)
```bash
colcon build --symlink-install --cmake-args -DPYTHON_EXECUTABLE=/usr/bin/python3
source install/setup.bash
```





# 데이터셋 돌려보기

## HKU MARS 데이터셋

- HKU MARS에서 제공하는 데이터셋이 ros1 bag 파일이라서 ros2 bag 파일로 변환해야함
```bash
pip install rosbags
rosbags-convert --src bag_name.bag --dst folder_name
```
- 근데 livox driver의 ros1 ros2 호환 문제 (CustomMsg) 때문에 안됨

## M2DGR 데이터셋

- 데이터셋 변환
```bash
rosbags-convert --src bag_name.bag --dst folder_name
rm -rf folder_name/metadata.yaml
ros2 bag reindex folder_name
```

- 최종 실행
```bash
ros2 launch fast_lio mapping_m2dgr.launch.py
ros2 bag play ~/Documents/dataset/M2DGR/gate_01
```


## NESL ASRI 데이터셋
```bash
ros2 bag play test_6
```






# Reference

- https://github.com/Taeyoung96/FAST_LIO_ROS2?tab=readme-ov-file