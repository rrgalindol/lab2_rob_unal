# Laboratory 2 - Phantom X - ROS

## Forward Kinematics

### Model and measurements
Robot Phantom X <br>
[![Captura-de-pantalla-de-2022-05-13-10-22-16.png](https://i.postimg.cc/jd7TsXz0/Captura-de-pantalla-de-2022-05-13-10-22-16.png)](https://postimg.cc/bspWg1C3)
### DH Parameters
| Link | alpha |  a |  d | theta | offset |
|:----:|:-----:|:--:|:--:|:-----:|--------|
| 1    | 90°   | 0  | L1 | q1    | 0      |
| 2    | 0     | L2 | 0  | q2    | 90°    |
| 3    | 0     | L3 | 0  | q3    | 0      |
| 4    | 0     | L4 | 0  | q4    | 0      |
## ROS
We've created a python script which moves each of the joints between two positions, one home position and one objective position. These postions were defined as follows:
- Waist
  - Home: 0° (512 bits)
  - Objective: 32,6° (400 bits)
- Shoulder
  - Home: 0° (512 bits)
  - Objective: 32,6° (400 bits)
- Elbow
  - Home: 0° (512 bits)
  - Objective: 32,6° (400 bits)
- Wrist
  - Home: 0° (512 bits)
  - Objective: 32,6° (400 bits)


This script is located in the scripts folder of the px_robot package in this repository with the name ***boardOperation.py***. When running the script the Phantom X can be operated in the next way:
- You can tell the program which joint you want to move. In the console, the name of the operating joint is printed.
  - With the 'W' key you go to the next joint (if you are in Waist you go to Shoulder ; if you are in shoulder you go to elbow and so on).
  - With the 'S' key you go to the previous articulation (if you are in wrist you go to elbow and so on).
  - It can be done cyclically, that is, the next joint to Wrist is Waist; and that the one before Waist is Wrist.
- Pressing the 'D' key brings the operated joint to the target position.
- Pressing the 'A' key should bring the operated joint to the home position.

For the script to work first we have to run any of the next ROS launch files:
- px_controllers.launch - allows to move the robot
- px_rviz_dyna.launch - allows to move the robot but also creates a virtual visualization on rviz
Then we can run and use the script:
``` 
roslaunch px_robot px_rviz_dyna.launch
cd catkin_ws/src/px_robot/scripts
python boardOperation.py
```

### Code explanation
### Video

## Toolbox
We used the Peter Corke Toolbox in order to define and visualize a plot of de robot's model. First we define each of the Links with the function ***Link***, their inputs are the DH parameters that we defined in a previous section. With these Links created, then we create the robot by using the ***Serial Link*** function:

``` matlab
l = [14.5, 10.7, 10.7, 9]; % Links lenght
% Robot Definition RTB
L(1) = Link('revolute','alpha',pi/2,'a',0,   'd',l(1),'offset',0,   'qlim',[-3*pi/4 3*pi/4]);
L(2) = Link('revolute','alpha',0,   'a',l(2),'d',0,   'offset',pi/2,'qlim',[-3*pi/4 3*pi/4]);
L(3) = Link('revolute','alpha',0,   'a',l(3),'d',0,   'offset',0,   'qlim',[-3*pi/4 3*pi/4]);
L(4) = Link('revolute','alpha',0,   'a',0,   'd',0,   'offset',0,   'qlim',[-3*pi/4 3*pi/4]);
PhantomX = SerialLink(L,'name','Px');
```

Now, we have to orientate the TCP in the correct way:

``` matlab
% Tool orientation
PhantomX.tool = [0 0 1 l(4); -1 0 0 0; 0 -1 0 0; 0 0 0 1];
```
Thus, the HTM of the system from the base to the tool is: 


With the model defined, we can start to play with it. We can use the ***.teach*** function to plot the robot and move their Links with sliders.
``` matlab
PhantomX.teach()
```
Now we are going to plot some positions of the Robot with the ***.plot*** function, which arguments are the Links' values q:
- Home position q = [0 0 0 0] <br>
[![Captura-de-pantalla-de-2022-05-13-11-33-52.png](https://i.postimg.cc/ZnnXf1kg/Captura-de-pantalla-de-2022-05-13-11-33-52.png)](https://postimg.cc/dZMW3NVm)
- q = [30° 45° -30° 70°]  <br>
[![Captura-de-pantalla-de-2022-05-13-11-36-38.png](https://i.postimg.cc/9QL8kLbH/Captura-de-pantalla-de-2022-05-13-11-36-38.png)](https://postimg.cc/rDtN0C7f)
- q = [90° -20° -70° 15°]  <br>
[![Captura-de-pantalla-de-2022-05-13-11-37-55.png](https://i.postimg.cc/76k2nkzH/Captura-de-pantalla-de-2022-05-13-11-37-55.png)](https://postimg.cc/GBXtb6zV)

## Matlab conection

With Matlab we can create a script that moves each of the joints of the Phantom X. First we have to start the conection between Matlab an ROS with ***rosinit*** to be able to use the Services of the dynamixel motors of the robot. Then we create a client for the service ****dynamixel_workbench/dynamixel_command**** and the message for the service. And Finally we can call the service with the parameters that we want:
``` matlab
%%
rosinit
%%
motorSvcClient = rossvcclient('dynamixel_workbench/dynamixel_command');%Client creation
motorCommandMsg = rosmessage(motorSvcClient);%Message creation
%%
motorCommandMsg.AddrName = "Goal_Position";
motorCommandMsg.Id = 1;
motorCommandMsg.Value = 400;
call(motorSvcClient,motorCommandMsg); %Service call
```


## Matlab + ROS + Toolbox

## Conclusions
