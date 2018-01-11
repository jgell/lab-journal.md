# The Robot Arm project

## Part 1 - Servo control



###*Servo motor circuit diagram:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/Servo%20motor%20design.JPG

### Operation

The first step is to control a servo motor with arduino so that its movement follows a sine wave of frequency 0.2Hz.

*Code:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/ros%20lab%20screenshot%201.jpeg


## Part 2 - Robot Arm

### Solidworks design

*Solidworks screenshots:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/arm1.JPG

https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/arm2.JPG


### Getting started with ROS

With ROS setup on the system, I can control the servo motors using the terminal, and later with rviz.

Using the given code sample:

*Code link:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/Controlling%20servos%20manually.txt

And using in terminal: > rosrun rosserial_python serial_node.py /dev/ttyACM0
I can control the servo motor's angle using a ros command: rostopic pub. The parameter needed is:--once servo std_msgs/UInt16 90 (to move 90 degrees, for example).

*Controlling with terminal:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/controllingservo_%20rostopicpub.png

The function defined at line 10 'cb' is a topic that allows the user to write a number to terminal using rosparam, that represents the position they want the servo to move to.
The object instantiated at line 14 reads what the user has input to terminal and calls the cb function

### 3D modelling the Arm

Our Arm has three degrees of freedom in a simple design. The first servo motor control the direction- attached to a ground plate, it rotates the rest of the arm 180 degrees. The second servo motor corresponds to the first joint- the 'shoulder'. It rotates the next segment 180 degrees. The third servo rotates the last segment 180 degrees in such as way that the arm can fold in on itself. 

*URDF for Arm:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/armURDF.txt

### Controlling the Arm:

Connceting the three servos up to the arduino, it is now possible to use the joint_state sliders to interface with the servos. We first tried controlling the URDF model on rviz.
The arm on rviz can be controlled with the joint-state publisher node- sliders control each servo position.

*rviz image and video:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/rvizinitial.png

https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/20180110_141118000_iOS.MOV

To control the arm proper, it is neccessary to input the following ros commands:

> rosparam set robot_description -t models/robot-arm.urdf**


This tells ros that the urdf file held in the 'models' directory will be used to describe the robot. Rviz will therefore be able to load this.

> rosrun robot_state_publisher robot_state_publisher


Starts the node to let us control the robot state.

> rosrun joint_state_publisher joint_state_publisher _use_gui:=true


Starts the joint_state_publisher node with a gui that has sliders to control servos

Now to create code that will enable us to control the arm servos themselves. We realised we would have to setup more servos in the code as we are now working with three. I have named them after the URDF servo names. We had to adjust the int angle for each servo to match the degrees covered by each servo in the URDF. For example, one of our servos has a range of 3.14-6.28 radians (i.e. 180-360 degrees). This needed to be converted to 0-180 in the code.

*Code:*
https://github.com/rdgrainger/roco222-journal/blob/master/DC-Motor-Project%20resources/Arm%20control.txt

> msg.position[2] * 180/6.28

converts to the correct range.

//reference robgrainger(lab partner)
