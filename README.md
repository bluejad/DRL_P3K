# DRL_P3K
一、实验目的

使用深度强化学习控制移动机器人在复杂环境中避障、收集物品到指定点。所用到的算法包括DQN、Deuling-DDQN、A3C、DDPG、NAF。

二、环境和模型

1. 实验环境：

ROS、Gazebo

2. 移动机器人模型

安装有Kinect的Pioneer3移动机器人

三、训练

机器人从Kinect获取State，通过reward训练出合适的Action。

问题：

在Atari或Mujoco环境下训练网络时，可以读取一帧State然后暂停模拟器，训练一次网络后再启动模拟器执行Action。而在ROS/Gazebo或者真实环境下不可能暂停，控制器和传感器时刻都在变化，所以对实时性要求非常高，也就是说当你读取一帧State，训练一次网络后还未执行Action，此时的State已经发生了变化，这就不满足强化学习的马尔可夫性了。
在Atari或Mujoco里可以对环境变化的模拟进行加速。而ROS/Gazebo或者真实环境下不可能做到，这就造成了收集样本的速度非常慢，想训练好一个网络势必会比在Atari或Mujoco花费更多的时间。
解决方案：

多移动机器人异步训练。（相同思想的论文Deep Reinforcement Learning for Robotic Manipulation with Asynchronous Off-Policy Updates）

即，多个移动机器人（collector threads）负责收集样本，一个training thread负责训练网络。

四、实验视频

移动机器人需要避开障碍物（或其他机器人）同时收集绿色的方块到达出口。



部分代码：https://github.com/DajunZhou/DRL_P3K

视频网站：http://m.blog.csdn.net/u013236946/article/details/72997797
