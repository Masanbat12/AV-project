# 🚗 Autonomous Vehicle Control System
## Lane Following • FSM Decision Making • PID Control
A modular autonomous driving system built with ROS, OpenCV, and Finite State Machines, designed for lane following, stop-sign handling, and autonomous maneuvering.

### 📌 Overview
This project implements an autonomous vehicle controller that combines:
- Computer Vision for lane and sign detection
- Finite State Machine (FSM) for decision-making
- PID Control for smooth and stable driving
The system is designed to be modular, extensible, and simulation-ready, making it suitable for academic projects, autonomous driving simulations, and future research extensions.

### 🧠 System Architecture
The system is divided into three logical layers:
1️⃣ Perception Layer
- Lane detection using OpenCV
- Red stop-sign detection via color thresholding
- Camera-based input processing

2️⃣ Decision Layer (Finite State Machine)
- Manages vehicle behavior
- Handles state transitions based on perception and timing

3️⃣ Control Layer
- PID controller for steering
- Speed control based on the current FSM state


### 🔄 Finite State Machine (FSM)
graph TD
    FOLLOW_LANE["FOLLOW_LANE<br/>PID lane keeping"]
    STOP["STOP<br/>Stop for 3 seconds"]
    OVERTAKE["OVERTAKE<br/>Avoid obstacle"]
    RETURN["RETURN<br/>Merge back to lane"]

    FOLLOW_LANE -->|Red Sign Detected| STOP
    STOP -->|Timer 3s| OVERTAKE
    OVERTAKE -->|Timer Expired| RETURN
    RETURN -->|Merge Completed| FOLLOW_LANE

### 🧩 State Description
- FOLLOW_LANE – Default driving mode using PID-based lane keeping
- STOP – Vehicle stops for a fixed duration after detecting a red sign
- OVERTAKE – Executes avoidance maneuver
- RETURN – Safely merges back into the lane

### 📂 Project Structure

src/
├── cv_lane_follower/

│   ├── scripts/

│   │   └── smart_driver.py      # FSM + PID main controller

│   ├── launch/

│   │   └── autorace.launch      # ROS launch file (optional)

│   └── CMakeLists.txt

└── README.md


### 🐍 Main Module
smart_driver.py

Responsibilities:

- Lane detection pipeline

- FSM logic and state transitions

- PID steering and speed control

- Timing logic for stop and maneuver states

The script is written to allow easy tuning and extension.

### ⚙️ Dependencies
- ROS (Noetic recommended)
- Python 3
- OpenCV

## 🎯 Key Features

- 🚘 **Stable lane following** using PID control for smooth and accurate steering  
- 🛑 **Stop-sign detection** with timed stopping behavior  
- 🔁 **Autonomous maneuvering** and safe lane return  
- 🧠 **FSM-based modular decision logic** for clear and maintainable behavior control  
- 🧩 **Easily extensible architecture** for adding new driving behaviors  

---

## 🧪 Debugging & Tuning

- PID parameters can be tuned directly in `smart_driver.py`  
- Each FSM state is isolated, allowing focused debugging and testing  
- Console logs indicate the active state and state transitions in real time  

---

## 🚀 Future Improvements

- Traffic light detection and handling  
- Pedestrian and obstacle classification  
- Dynamic speed adaptation based on environment  
- Sensor fusion (LiDAR / depth camera integration)  
- Reinforcement learning-based decision layer  

---

## ⭐ Why This Project Matters

This project demonstrates:

- Real-world autonomous driving architecture  
- Clean and structured FSM-based decision making  
- Practical application of PID controllers  
- Readable, maintainable, and scalable ROS code structure  


## Development Environment

This project was developed and tested using **The Construct**, a cloud-based
ROS development and simulation platform.  
The platform enabled running, debugging, and validating the system without
local installation, using a Linux-based environment with ROS and Gazebo.

### סביבת פיתוח

הפרויקט פותח והורץ באמצעות פלטפורמת **The Construct**, סביבת פיתוח וסימולציה
מבוססת ענן לפרויקטים של ROS, המאפשרת עבודה עם Gazebo ולינוקס ללא התקנה מקומית.


## Simulation Demo
Screenshots and simulations were executed using The Construct platform.

<img width="174" height="111" alt="image" src="https://github.com/user-attachments/assets/68563bb2-fd00-4898-ac05-e48fb3075871" />

<img width="143" height="67" alt="1112026-01-21 053015" src="https://github.com/user-attachments/assets/c68e271d-20fc-4280-a82e-fa2cb944d70c" />
<img width="140" height="69" alt="222 2026-01-21 053042" src="https://github.com/user-attachments/assets/a885f2e6-7c76-4c7e-84c6-e60fe328d912" />
