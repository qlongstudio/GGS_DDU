# HITDLRBOT Package Description
Date: 2020-05-04 20:00  

## 功能包目录及简介
    1.  hitdlrbot_description: 机器人描述文件功能包.  
    2.  hitdlrbot_control: gazebo控制器配置文件功能包.  
    3.  hitdlrbot_gazebo: 存放gazebo或Rviz显示launch文件功能包.  
    4.  hitdlrbot_moveit: moveit配置文件功能包.  
    5.  hitdlrbot_planning: 任务规划程序功能包.  
    6.  hitdlrbot_teleop: 遥操作功能包.
    7.  hitdlrbot_navigation: 导航功能包.  
    8.  other_robot: 可能用到的其它机器人的功能包.
###tip
    可以利用tree命令打印出文件结构.

## package:hitdlrbot_description
    ├── CMakeLists.txt
    ├── config
    ├── launch
    │   └── display_Rivz.launch
    ├── meshes
    ├── navg.rviz
    ├── package.xml
    ├── urdf
    │   ├── hitdlrbot_all.xacro  机器人总的描述文件
    │   ├── hitdlrbot_arm_left.xacro
    │   ├── hitdlrbot_body.xacro 机器人body的描述文件
    │   ├── hitdlrbot.gazebo.xacro gazebo标签配置文件,包括gazebo插件,link和joint属性.
    │   ├── hitdlrbot.gv 
    │   ├── hitdlrbot_hand 手的描述文件,该版本不是灵巧手.
    │   │   ├── actuators.urdf.xacro
    │   │   ├── deg_to_rad.urdf.xacro
    │   │   ├── fingers.urdf.xacro
    │   │   ├── gazebo.urdf.xacro
    │   │   ├── hand.urdf
    │   │   ├── hey5_hand_all.urdf.xacro
    │   │   ├── hey5_hand.xacro
    │   │   ├── materials.urdf.xacro
    │   │   ├── palm.urdf.xacro
    │   │   ├── transmissions.urdf.xacro
    │   │   └── two_hands.rviz
    │   ├── hitdlrbot.pdf
    │   ├── hitdlrbot_sensor.xacro 传感器描述文件,包括imu,深度相机等
    │   ├── hitdlrbot_transmission_arm_left.xacro
    │   ├── hitdlrbot_transmission.xacro 机器人transmission配置文件
    │   ├── hitdlrbot.urdf
    │   └── hitdlrbot_wheel.xacro 机器人轮子描述文件
    ├── gazebo_moveit.rviz gazebo&moveit联合仿真
    ├── gzmoveit.rviz 
    ├── gzmoveit_video.rviz gazebo&moveit联合仿真录视频用
    ├── gz.rviz
    └── urdf.rviz

###特别说明:
    在hitdlrbot_body.xacro文件中，第一个连杆主要用途为与gazebo或Rviz世界系连接,即机器人在gazebo或Rviz中的位置是该系参照于gazebo或Rviz中的{world},因此是虚的连杆，没有实体.
    
## package:hitdlrbot_control  
    ├── CMakeLists.txt
    ├── config: gazebo controller 配置文件夹
    │   ├── bak 
    │   ├── gazebo_states_control.yaml: 发布gazebo中joint state的控制器
    │   ├── hitdlrbot_controller_body_JntPosCon.yaml: 配置机器人body的joint控制器,类型为JointPositionController
    │   ├── hitdlrbot_controller_body_JntTrajCon.yaml: 配置机器人body的joint控制器,类型为JointTrajectoryController
    │   ├── hitdlrbot_controller_hand.yaml: 配置手的joint,类型为JointPositionController
    │   └── hitdlrbot_controller_wheel.yaml: 配置轮子的joint,类型为JointVelocityController
    ├── launch: 用于启动对应controller的launch文件
    │   ├── gazebo_states_controller.launch
    │   ├── hitdlrbot_controller_all.launch: 启动所有需要的controller
    │   ├── hitdlrbot_controller_body_JntPosi.launch
    │   ├── hitdlrbot_controller_body_JntTraj.launch
    │   ├── hitdlrbot_controller_hand.launch
    │   └── hitdlrbot_controller_wheel.launch
    ├── package.xml
    ├── script:存放一些控制程序,如操作界面
    │   ├── bak: 备份文件夹
    │   ├── hand_control.py
    │   ├── hirdlrbot_transpose_auto.py
    │   ├── hitdlrbot_gazebo_rviz_test.py
    │   ├── hitdlrbot.py: gui所需要的程序
    │   ├── hitdlrbot.pyc: gui所需要的程序
    │   ├── kinematics_algorithm.py~: 自动生成的程序
    │   ├── tarj_data.py~: 自动生成的程序
    │   ├── user_gui.py: gui所需要的程序
    │   ├── user_gui.pyc: gui所需要的程序
    │   ├── user_kdl.py
    │   ├── user_node.py: 启动gui界面的程序
    │   ├── user_node_ql.py
    │   ├── user_plan.py
    │   ├── user_subfun.py
    │   ├── user_subfun.pyc
    │   ├── user_teleop_hand.py
    │   ├── v_ctrl.py: 控制移动平台的程序
    │   ├── v_ctrl.pyc
    │   └── v_ctrl.ui
    └── src 
