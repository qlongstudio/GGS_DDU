Title: hitdlrbot
Date: 2020-01-13 10:00
Modified: 2020-02-04 10:00
Category: learn
Tags: gazebo
Authors: Qlong
Summary: ROS+Gazebo学习记录
## ROS+Gazebo
### 一些不会被默认安装的包
ros_control补充包：

    sudo apt-get install ros-melodic-ros-control*
    
controller-manager补充包:

    sudo apt-get install ros-melodic-rqt-controller-manager*
    
rqt_controller-manager补充包:

    sudo apt-get install ros-melodic-rqt-controller-manager*
    
rqt补充包:

    sudo apt-get install ros-melodic-rqt*
    
查询ros_melodic的可用包，实际上这个个指令可以继续改写，它查的是开头为ros-melodic的所有包:
  
    apt-cache search ros-melodic
    
如果改为下面这种，就会查到开头为ros-melodic-ros-control的所有包:

    apt-cache search ros-melodic-ros-control*
    
关于topic和node的查询，例如/joy_node节点通过/joy话题和/keyboard_node通讯：
查找所有运行中的话题：

    rostopic list
    
查看话题/joy的相关信息：

    rostopic info /joy

我们可以得到：

    Type: sensor_msgs/Joy
    Publishers: 
      */joy_node (http://localhost:34419/)
    Subscribers: 
      */keyboard_node (http://localhost:37451/)
  
进一步查看这个type：

    rosmsg show sensor_msgs/Joy

可以得到：

    std_msgs/Header header
      uint32 seq
      time stamp
      string frame_id
    float32[] axes
    int32[] buttons


当我们使用python对这个话题进行接收的时候：

    from sensor_msgs.msg import Joy

    rospy.Subscriber('joy', Joy, callback)

    def callback(data):

        global v_y
        global v_x
        v_y = 40 * data.axes[0]  #left 1 right -1
        v_x = 30 * data.axes[4]  #up 1 down -1
        rospy.loginfo('%0.1f, %0.1f', v_y, v_x)
