Title: Readme
Date: 2020-02-04 10:00
Modified: 2020-02-06 23:54
Category: learn
Tags: gazebo
Authors: Qlong
Summary:hitdlrbot仿真使用说明，本文可以用pycharm打开，能看到排版格式(markdown)

## 基本说明
### 特别说明
在hitdlrbot_body.xacro文件中，第一个连杆是虚的连杆，没有实体，当我们把它的名字起作world的时候，就是代表世界。其他名字就随便了。由于这个虚连杆是和机器人连接fixed固定的，所以如果改为world（后面joint的child参数也要改），就会把机器人固定到世界中。目前默认是固定的。

### 文件说明

        hitdlrbot_xacro    
        ├── hitdlrbot_control #机器人的控制部分
        │   ├── CMakeLists.txt
        │   ├── config
        │   │   └── hitdlrbot_control.yaml #机器人的控制器参数文件
        │   ├── launch
        │   │   └── hitdlrbot_control.launch #机器人的控制器launch文件，可以快速部署控制器
        │   ├── package.xml
        │   └── src
        │       ├── kinematics_algorithm.py~ #隐藏文件，可能是自动添加的
        │       ├── tarj_data.py~ #隐藏文件，可能是自动添加的
        │       └── usr_node.py #我们的控制节点文件
        │       └── usr_kdl.py #用来测试kdl库的文件
        │       └── usr_plan.py #包含三次规划的函数
        |
        ├── hitdlrbot_description #模型和机器人参数
        │   ├── CMakeLists.txt
        │   ├── config
        │   │   └── joint_names_hitdlrbot.yaml #自动生成的，没有用到
        │   ├── launch
        │   │   ├── display_robot_all.launch #启动rviz
        │   │   └── gazebo.launch #启动gazebo
        │   ├── meshes #模型文件，用来定义碰撞边界和外观
        │   │   ├── arm_l0.STL
        │   │   ├── arm_l1.STL
        │   │   ├── arm_l2.STL
        │   │   ├── arm_l3.STL
        │   │   ├── arm_l4.STL
        │   │   ├── arm_l5.STL
        │   │   ├── arm_l6.STL
        │   │   ├── arm_r0.STL
        │   │   ├── arm_r1.STL
        │   │   ├── arm_r2.STL
        │   │   ├── arm_r3.STL
        │   │   ├── arm_r4.STL
        │   │   ├── arm_r5.STL
        │   │   ├── arm_r6.STL
        │   │   ├── base_link.STL
        │   │   ├── h0.STL
        │   │   ├── h1.STL
        │   │   ├── hand_l.STL
        │   │   ├── hand_r.STL
        │   │   ├── leg_l0.STL
        │   │   ├── leg_l1.STL
        │   │   ├── leg_r0.STL
        │   │   ├── leg_r1.STL
        │   │   ├── t0.STL
        │   │   └── t1.STL
        |   ├── urdf.rviz #rviz属性设置文件
        │   ├── package.xml
        │   ├── urdf 
        │   │   ├── hitdlrbot_all.xacro #主要的xacro，把另外两个融合在一起
        │   │   ├── hitdlrbot_body.xacro #urdf结构部分，同时包括了控制器接口类型定义
        │   │   └── hitdlrbot.gazebo #gazebo属性，主要是摩擦
        │   │   └── hitdlrbot_urdf.urdf #为了方便使用kdl库，而保留的urdf格式
        │   └── worlds
        │       └── hitdlrbot.world #gazebo世界文件，可以在gezebo里面生成后保存到此处，实现自定义
        |
        ├── hitdlrbot_moveit_config #moveit相关，暂时没用到
        │   ├── CMakeLists.txt
        │   ├── config
        │   │   ├── chomp_planning.yaml
        │   │   ├── fake_controllers.yaml
        │   │   ├── hitdlrbot_all.srdf
        │   │   ├── joint_limits.yaml
        │   │   ├── kinematics.yaml
        │   │   ├── ompl_planning.yaml
        │   │   ├── ros_controllers.yaml
        │   │   └── sensors_3d.yaml
        │   ├── launch
        │   │   ├── chomp_planning_pipeline.launch.xml
        │   │   ├── default_warehouse_db.launch
        │   │   ├── demo_gazebo.launch
        │   │   ├── demo.launch
        │   │   ├── fake_moveit_controller_manager.launch.xml
        │   │   ├── gazebo.launch
        │   │   ├── hitdlrbot_all_moveit_controller_manager.launch.xml
        │   │   ├── hitdlrbot_all_moveit_sensor_manager.launch.xml
        │   │   ├── joystick_control.launch
        │   │   ├── move_group.launch
        │   │   ├── moveit.rviz
        │   │   ├── moveit_rviz.launch
        │   │   ├── ompl_planning_pipeline.launch.xml
        │   │   ├── planning_context.launch
        │   │   ├── planning_pipeline.launch.xml
        │   │   ├── ros_controllers.launch
        │   │   ├── run_benchmark_ompl.launch
        │   │   ├── sensor_manager.launch.xml
        │   │   ├── setup_assistant.launch
        │   │   ├── trajectory_execution.launch.xml
        │   │   ├── warehouse.launch
        │   │   └── warehouse_settings.launch.xml
        │   └── package.xml
        ├── hitdlrbot_planning #moveit相关，暂时没用到
        │   ├── CMakeLists.txt
        │   ├── package.xml
        │   ├── scripts
        │   │   ├── moveit_cartesian_demo.py
        │   │   ├── moveit_fk_demo.py
        │   │   ├── moveit_hitdlrbot.py
        │   │   ├── moveit_ik_demo.py
        │   │   └── moveit_selfdefine_joint.py
        │   └── src
        │       ├── jnt_armL_input.cpp
        │       ├── JointState_publisher_armL.cpp
        │       ├── JointState_set_armL copy.cpp
        │       ├── JointState_set_armL.cpp
        │       └── rec_input.cpp
        ├── hrl-kdl #kdl库部分，已经下载好了，需要安装
        │   ├── hrl_geom
        │   │   ├── build
        │   │   │   └── lib.linux-x86_64-2.7
        │   │   │       └── hrl_geom
        │   │   │           ├── __init__.py
        │   │   │           ├── pose_converter.py
        │   │   │           └── transformations.py
        │   │   ├── CMakeLists.txt
        │   │   ├── example.py
        │   │   ├── package.xml
        │   │   ├── setup.py
        │   │   └── src
        │   │       └── hrl_geom
        │   │           ├── __init__.py
        │   │           ├── pose_converter.py
        │   │           ├── pose_converter.pyc
        │   │           ├── transformations.py
        │   │           └── transformations.pyc
        │   ├── hrl_kdl
        │   │   ├── CMakeLists.txt
        │   │   └── package.xml
        │   ├── pykdl_utils
        │   │   ├── build
        │   │   │   └── lib.linux-x86_64-2.7
        │   │   │       └── pykdl_utils
        │   │   │           ├── __init__.py
        │   │   │           ├── joint_kinematics.py
        │   │   │           ├── kdl_kinematics.py
        │   │   │           └── kdl_parser.py
        │   │   ├── CMakeLists.txt
        │   │   ├── package.xml
        │   │   ├── setup.py
        │   │   └── src
        │   │       └── pykdl_utils
        │   │           ├── __init__.py
        │   │           ├── joint_kinematics.py
        │   │           ├── kdl_kinematics.py
        │   │           ├── kdl_kinematics.pyc
        │   │           ├── kdl_parser.py
        │   │           └── kdl_parser.pyc
        │   └── README.md
        └── readme.md
