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


This script is located in the scripts folder of the px_robot package in this repository with the name *_boardOperation.py_*. When running the script the Phantom X can be operated in the next way:
- You can tell the program which joint you want to move. In the console, the name of the operating joint is printed.
  - With the 'W' key you go to the next joint (if you are in Waist you go to Shoulder ; if you are in shoulder you go to elbow and so on).
  - With the 'S' key you go to the previous articulation (if you are in wrist you go to elbow and so on).
  - It can be done cyclically, that is, the next joint to Wrist is Waist; and that the one before Waist is Wrist.
- Pressing the 'D' key brings the operated joint to the target position.
- Pressing the 'A' key should bring the operated joint to the home position.



## Matlab

## Conclusions
