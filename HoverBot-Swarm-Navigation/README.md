# HoverBot | Computer Vision-Based Robot Localization and Navigation

**McGill University | Electrical Engineering Capstone Design | 2024–2025**

**Python | OpenCV | NumPy | ESP32 | Computer Vision | REST API | TCP/IP | Robotics**

[← Back to Main Capstone Repository](../README.md)

---

## 1. Project Overview

The HoverBot project focused on developing a computer-vision-based localization and navigation system for an existing robotic platform within McGill University's Swarm Robotics project.

The original robotic platform supported movement through commands sent over a network using an existing TCP server and REST API. However, it lacked a centralized navigation system capable of continuously tracking the robot's position, calculating movement decisions and guiding it toward a user-selected destination.

Our four-member capstone team developed a Python-based navigation system using OpenCV to process a live camera feed, estimate the HoverBot's position and orientation and generate movement commands based on its current location relative to a destination.

The navigation system communicated with the existing REST API and Java-based TCP server, allowing calculated movement instructions to reach the ESP32-based robot controller.

The project combined computer vision, geometric transformations, robot localization, navigation algorithms, embedded hardware integration, communication protocols and extensive troubleshooting.

## 2. Project Objectives

The primary objectives were to:

1. Understand and troubleshoot the inherited robotic hardware and software infrastructure.
2. Develop a centralized camera-based localization system.
3. Detect and track the HoverBot's position and orientation in real time.
4. Correct camera perspective distortion to improve spatial measurements.
5. Allow users to select destinations through an interactive camera feed.
6. Develop navigation logic to determine appropriate robot movements.
7. Integrate movement commands with the existing REST API and TCP server.
8. Build a modular foundation that could support additional robots in future development.

Although the wider project involved multiple robotic platforms, the completed navigation implementation focused on the HoverBot due to hardware limitations and the available development time.

## 3. Technologies and Tools

| Category | Technologies and Concepts |
|---|---|
| Programming | Python, existing C++ and Java infrastructure |
| Computer Vision | OpenCV |
| Numerical Computing | NumPy |
| Embedded Hardware | ESP32 microcontroller |
| Communication | HTTP, REST API, TCP/IP |
| Server Infrastructure | Existing Java-based TCP server |
| Camera Processing | Live video acquisition, perspective transformation |
| Object Detection | HSV color thresholding, contour detection |
| Robot Localization | Marker tracking, position and orientation estimation |
| Navigation | Heading calculation, turn-and-move control |
| Development and Testing | Python environment, hardware testing, system integration and debugging |

## 4. Existing Robotic Platform

The HoverBot was inherited from previous McGill engineering teams and included both hardware and software systems that had already been developed.

The hardware architecture included:

- An ESP32-based control unit
- Motor controllers and motor-control interfaces
- Transistor-based control circuitry
- A power distribution system using DC-DC conversion
- Electrical connections for braking and directional control

The existing software infrastructure included ESP32 firmware, a Java TCP server and an HTTP-based REST API for sending commands to connected robots.

Before integrating the navigation system, our team had to become familiar with the existing hardware, verify electrical connections, troubleshoot communication issues and establish reliable movement control.

### System Architecture

```text
      External Camera / Video Feed
                  |
                  v
        Python + OpenCV System
                  |
                  v
       Perspective Correction
                  |
                  v
       Color-Based Detection
                  |
                  v
       Robot Pose Estimation
       (Position + Orientation)
                  |
                  v
        Navigation Algorithm
                  |
                  v
          HTTP / REST API
                  |
                  v
          Java TCP Server
                  |
                  v
       HoverBot ESP32 Controller
                  |
                  v
           Motor Controllers
                  |
                  v
           Robot Movement
```

The computer vision and navigation calculations run on an external computer rather than directly on the robot's ESP32.

This architecture separates high-level navigation from low-level motor control and provides a foundation for integrating additional robotic platforms.

## 5. Computer Vision and Localization

A major technical component involved developing a reliable computer vision pipeline to identify the HoverBot and determine its position and orientation using an external camera.

The system processes live video frames, applies perspective correction, detects colored markers and calculates the robot's position and heading.

### 5.1 Camera Acquisition and Perspective Correction

An external camera captures the robot's operating area.

Because the camera may observe the floor from an angle, the original image can contain perspective distortion that affects distance and direction calculations.

To address this, the system implements a four-point perspective transformation.

During calibration, the user identifies four corners of a rectangular area with known dimensions.

OpenCV calculates the corresponding transformation matrix and applies it to incoming video frames to generate a corrected top-down representation.

Relevant OpenCV functions include:

- `cv2.getPerspectiveTransform()`
- `cv2.warpPerspective()`

The corrected view provides a more consistent coordinate system for localization and navigation.

### 5.2 HSV Color-Based Marker Detection

The HoverBot uses two distinguishable visual markers, pink and green, to support position and orientation estimation.

The system converts captured frames from BGR to HSV color space.

HSV-based processing was selected because it allows color filtering to be adjusted more effectively for the lighting conditions in the operating environment.

