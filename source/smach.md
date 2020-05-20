Title: Readme
Date: 2020-05-20 16:50
Modified: 2020-05-20 20:50
Category: learn
Tags: smach
Authors: Qlong
Summary:ros smach学习笔记

## ROS+smach
### ***smach和smach viewr基本说明***
1. 完整的smach安装指令：
```
sudo apt-get install ros-melodic-smach*
```
建议及时更新一些系统软件，不然可能装不上。

2. 启动监视器viewer的指令：
```
rosrun smach_viewer smach_viewer.py
```

3. 以下代码加到状态机sm运行之后，才会启动监视器功能，注意根据实际改此处sm的名字。
```
    sis = smach_ros.IntrospectionServer('server_name', sm, '/SM_ROOT')
    sis.start()
    # Execute SMACH plan
    outcome = sm.execute()
    rospy.spin() #让程序在这里卡住循环，才能用viewer观察。
    sis.stop()
```
    
### ***创建一个状态机***
#### 创建状态
1. 从State基类继承，并有两个方案。
```
  class Foo(smach.State):
     def __init__(self, outcomes=['outcome1', 'outcome2']):
       # Your state initialization goes here

     def execute(self, userdata):
        # Your state execution goes here
        if xxxx:
            return 'outcome1'
        else:
            return 'outcome2'
```
2. 在init方法中，可以初始化状态类。
   确保切勿阻塞init方法！
   如果需要等待系统的其他部分启动，请从单独的线程执行此操作。
   **用户的初始化代码写在这里**

3. 在execute方法中，可以执行所需的任何代码。
   只要您愿意，可以使用此方法进行阻止。
   从此方法返回后，当前状态即告完成。
   **用户的执行代码写在这里。**

4. 状态结束时，它返回结果。
   每个状态都有许多与之相关的可能结果。
   结果是描述状态如何完成的用户定义的字符串。
   例如，一组可能的结果可能是（“成功”，“失败”，“真棒”）。
   将根据前一个状态的结果指定到下一个状态的转换。
   **用户可以return字符串结果。**

#### 把状态加入到状态机
1. 状态机是包含多个状态的容器。
   将状态添加到状态机容器时，可以指定状态之间的转换。
```
  sm = smach.StateMachine(outcomes=['outcome4','outcome5'])
  with sm:
     smach.StateMachine.add('FOO', Foo(),
                            transitions={'outcome1':'BAR',
                                         'outcome2':'outcome4'})
     smach.StateMachine.add('BAR', Bar(),
                            transitions={'outcome2':'FOO'})
```
2. 初始化状态机的时候，也会定义状态机的输出。
   用add方法把状态加入，同时用transitions参数设定结果与状态变化的对应关系。
   ![smatch_simple](images/smatch_simple.png '示例')  

#### 一个例子
```
#!/usr/bin/env python

import rospy
import smach

# define state Foo
class Foo(smach.State):
    def __init__(self):
        smach.State.__init__(self, outcomes=['outcome1','outcome2'])
        self.counter = 0

    def execute(self, userdata):
        rospy.loginfo('Executing state FOO')
        if self.counter < 3:
            self.counter += 1
            return 'outcome1'
        else:
            return 'outcome2'


# define state Bar
class Bar(smach.State):
    def __init__(self):
        smach.State.__init__(self, outcomes=['outcome2'])

    def execute(self, userdata):
        rospy.loginfo('Executing state BAR')
        return 'outcome2'
        
# main
def main():
    rospy.init_node('smach_example_state_machine')

    # Create a SMACH state machine
    sm = smach.StateMachine(outcomes=['outcome4', 'outcome5'])

    # Open the container
    with sm:
        # Add states to the container
        smach.StateMachine.add('FOO', Foo(), 
                               transitions={'outcome1':'BAR', 
                                            'outcome2':'outcome4'})
        smach.StateMachine.add('BAR', Bar(), 
                               transitions={'outcome2':'FOO'})

    # Execute SMACH plan
    outcome = sm.execute()

if __name__ == '__main__':
    main()
```
1. 创建两个状态。
2. main函数中初始化节点，然后初始化状态机，并获取状态机最终输出结果

#### 拓展说明
1. smatch自带一些默认状态类
2. smatch还自带一些默认状态机容器
这些将在之后的部分中说明