###特别注意:
    JointTrajectoryController类型的控制器自带插值,详情可见wiki:http://wiki.ros.org/joint_trajectory_controller/  
    使用注意:python使用时,发的数据注意使用deepcopy,时间要递增.详情可参考hitdlrbot_planning功能包中hirdlrbot_carrywood_righthand_test.py.
    
##package:hitdlrbot_gazebo
    ├── CMakeLists.txt
    ├── include
    ├── launch:存放gazebo或Rviz显示的launch
    │   ├── display_Rivz.launch: 只启动Rviz
    │   ├── gazebo.launch: 只启动Gazebo
    │   ├── gazebo_moveit.launch: 启动gazebo和Rviz(包括moveit,octomap)
    │   ├── gazebo_navigation.launch: 启动gazebo和Rviz,用于测试建立二维栅格地图
    │   ├── hitdlrbot_world.launch: 启动gazebo的world文件,为测试比赛任务的环境
    │   └── hitdlrbot_world_navigation.launch: 启动gazebo的world文件,为测试建立map的环境
    ├── package.xml
    ├── script
    ├── src
    └── worlds: 存放gazebo的world文件
        ├── cloister.world
        ├── empty.world
        ├── hitdlrbot_grasp_cokacan.world
        ├── hitdlrbot_grasp_drill.world
        ├── hitdlrbot_move_test.world
        ├── hitdlrbot.world
        ├── hitdlrbot_world_v1.world
        ├── open_door.world
        ├── task_test_v2.world: 比赛任务测试环境
        ├── task_test.world
        └── useful_models.world
###特别注意:
    可能遇到的问题:
    1.不能成功加载gazebo的world.  
    解决方案:需要将gazebo的world文件中的模型路径改为自己的路径(<uri>/home/shuxin/.gazebo/models/door_handle/meshes/handle.dae</uri>).
    
##package:hitdlrbot_moveit
    该文件由moveit_setup_assistant自动社过程
    ├── CMakeLists.txt
    ├── config
    │   ├── chomp_planning.yaml
    │   ├── fake_controllers.yaml
    │   ├── hitdlrbot.srdf
    │   ├── joint_limits.yaml
    │   ├── kinematics.yaml: 运动学配置文件,可以设置为kdl,trac-ik等
    │   ├── ompl_planning.yaml
    │   ├── ros_controllers_2.yaml
    │   ├── ros_controllers.yaml
    │   └── sensors_3d.yaml: 若使用octomap,需要配置该文件
    ├── launch
    │   ├── chomp_planning_pipeline.launch.xml
    │   ├── default_warehouse_db.launch
    │   ├── demo_gazebo.launch
    │   ├── demo.launch
    │   ├── fake_moveit_controller_manager.launch.xml
    │   ├── gazebo.launch
    │   ├── hitdlrbot_moveit_controller_manager.launch.xml: 若需要跟gazebo联合,需要配置该controller文件
    │   ├── hitdlrbot_moveit_sensor_manager.launch.xml
    │   ├── joystick_control.launch
    │   ├── move_group.launch
    │   ├── moveit_planning_execution.launch
    │   ├── moveit.rviz
    │   ├── moveit_rviz.launch
    │   ├── ompl_planning_pipeline.launch.xml
    │   ├── planning_context.launch
    │   ├── planning_pipeline.launch.xml
    │   ├── ros_controllers.launch
    │   ├── run_benchmark_ompl.launch
    │   ├── sensor_manager.launch.xml
    │   ├── setup_assistant.launch
    │   ├── trajectory_execution.launch.xml
    │   ├── warehouse.launch
    │   └── warehouse_settings.launch.xml
    └── package.xml
###特别注意
    Rviz中有octomap插件,需要安装:sudo apt-get install ros-melodic-octomap*
    trac-ik插件安装:sudo apt-get install ros-melodic-trac-ik*
    