The marker detection process involves:

1. Capturing a frame from the live video feed.
2. Converting the image from BGR to HSV.
3. Applying calibrated HSV thresholds.
4. Generating binary masks for the target colors.
5. Detecting contours within the masks.
6. Identifying the relevant marker contours.
7. Calculating the center coordinates of each detected marker.

Relevant OpenCV functions include:

- `cv2.cvtColor()`
- `cv2.inRange()`
- `cv2.findContours()`
- `cv2.boundingRect()`

A calibration process allows the user to select reference regions for the marker colors, helping adapt detection to the operating environment.

### 5.3 Robot Position Estimation

After detecting the two markers, the system calculates their center coordinates.

The robot's estimated position is defined as the midpoint between the two marker centers.

This provides a reference point representing the robot's location within the corrected camera frame.

### 5.4 Robot Orientation Estimation

In addition to determining position, the navigation system must estimate the direction in which the robot is facing.

The relative positions of the pink and green markers are used to construct orientation vectors.

Using NumPy and geometric calculations, the system determines the robot's forward direction and calculates its orientation angle.

The `atan2()` function is used for angle estimation.

Using two markers allows the system to distinguish the robot's heading rather than simply locating it as a single point.

### 5.5 Real-Time Visualization

The computer vision system displays processed video frames with visual annotations that support navigation and debugging.

These annotations include:

- Detected marker locations
- Estimated robot bounding box
- Robot position and heading
- Selected destination
- Direction indicators
- Navigation-related information

This visualization allows users to observe how the system interprets the robot's position and makes movement decisions.

## 6. Navigation Algorithm

The navigation system enables a user to select a target destination by clicking within the processed camera feed.

Once a destination is selected, the algorithm uses the robot's estimated position and orientation to calculate movement decisions.

### Navigation Process

1. Detect the robot's current position and orientation.
2. Obtain the destination coordinates from the user's input.
3. Calculate the desired heading toward the destination.
4. Compare the desired heading with the robot's current orientation.
5. Determine whether the robot should turn left, turn right or move forward.
6. Send the appropriate movement instruction through the REST API.
7. Continue updating the robot's position using incoming camera frames.
8. End navigation when the robot reaches the defined destination threshold.

### Heading Calculation

The navigation algorithm calculates a heading difference:

**Heading Error = Desired Heading − Current Robot Heading**

The heading error determines the required motion.

- Larger heading differences result in turning commands.
- Moderate heading differences produce smaller orientation adjustments.
- When the robot is sufficiently aligned with the destination, forward movement is commanded.

The final implementation uses a turn-then-move strategy with empirically adjusted movement parameters.

### Navigation Threading

Navigation commands run in a separate thread from the main computer vision processing loop.

This allows camera processing and visualization to continue while movement instructions are issued.

Separating these tasks helps prevent command execution from blocking the video interface.

## 7. REST API and TCP Server Integration

The project integrated the newly developed navigation application with the robot's existing communication infrastructure.

The Python navigation system communicates with the REST API using HTTP requests.

Movement instructions are forwarded through the Java TCP server to the ESP32-based HoverBot controller.

### Communication Flow

```text
      Python Navigation Application
                   |
                   | HTTP POST / JSON
                   v
                REST API
                   |
                   v
              TCP Server
                   |
                   | TCP/IP
                   v
           HoverBot ESP32
                   |
                   v
           Motor Controllers
                   |
                   v
             Robot Movement
```

### Movement Commands

The navigation system uses commands such as:

- Forward
- Turn left
- Turn right

The requests include the intended robot identifier and the appropriate movement parameter.

This arrangement allows the navigation application to issue higher-level movement instructions without directly implementing the motor-control functions.

## 8. Engineering Challenges and Troubleshooting

### Challenge 1: Understanding an Inherited System

The HoverBot platform contained existing hardware, firmware and server software developed by previous teams.

The available documentation was not always complete or consistent with the current system configuration.

Our team had to investigate the existing architecture before implementing new functionality.

This involved:

- Reviewing previous technical documentation.
- Examining existing software components.
- Understanding ESP32 and motor-control interfaces.
- Verifying the existing TCP server and REST API.
- Testing basic robot movement commands.
- Troubleshooting hardware and communication problems.

This process was essential for establishing a usable development platform.

### Challenge 2: Hardware and Electrical Reliability

Hardware-related problems affected the robot's operation during development.

The team investigated electrical connections, motor-control behavior and the power distribution system.

Testing included checking the ESP32 interface, verifying grounding and power connections and observing how individual control signals affected robot movement.

Hardware troubleshooting and repairs were necessary before the navigation system could be tested reliably.

### Challenge 3: Camera Perspective Distortion

Camera placement introduced geometric distortion in the recorded operating area.

Without correction, distances and directions measured directly from image coordinates could be inaccurate.

The team implemented a four-point perspective transformation to produce a more consistent top-down view of the operating area.

### Challenge 4: Lighting Conditions and Marker Detection

