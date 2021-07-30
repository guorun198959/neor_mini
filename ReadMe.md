# Neor mini Simulation Tutorials

## Chapter 1: Make a indoor autonomous mobile robot

![](pictures/neor_mini_gazebo.png)

Developing Environments:

```bash
ubuntu 18.04 + ROS Melodic desktop full 
```

Explaination:

```bash
mini_sim18_ws                               #   this folder is ROS Workspace, you can run launchs and look at every demo.
original_neor_mini                       #    this folder is an original neor_mini urdf file, you can construction by yourself 
pictures                                              #    the total process pictures
```



Neor mini Simulation in Gazebo with ROS, Follow below steps:

Step 1:

```bash
# open your Terminal
git clone https://github.com/COONEO/neor_mini.git
cd neor_mini/mini_sim18_ws
rosdep install --from-paths src --ignore-src -r -y     # you need wait a moment
catkin_make
```

You can see 5 ROS packages in mini_sim18_ws/src folder,lists:

```bash
neor_mini                           # Storing the description of neor mini's appearance with urdf file
steer_drive_ros                # Ackermann kinematics ROS plugins
steer_mini_gazebo         # Storing the launch files of neor mini model visual in Gazebo
mini_gmapping                # Storing the launch files and gmapping params files
mini_navigation               # Storing the launch file and navigation params files
```



Step 2: launch neor_mini's launch file, visualize the urdf in Rviz.

```bash
# show the neor_mini.urdf in Rviz
cd ~/neor_mini/mini_sim18_ws
source devel/setup.bash
roslaunch neor_mini display.launch
```

![](pictures/neor_mini_rviz.png)



```bash
#show the neor_mini_gazebo_sensors.urdf in Rviz
cd ~/neor_mini/mini_sim18_ws
source devel/setup.bash
roslaunch neor_mini display_gazebo_sensors.launch
```

<img src="pictures/neor_mini_gazebo_sensors.png" style="zoom:150%;" />



Step 3: launch steer_mini_gazebo's launch file. visualize the urdf in Gazebo and try to control neor_mini .

```bash
#show the neor_mini_gazebo.urdf in Gazebo
cd ~/neor_mini/mini_sim18_ws
source devel/setup.bash
roslaunch steer_mini_gazebo steer_mini_sim.launch
```

![](pictures/visual neor_mini gazebo.png)



```bash
#show the neor_mini_gazebo_sensors.urdf in Gazebo
cd ~/neor_mini/mini_sim18_ws
source devel/setup.bash
roslaunch steer_mini_gazebo steer_mini_sim_sensors.launch
```

![](pictures/neor_mini_sensors_gazebo.png)



Step 4 : Gmapping with neor_mini urdf

```bash
# launch gmapping_steer_mini_sim.launch file and construction map
cd ~/neor_mini/mini_sim18_ws
source devel/setup.bash
roslaunch mini_gmapping gmapping_steer_mini_sensors.launch
```

![](pictures/gmapping_neor_mini.png)

When you think the construction map is finished, Open a new terminal, you can run the below command to save the map.

```bash
cd ~/neor_mini/mini_sim18_ws/
source devel/setup.bash
cd src/mini_gmapping/map
rosrun map_server map_saver -f cooneo_office_map      # You can saved as another name
```



Step 5 : Using cooneo_office_map to make a navigation demo.

```bash
cd ~/neor_mini/mini_sim18_ws/
source devel/setup.bash
roslaunch mini_navigation navigation_steer_mini_sensors.launch                           # start a navigation demo
```

![](pictures/navigation_Demo.png)

When you run up the list launch file, your monitor will show two windows, one is Rviz, another is Gazebo. looking at the rviz window up toolbar, you need to click "2D Nav Goal", and select a navigation goal on the map, soon the neor_mini model car will plan a route, and arrived.



## Chapter 2: Make a outdoor line-tracking mobile robot

![](pictures/new_neor_mini.png)



step 1 : Copy Gazebo-world from neor_mini_linetrack/models folder

