# traffic_simulation
﻿# **文件准备**

## **A. SUMO安装**

1. **安装步骤**

```c++
sudo apt-get install cmake python g++ libxerces-c-dev libfox-1.6-dev libgdal-dev libproj-dev libgl2ps-dev swig
 git clone --recursive https://github.com/eclipse/sumo
 export SUMO_HOME="$PWD/sumo"
 mkdir sumo/build/cmake-build && cd sumo/build/cmake-build
 cmake ../..
 make -j$(nproc)
```

2. **运行：```$PWD/sumo```路径下**

```
$ sumo
或者
$sumo-gui
```

3. **错误解决：无法正常运行SUMO**

   * 打开系统下的bash文件，```sudo gedit  ~/.bashrc```

   * 最后一行追加

   * ```c++
     export SUMO_HOME=/home/iair/traffic_simulation/sumo
     export PATH=/home/iair/traffic_simulation/sumo/bin:${PATH}
     
     ```



## **B. 决策模块安装**

1. **代码包```catkin_ws```以及```map_ws```编译**



## **C. SUMO2Gazebo**

1. ```
   mkdir -p traffic_ws/src
   cd ~/catkin_ws2/src
   catkin_init_workspace
   
   git clone https://github.com/Tong-Xiao/sumogazebo.git
   cd ..
   catkin_make
   source devel/setup.bash
   ```

2. ```障碍物消息：第一层是ibeo_lidar_msg::object_filter_data，感知发送和决策接收类型。第二层是ibeo_lidar_msgs::object_filter，包含在object_filter_data里面，把此内容取出来做可视化。SUMO数据是先构造ibeo_lidar_msgs::object_filter，再包装成ibeo_lidar_msg::object_filter_data发送```



## **D. 运行**

1. **决策模块**

   ```cd ./map_ws
   cd ./map_ws
   source devel/setup.bash
   roslaunch vec_map_cpp vec_map_cpp_sim.launch(九宫格地图)或`roslaunch vec_map_cpp vec_map_cpp_sim_highway.launch`(高架地图)
   ```
   
   ```
   cd ./map_ws
   source devel/setup.bash
   rosrun vec_map_cpp fake_astar
   ```
   
   ```
   cd ./catkin_ws
   source devel/setup.bash
   roslaunch motion_planning multi_path_following_simulation_click_test_ulc.launch
   ```

2. **SUMO2Gazebo模块**

   ```
   cd ./sumogazebo
   source devel/setup.bash
   roslaunch sumo_simulation simulation_launch.launch
   ```

3. **给定自主车辆终点**：**点击rviz上的按键2D Nav Goal,在地图上点一个终点，加载目标点。**





## **E. 替换地图配置**

1.  `SUMO2Gazebo`中**prepare文件夹内:**

* `entrance.osm` 文件拷到目录 `./map_ws/src/vec_map_cpp/data`
* `entrance.world` 文件拷到目录 `./catkin_ws/src/multi_path_generator/worlds`
* `./traffic_ws/src`



2. ```catkin_ws```**中:**

- `multi_path_following_simulation_click_test_ulc.launch`中，修改使用的地图launch文件名称【【default: changshu.launch】
- `multi_path_following_simulation_click_test_ulc.launch`中，修改使用的world文件名称【【default: empty_world.world】
- `xian.launch`文件中

```handlebars
<node pkg="tf2_ros" type="static_transform_publisher" name="odom_shift" args="
736451.229 2879738.4 0  0 0 0  odom datum"/>
```


3. ```map_ws```**中：**

 - `roslaunch vec_map_cpp vec_map_cpp_sim.launch`中，修改使用的地图launch文件名称【【default: changshu.launch】

 - `roslaunch vec_map_cpp vec_map_cpp_sim.launch`中，修改使用的地图osm文件名称【default: nine_squares.osm】

 - `xian.launch`文件中

   ```
   <node pkg="tf2_ros" type="static_transform_publisher" name="odom_shift" args="
   736451.229 2879738.4 0  0 0 0  odom datum"/>
   ```

   