The lighting conditions in the laboratory complicated object identification.

Simple grayscale thresholding was not reliable enough to separate the robot from its surroundings.

The system therefore used HSV color-based detection, marker calibration and contour analysis to improve tracking.

### Challenge 5: Navigation and Communication Integration

Developing computer vision functionality alone was not sufficient.

The calculated movement instructions also needed to be compatible with the existing server and robot-control system.

The team integrated localization outputs with navigation logic, HTTP requests, TCP communication and physical robot movement.

System integration required repeated testing and refinement to ensure that the robot responded appropriately to calculated commands.

## 9. Testing and Validation

Testing was performed throughout development to verify individual components and the complete navigation workflow.

The testing process included:

- Verifying camera acquisition and frame processing.
- Testing perspective transformation and calibration.
- Evaluating color-based marker detection.
- Checking position and orientation calculations.
- Testing destination selection.
- Validating heading calculations and navigation commands.
- Verifying HTTP requests and TCP communication.
- Testing movement commands on the physical HoverBot.
- Observing navigation behavior and refining system parameters.

The team used iterative debugging to address issues affecting both the robotic hardware and navigation software.

## 10. Project Results

The team successfully developed and demonstrated a computer-vision-based navigation system for the HoverBot.

The completed system was capable of:

- Tracking the robot through a live camera feed.
- Correcting camera perspective distortion.
- Detecting visual markers.
- Estimating the robot's position and orientation.
- Accepting a user-selected destination.
- Determining appropriate turning and forward movement commands.
- Sending movement instructions through the REST API and TCP server.
- Guiding the HoverBot toward the selected destination.

The project demonstrated the integration of external visual perception, navigation logic and an existing embedded robotic control system.

The completed implementation focused on one robot, while the architecture was designed to support future expansion to additional members of the swarm.

## 11. My Contributions

As an equal member of the four-person capstone team, I actively participated throughout the development, integration, testing and refinement of the HoverBot project.

My contributions included:

- **System Investigation:** Reviewing the inherited hardware, firmware and communication architecture to understand the platform and identify integration requirements.
- **Hardware Testing and Troubleshooting:** Working with the ESP32 control circuitry, electrical connections, grounding, power distribution and motor-control behavior to investigate hardware issues.
- **Computer Vision Development:** Contributing to the Python and OpenCV processing pipeline, including camera calibration, marker tracking and localization.
- **Navigation Development:** Participating in the integration and testing of position estimation, heading calculations and movement decision logic.
- **Communication Integration:** Working with the existing REST API and TCP server to verify command transmission and physical robot responses.
- **System Validation:** Conducting iterative tests, investigating unexpected behavior and refining the integrated navigation system.
- **Technical Documentation:** Contributing to progress reports, system documentation and the final presentation of project results.

A particularly valuable part of the experience was learning how to approach an unfamiliar robotic system, investigate problems across hardware and software boundaries and contribute to the development of a functioning integrated solution.

## 12. Project Documentation

The following reports contain additional information about the system architecture, technical implementation, development progress, testing and results.

- [Final Report](docs/Final%20Report.pdf) — Detailed technical report covering computer vision, localization, navigation, communication integration and project results.
- [Progress Report 5](docs/Progress%20Report%205.pdf) — Initial transition from R1D4 to the swarm robotics project and system evaluation.
- [Progress Report 6](docs/Progress%20Report%206.pdf) — Investigation of the existing robotic platforms, hardware issues and navigation development plans.
- [Progress Report 7](docs/Progress%20Report%207.pdf) — HoverBot communication setup, early computer vision tracking and navigation milestones.

These documents provide additional context about the engineering decisions, testing activities and challenges encountered throughout development.

## 13. Future Improvements

Potential improvements identified for future development include:

- Extending the navigation system to additional robotic platforms.
- Improving detection reliability under changing lighting conditions.
- Refining navigation accuracy and movement control.
- Incorporating additional sensor feedback.
- Improving route planning and obstacle handling.
- Further enhancing system modularity and scalability.

These are possible extensions rather than features claimed as part of the completed implementation.

## 14. Technical Skills Demonstrated

- Python programming
- OpenCV and image processing
- NumPy and vector geometry
- Camera calibration and perspective transformation
- Color-based object detection
- Robot localization and pose estimation
- Navigation and movement control
- REST API and TCP/IP communication
- ESP32 hardware integration
- Electrical troubleshooting and hardware testing
- System integration and validation
- Collaborative engineering problem-solving

## 15. Acknowledgments

This project was completed collaboratively by **Capstone Group DP 36** at McGill University under the guidance of Professor Joseph Vybihal.

The HoverBot platform and portions of its existing embedded firmware, TCP server and REST API were developed by previous engineering teams.

Our work built upon this foundation by investigating and improving the inherited system and developing its computer-vision-based localization and navigation functionality.

---

*This repository documents the engineering design, implementation and results of the HoverBot navigation project. Complete executable source code is not included.*

**[← Return to Main Capstone Repository](../README.md)**