### ***在状态之间传递数据***
#### 指定用户数据
1. 状态之间需要数据传递，这种数据叫用户数据，构造状态类的时候，初始化需要设置用户数据。
2. input_keys列表枚举所有的状态需要运行的输入。不能写入，保持不变。
3. output_keys列表枚举所有的状态提供输出。可以写入。
```
  class Foo(smach.State):
     def __init__(self, outcomes=['outcome1', 'outcome2'],
                        input_keys=['foo_input'],
                        output_keys=['foo_output'])

     def execute(self, userdata):
        # Do something with userdata
        if userdata.foo_input == 1:
            return 'outcome1'
        else:
            userdata.foo_output = 3
            return 'outcome2'
```
![smatch_simple](images/user_data_single.png '示例') 

#### 连接用户数据
1. 在初始化状态机容器的时候，除了设置输入输出连接，还要通过状态机数据建立映射。
```
sm_top = smach.StateMachine(outcomes=['outcome4','outcome5'],
                          input_keys=['sm_input'],
                          output_keys=['sm_output'])
  with sm_top:
     smach.StateMachine.add('FOO', Foo(),
                            transitions={'outcome1':'BAR',
                                         'outcome2':'outcome4'},
                            remapping={'foo_input':'sm_input',
                                       'foo_output':'sm_data'})
     smach.StateMachine.add('BAR', Bar(),
                            transitions={'outcome2':'FOO'},
                            remapping={'bar_input':'sm_data',
                                       'bar_output1':'sm_output'})
```

2. 注意，这里重映射的'x':'y'中，x为状态数据，y为状态机数据。（状态机数据可以是输入输出，也可以是中间数据）
3. 如果状态数据和状态机数据名字一样，可以不用重新映射，但是最好写上，比如'x':'x'
4. 两个状态的输入输出数据通过重新映射，通过同一个状态机中间数据实现了连接。
```
FOO: remapping={'foo_output':'sm_user_data'}
BAR: remapping={'bar_input':'sm_user_data'}
```
注意此处的sm_user_data也需要定义一下，比如：
```
sm.userdata.sm_user_data = 0
```
5. 当然状态数据也可以和状态机输入输出数据连接。
```
BAR: remapping={'bar_output':'sm_output'}
FOO: remapping={'foo_input':'sm_input'}
```
![smatch_simple](images/user_data.png '示例')

```
#!/usr/bin/env python

import roslib; roslib.load_manifest('smach_tutorials')
import rospy
import smach
import smach_ros

# define state Foo
class Foo(smach.State):
    def __init__(self):
        smach.State.__init__(self, 
                             outcomes=['outcome1','outcome2'],
                             input_keys=['foo_counter_in'],
                             output_keys=['foo_counter_out'])

    def execute(self, userdata):
        rospy.loginfo('Executing state FOO')
        if userdata.foo_counter_in < 3:
            userdata.foo_counter_out = userdata.foo_counter_in + 1
            return 'outcome1'
        else:
            return 'outcome2'

# define state Bar
class Bar(smach.State):
    def __init__(self):
        smach.State.__init__(self, 
                             outcomes=['outcome1'],
                             input_keys=['bar_counter_in'])
        
    def execute(self, userdata):
        rospy.loginfo('Executing state BAR')
        rospy.loginfo('Counter = %f'%userdata.bar_counter_in)        
        return 'outcome1'

def main():
    rospy.init_node('smach_example_state_machine')

    # Create a SMACH state machine
    sm = smach.StateMachine(outcomes=['outcome4'])
    sm.userdata.sm_counter = 0

    # Open the container
    with sm:
        # Add states to the container
        smach.StateMachine.add('FOO', Foo(), 
                               transitions={'outcome1':'BAR', 
                                            'outcome2':'outcome4'},
                               remapping={'foo_counter_in':'sm_counter', 
                                          'foo_counter_out':'sm_counter'})
        smach.StateMachine.add('BAR', Bar(), 
                               transitions={'outcome1':'FOO'},
                               remapping={'bar_counter_in':'sm_counter'})


    # Execute SMACH plan
    outcome = sm.execute()


if __name__ == '__main__':
    main()
```