##hitdlrbot_planning
    ├── CMakeLists.txt
    ├── __init__.py
    ├── __init__.pyc
    ├── package.xml
    ├── scripts
    │   ├── bak: 备份文件夹
    │   ├── gazebo_moveit:调用的moveit接口的程序
    │   │   ├── gzmoveit_carrywood_dualhands_test.py
    │   │   ├── gzmoveit_open_door_righthand_test_2.py
    │   │   ├── gzmoveit_open_door_righthand_test.py
    │   │   ├── hirdlrbot_carrywood_dualhands_test_video.py:双手搬运任务(即含避障)
    │   │   ├── hirdlrbot_reset_pose.py:恢复机器人姿态的程序(防止避自碰)
    │   │   ├── moveit_cartesian_demo.py
    │   │   ├── moveit_cartesian_test.py
    │   │   ├── moveit_fk_demo.py
    │   │   ├── moveit_hitdlrbot.py
    │   │   ├── moveit_ik_demo.py
    │   │   ├── moveit_selfdefine_joint.py
    │   │   ├── pose_conv.py
    │   │   ├── subfun.py
    │   │   ├── user_subfun.py
    │   │   └── user_subfun.pyc
    │   ├── hirdlrbot_carrywood_dualhands_test.py:双手搬运任务(没有避障)
    │   ├── hirdlrbot_carrywood_righthand_test.py
    │   ├── hirdlrbot_climping_stairs.py
    │   ├── hirdlrbot_open_door_righthand_test.py:开门任务(没有避障)
    │   ├── hirdlrbot_transpose_.py
    │   ├── hitdlrbot_hand_control.py:手的控制程序,已设置gazebo的launch文件启动时会一起启动.
    │   ├── hitdlrbot_hand_telep.py:手的遥操作控制程序.
    │   ├── __init__.py
    │   ├── __init__.pyc
    │   ├── kinematics_useful_code.py
    │   ├── python_useful_code.py
    │   ├── user_subfun.py
    │   └── user_subfun.pyc
    └── src
###特别注意:
    通过对比位姿,确认末端的位姿参考坐标系是base_footprint,通过直接设置关节角可以成功实现.
    move他联合gazebo后,规划容易失败,在规划可行的前提下,多执行几次就可.
    报错:Solution found but controller failed during execution.可以加个循环,多执行几次后即可成功.

##package:hitdlrbot_teleop
    功能描述:目前主要存放了遥操作履带的程序
    ├── CMakeLists.txt
    ├── include
    │   └── hitdlrbot_teleop
    ├── __init__.py
    ├── mainpage.dox
    ├── Makefile
    ├── manifest.xml
    ├── script
    │   ├── hirdlrbot_teleop_alltrack.py
    │   ├── hirdlrbot_teleop_innertrack.py
    │   ├── hitdlrbot_teleop_outertrack.py
    │   ├── __init__.py
    │   ├── user_subfun.py
    │   ├── user_subfun.pyc
    │   ├── user_teleop_alltrack.py
    │   ├── user_teleop_innertrack.py
    │   └── user_teleop_outertrack.py
    └── src

## package:hitdlrbot_navigation
    ├── CMakeLists.txt
    ├── config
    │   ├── move_base_planning: move_base移动平台路径规划配置文件.[待用]
    │   │   ├── base_local_planner_params.yaml
    │   │   ├── costmap_common_params.yaml
    │   │   ├── global_costmap_params.yaml
    │   │   └── local_costmap_params.yaml
    │   └── rplidar.lua:雷达配置参数文件
    ├── include
    │   └── hitdlrbot_navigation
    ├── launch
    │   ├── amcl.launch
    │   ├── cartographer_demo_rplidar.launch:谷歌建图方法
    │   ├── gmapping_demo.launch:gmapping建图方法
    │   ├── gmapping.launch
    │   ├── mbot_laser_nav_gazebo.launch
    │   ├── move_base.launch
    │   ├── nav_cloister_demo.launch
    │   └── octomap_display.launch
    ├── maps
    │   ├── blank_map.pgm
    │   ├── blank_map_with_obstacle.pgm
    │   ├── blank_map_with_obstacle.yaml
    │   ├── blank_map.yaml
    │   ├── cloister_gmapping.pgm
    │   ├── cloister_gmapping.yaml
    │   ├── cloister_hector.pgm
    │   ├── cloister_hector.yaml
    │   ├── gmapping_map.pgm
    │   ├── gmapping_map.yaml
    │   ├── hector_map.pgm
    │   ├── hector_map.yaml
    │   ├── room.pgm
    │   ├── room.yaml
    │   ├── test_map.pgm
    │   └── test_map.yaml
    ├── package.xml
    ├── rviz
    │   └── navigation.rviz
    ├── script
    │   ├── odometry_publisher.py:里程计发布程序
    │   └── odom_.py
    └── src

##other_robot
    暂未使用

