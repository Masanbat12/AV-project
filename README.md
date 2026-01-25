# HIT - Intelligent Autonomous Vehicle Software Development
## Autonomous Lane Keeping & Traffic Sign Recognition System Course: Software Engineering for Autonomous Vehicles
The robot is capable of:
1.  **Lane Keeping:** robustly following yellow and white lane markers.
2.  **Traffic Sign Recognition:** detecting red stop signs.
3.  **Adaptive Maneuvering:** analyzing the sign's position to decide whether to overtake from the left or right.
4.  **State Management:** handling stops, overtaking, and returning to the lane autonomously.

## 🏗️ System Architecture

The system operates on a **Perception-Decision-Control** pipeline loop running at approximately 30Hz.

### 1. Perception Layer (Computer Vision)
Input data is received from the `/camera/rgb/image_raw` topic. The image processing pipeline includes:
* **ROI (Region of Interest):** cropping the top 45% of the frame to remove background noise (sky, horizon).
* **Color Space Conversion:** converting BGR images to **HSV** (Hue, Saturation, Value) to isolate yellow and white colors regardless of lighting conditions.
* **Histogram Analysis:** Instead of simple blob tracking, the system computes a vertical histogram of pixel intensity. The peaks (`argmax`) of the histogram represent the lane centers.
* **Dynamic Lane Estimation:** If one lane is missing (e.g., dashed lines), the system extrapolates the center based on the remaining visible lane and a fixed offset.

graph TD
    FOLLOW_LANE["FOLLOW_LANE<br/>PID lane keeping"]
    STOP["STOP<br/>Wait 3 seconds"]
    OVERTAKE["OVERTAKE<br/>Avoid obstacle"]
    RETURN["RETURN<br/>Merge back"]

    FOLLOW_LANE -->|Red Sign Detected| STOP
    STOP -->|Timer 3s| OVERTAKE
    OVERTAKE -->|Timer Expired| RETURN
    RETURN -->|Merge Done| FOLLOW_LANE


STOP: Triggered when the red mask area > 3000px. Stops the robot for 3 seconds.

OVERTAKE: Executes an open-loop maneuver (linear + angular velocity) to bypass the obstacle. Logic: If the sign is on the left, the robot overtakes right, and vice versa.

RETURN: A counter-maneuver to realign the robot with the track.

3. Control Layer (PID)
To ensure smooth lane centering, a Proportional-Derivative (PD) controller is implemented:

Error Calculation: Error = (Lane Center) - (Image Center)

Control Output: Angular Velocity = -Kp * Error

Adaptive Speed: Linear velocity is dynamically adjusted based on the error magnitude (slows down in sharp turns, speeds up in straights).

## 📂 File Structure

src/
├── cv_lane_follower/
│ ├── scripts/
│ │ └── smart_driver.py # Main logic node
│ ├── launch/
│ │ └── autorace.launch # Launch file (optional)
│ └── CMakeLists.txt
└── README.md


🚀 How to Run
Prerequisites
ROS (Kinetic or Noetic) installed.

TurtleBot3 Simulations packages.

Gazebo Simulator.

Execution Steps
Launch the Simulation:
export TURTLEBOT3_MODEL=burger
roslaunch turtlebot3_gazebo turtlebot3_autorace.launch

Run the Autonomous Driver:
rosrun cv_lane_follower smart_driver.py

🧠 Algorithms & Code Highlights
Histogram Peak Detection
The core lane detection uses Numpy to sum pixel values vertically:

histogram = np.sum(mask_road[mask_road.shape[0]//2:, :], axis=0)
left_base = np.argmax(histogram[:midpoint])
right_base = np.argmax(histogram[midpoint:]) + midpoint

This method is more robust than finding contours as it naturally filters out small noise.

Smart Overtaking Decision
The robot decides the direction dynamically based on the obstacle's centroid:

M = cv2.moments(mask_red)
sign_cx = int(M['m10'] / M['m00'])

if sign_cx < lane_center:
    self.overtake_dir = -1  # Sign is on Left -> Go Right
else:
    self.overtake_dir = 1   # Sign is on Right -> Go Left

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
