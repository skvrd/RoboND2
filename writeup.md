## Project: Kinematics Pick & Place
---

**Steps to complete the project:**  

1. Set up your ROS Workspace.
2. Download or clone the [project repository](https://github.com/udacity/RoboND-Kinematics-Project) into the ***src*** directory of your ROS Workspace.  
3. Experiment with the forward_kinematics environment and get familiar with the robot.
4. Launch in [demo mode](https://classroom.udacity.com/nanodegrees/nd209/parts/7b2fd2d7-e181-401e-977a-6158c77bf816/modules/8855de3f-2897-46c3-a805-628b5ecf045b/lessons/91d017b1-4493-4522-ad52-04a74a01094c/concepts/ae64bb91-e8c4-44c9-adbe-798e8f688193).
5. Perform Kinematic Analysis for the robot following the [project rubric](https://review.udacity.com/#!/rubrics/972/view).
6. Fill in the `IK_server.py` with your Inverse Kinematics code. 


[//]: # (Image References)

[image1]: ./kuka_arm.png
[image2]: ./link_assigments.png
[image3]: ./matrix.png
[image4]: ./theta1.jpg
[image5]: ./theta2-3.png


## [Rubric](https://review.udacity.com/#!/rubrics/972/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Kinematic Analysis
#### 1. Run the forward_kinematics demo and evaluate the kr210.urdf.xacro file to perform kinematic analysis of Kuka KR210 robot and derive its DH parameters.

Here is an forward kinematics demo screenshot. 

![FWD kinematics demo screenshot][image1]

Here is a DH parameters table

i | alpha(i-1) | a(i-1) | d(i) | theta(i)
--- | --- | --- | --- | ---
1| 0 | 0 | 0.75 |theta(i)
2 | - pi/2 | 0.35 | 0 | -pi/2 + theta(i)
3 | 0 | 1.25 | 0 | theta(i)
4 | - pi/2 | -0.054 | 1.5 | theta(i)
5 | pi/2 | 0 | 0 | theta(i)
6 | - pi/2 | 0 | 0 | theta(i)
7 or g(reeper) | 0 | 0 | 0.303 | 0


##### Link assigments 
![Link assigments][image2]

#### 2. Using the DH parameter table you derived earlier, create individual transformation matrices about each joint. In addition, also generate a generalized homogeneous transform between base_link and gripper_link using only end-effector(gripper) pose.

##### Individual transformation matrices:
##### T0_1
[[cos(q1), -sin(q1), 0, 0],
[sin(q1), cos(q1), 0, 0],
[0, 0, 1, 0.75],
[0, 0, 0, 1]]
##### T1_2
[[cos(q2 - 0.5\*pi), -sin(q2 - 0.5\*pi),
0, 0.35], [0, 0, 1, 0],
[-sin(q2 - 0.5\*pi),
-cos(q2 - 0.5\*pi), 0, 0],
[0, 0, 0, 1]]
##### T2_3
[[cos(q3), -sin(q3), 0, 1.25],
[sin(q3), cos(q3), 0, 0],
[0, 0, 1, 0],
[0, 0, 0, 1]]
##### T3_4
[[cos(q4), -sin(q4), 0, -0.054],
[0, 0, 1, 1.50], 
[-sin(q4), -cos(q4), 0, 0],
[0, 0, 0, 1]]
##### T4_5
[[cos(q5), -sin(q5), 0, 0], 
[0, 0, -1, 0], 
[sin(q5), cos(q5), 0, 0], 
[0, 0, 0, 1]]
##### T5_6
[[cos(q6), -sin(q6), 0, 0], 
[0, 0, 1, 0], 
[-sin(q6), -cos(q6), 0, 0], 
[0, 0, 0, 1]]
##### T6_G
[[1, 0, 0, 0], 
[0, 1, 0, 0], 
[0, 0, 1, 0.303], 
[0, 0, 0, 1]]

##### Rotation part (RT) of the complete homogeneous transform:
[[cos(p)*cos(y), sin(p)*sin(r)*cos(y) - sin(y)*cos(r), sin(p)*cos(r)*cos(y) + sin(r)*sin(y)], [sin(y)*cos(p), sin(p)*sin(r)*sin(y) + cos(r)*cos(y), sin(p)*sin(y)*cos(r) - sin(r)*cos(y)], [-sin(p), sin(r)*cos(p), cos(p)*cos(r)]]

##### Here is how complete homogeneous transform will looks like:
![complete homogeneous transform][image3]

#### 3. Decouple Inverse Kinematics problem into Inverse Position Kinematics and inverse Orientation Kinematics; doing so derive the equations to calculate all individual joint angles.

Since the last three joints in our robot are revolute and their joint axes intersect at a single point, we have a case of spherical wrist with joint_5 being the common intersection point and hence the wrist center.

This allows us to kinematically decouple the IK problem into Inverse Position and Inverse Orientation problems as discussed in the Inverse Kinematics theory lesson.

Here is supporting visuals for theta 1
![theta1][image4]

And here are supporting visuals for theta 2 and 3
![theta1][image4]

theta 4 5 and 6 can be found using Euler angles from rotation matrix

More math inside the code.

### Project Implementation

#### 1. Fill in the `IK_server.py` file with properly commented python code for calculating Inverse Kinematics based on previously performed Kinematic Analysis. Your code must guide the robot to successfully complete 8/10 pick and place cycles. Briefly discuss the code you implemented and your results. 


Code inside `IK_server.py` file.