```bash
# open a Terminal
cp -r ~/neor_mini/mini_sim18_ws/src/neor_mini_linetrack/models/* ~/.gazebo/models/
```

and you will see four new folder in your ./gazebo/models folder as follows:

![](pictures/folder_list.png)



Step 2 : launch neor_mini_linetrack package launch file

```bash
# open a Terminal 
cd ~/neor_mini/mini_sim18_ws/
catkin_make                                                         # compile you all ros node in you workspace
source devel/setup.bash
roslaunch neor_mini_linetrack neor_mini_linetrack_day.launch
```

and you will see:

![](pictures/gazebo_start_success.png)



Step 3: rosrun line-tracking python node

```bash
# open a Terminal
cd ~/neor_mini/mini_sim18_ws/
source devel/setup.bash
rosrun neor_mini_linetrack line_follower_object.py
```

and you will see four new windows ,it's like this:

![](pictures/line-tracking_start_success.png)

Congratulations!!! you will see the neor_mini urdf model running automatically using the data from the camera. 



## Chapter 3: Velodyne-16 && Lego_LOAM 3D Mapping

![](pictures/face_img.png)



Step 1: launch velodyne_simulator ROS package,and you will see:

```bash
# open a Terminal
cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash

# launch VLP-16
roslaunch velodyne_description velodyne_16.launch
```

![](pictures/velodyne_16.png)



Step 2: Install the Velodyne-16 on the neor_mini

```bash
# open a Terminal
cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash

# start launch file and see neor_mini + vlp-16 in Rviz
roslaunch neor_mini display_gazebo_sensors_VLP16.launch
```

![](pictures/neor_mini_vlp16_rviz.png)



Step 3: launch the neor_mini+vlp16 model in Gazebo

```bash
# open a Terminal and add the museum model world into Gazebo's model folder
cp -r ~/neor_mini/mini_sim18_ws/src/neor_mini/models/* ~/.gazebo/models/

# open a Terminal
cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash

# start launch file and see neor_mini + vlp-16 in Gazebo
roslaunch steer_mini_gazebo steer_mini_sim_sensors_VLP16.launch 
```

![](pictures/neor_mini_vlp16_Gazebo.png)

*(Gazebo from: iscas_museum.world )*



Step 4: Record the data collected by VLP-16 in Gazebo

```bash
# open a Terminal
cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash
roslaunch steer_mini_gazebo steer_mini_sim_sensors_VLP16.launch 

# open another Terminal and record VLP-16 data By rosbag
cd ~/neor_mini/mini_sim18_ws/     
rosbag record -a

# and then, control the neor_mini traverses the iscas_museum world
```

![](pictures/record_data_demo.gif)



Step 5: Play the Record data && Mapping by Lego_Loam

```bash
# open a Terminal
cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash
 roslaunch lego_loam run.launch 
 
 # open another Terminal and "cd" in your bag file folder
 cd ~/neor_mini/mini_sim18_ws/     
source devel/setup.bash
rosbag play  ####.bag         # replace with your own or current bag file's name

# and then,you will see mapping by Leog_loam
```

![](pictures/lego_loam_record_data.gif)

Congratulations!!! You can run Velodyne-16 Lidar in Gazebo with neor_mini and Mapping by Lego_Loam. 



The Lego_loam ROS Package from The RobustFieldAutonomyLab.

```bash
@inproceedings{legoloam2018,
  title={LeGO-LOAM: Lightweight and Ground-Optimized Lidar Odometry and Mapping on Variable Terrain},
  author={Shan, Tixiao and Englot, Brendan},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  pages={4758-4765},
  year={2018},
  organization={IEEE}
}
```



​																																									2021.07.30     

​																																				author:ZhaoXiang Lee

COONEO Co.,Ltd

Web:http://cooneo.cc

E: cooneo@outlook.com



For more details,you can search "COONEO" in your WeChat.

![](pictures/COONEO_wechat.jpg)

# neor_mini