### 指令说明
把hitdlrbot_xacro放入catkin_ws的src中，运行catkin make，编译成功后就可以用了。

如果要运行gazebo

        roslaunch hitdlrbot_description gazebo.launch 

注意：可能会报错，提示pid参数不对，这个可以忽略（没有确切结论，直接的原因是用了直接的位置控制，不需要pid，没有力矩内环）

如果要运行rviz
      
        roslaunch hitdlrbot_description display_robot_all.launch 

但是，打开过gazebo再打开rviz，会有bug，机器人tf错误，导致散架。这个问题，暂时没有解决。

gazebo.launch里面包含了启动控制control.launch的部分。control.launch中又包含了user_node的启动。


## KDL库的说明（python）
### 安装kdl
参考下面的内容，但是前两步我们都做过，其中clone这一步，我已经把文件放到压缩包里了，直接可以在这个hrl-kdl文件夹中继续后面的部分

        https://blog.csdn.net/liyinghua6/article/details/86615499

### 参考资料
介绍c++和python下的kdl

        https://blog.csdn.net/LordofRobots/article/details/77949111
        
比较详细的中文资料，该作者好像还有别的资料

        https://www.cnblogs.com/21207-iHome/p/8316030.html
  
感觉不够全的官方文档 
     
        http://wiki.ros.org/pykdl_utils
        
貌似是比较全的官方文档，还没看      

        https://scipy-cookbook.readthedocs.io/items/KDTree_example.html
