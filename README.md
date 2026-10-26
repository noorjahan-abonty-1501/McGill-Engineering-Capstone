# McGill University | Electrical Engineering Capstone Projects

**ECSE 458D1/D2: Capstone Design Project | 2024–2025**

**Robotics | Embedded Systems | Computer Vision | Software Development | Hardware-Software Integration**

## Overview

This repository documents two robotics projects completed as part of my Electrical Engineering Capstone Design Project at McGill University.

Working collaboratively in a four-member engineering team, I contributed across the development of both projects, including system design, software development, hardware-software integration, technical troubleshooting, testing and documentation.

The capstone consisted of two major projects:

1. **R1D4 Hospital Delivery Robot:** Development of an Android tablet application for delivery scheduling, task management and robot status monitoring, integrated with an ESP32-based communication system.
2. **HoverBot Navigation System:** Development of a computer-vision-based localization and navigation system using Python, OpenCV and the existing robotic control infrastructure.

Both projects involved understanding and extending existing engineering systems, integrating software and hardware components and solving technical challenges through iterative testing and debugging.

---

## Project 1: R1D4 Hospital Delivery Robot

**Technologies:** React Native, Expo, JavaScript, ESP32, C++, Android, USB Serial Communication, Telnet, TCP/IP, AsyncStorage

### Project Overview

The R1D4 project focused on developing an interactive Android tablet application for a robotic platform designed to deliver medical supplies in hospital environments.

The existing R1D4 robot required a user-friendly interface that would allow hospital staff to schedule deliveries, manage tasks, confirm pickups and monitor the robot's operational status.

Our team developed a tablet-based application using React Native and Expo, while working to establish bidirectional communication between the Android tablet, an ESP32 microcontroller and the existing Telnet-based robot control system.

### Key Technical Work

- Developed and integrated an Android tablet application using React Native, Expo and JavaScript.
- Implemented delivery scheduling, task management, editing, deletion and pickup confirmation functionality.
- Used React Native AsyncStorage to maintain delivery records and track their status across application sessions.
- Implemented delivery scheduling logic that checks upcoming tasks and transmits delivery information approximately five minutes before the scheduled time.
- Established USB serial communication between the Android tablet and ESP32 microcontroller.
- Integrated communication between the ESP32 and the existing Telnet server over TCP/IP.
- Developed animated interface states to display robot status, including Welcome, Processing, Traveling, Ready and Error.
- Tested and debugged communication, message handling and hardware-software integration.

### Engineering Challenges

The project required integrating a new application with an existing robotic communication architecture.

A key challenge involved establishing reliable data exchange between the tablet, ESP32 and Telnet server while maintaining compatibility with the robot's existing control system.

The team also evaluated different application architectures, ultimately selecting a tablet-based Android application instead of a web interface to better support local functionality and hardware integration.

### Project Outcome

The team completed and installed the Android application on the designated tablet.

The final application supported delivery scheduling, task management, pickup confirmation, persistent local storage and robot status visualization.

Bidirectional communication between the tablet, ESP32 and Telnet-based infrastructure was demonstrated, providing a foundation for further integration with the complete robotic platform.

**[Explore the R1D4 Project](R1D4-Hospital-Delivery-Robot/)**

---

## Project 2: HoverBot Computer Vision Navigation System

**Technologies:** Python, OpenCV, NumPy, ESP32, Computer Vision, REST API, HTTP, TCP/IP, Java-Based Server, Robotics

### Project Overview

The HoverBot project focused on developing a computer-vision-based localization and navigation system for an existing robot within a larger swarm robotics research project.

Initially, robot movement depended on individual commands transmitted through an existing communication system. The platform lacked a centralized navigation system capable of tracking the robot's position and automatically generating movement commands toward a selected destination.

Our team developed a Python-based navigation system that uses an external camera to track the HoverBot, estimate its position and orientation and communicate movement instructions through the existing REST API and TCP server.

The system was designed with modularity in mind to support potential expansion to additional robots.

### Key Technical Work

- Investigated and troubleshot inherited robotic hardware, embedded software and network infrastructure.
- Examined ESP32 connections, motor-control interfaces, grounding, power distribution and communication functionality.
- Developed a Python and OpenCV computer vision pipeline for real-time robot tracking.
- Implemented camera perspective correction to generate a more consistent top-down view of the operating environment.
- Used HSV color thresholding and contour detection to identify visual markers on the robot.
- Calculated the robot's estimated position and orientation using marker coordinates and vector geometry.
- Developed navigation logic to determine movement direction from the robot's current position to a user-selected destination.
- Integrated HTTP-based movement commands with the existing REST API and Java TCP server.
- Performed iterative testing, debugging and refinement to improve localization and navigation performance.

### Engineering Challenges

The HoverBot was inherited from previous development teams, and the existing system required extensive investigation before new functionality could be added.

Challenges included incomplete documentation, hardware-related faults, communication setup issues, camera perspective distortion and unreliable marker detection under varying lighting conditions.

The team worked through these problems by studying the existing architecture, troubleshooting hardware and communication components, refining computer vision algorithms and repeatedly testing the integrated system.

### Project Outcome

The completed system demonstrated real-time robot localization and computer-vision-guided navigation toward user-selected destinations.

The application could identify the HoverBot in a camera feed, estimate its position and orientation, calculate movement decisions and send commands through the existing network infrastructure.

The implementation focused on the HoverBot while establishing a modular foundation for future multi-robot navigation development.

**[Explore the HoverBot Project](HoverBot-Swarm-Navigation/)**

---

## My Contributions

As an equal member of the four-person engineering team, I participated throughout the development, integration and testing of both capstone projects.

My contributions included:

- **System Design and Architecture:** Collaborated on understanding existing robotic platforms, evaluating technical approaches and developing system integration strategies.
- **Software Development:** Contributed to Android application functionality, communication interfaces, computer vision processing and navigation system development.
- **Hardware-Software Integration:** Worked with ESP32-based robotic systems, USB serial communication, TCP/IP networking and the integration of software with physical hardware.
- **Testing and Troubleshooting:** Investigated communication problems, checked hardware connections, validated system functionality and participated in iterative debugging and performance refinement.
- **Project Development and Collaboration:** Worked with teammates to coordinate implementation, resolve technical challenges, integrate system components and prepare engineering documentation.

Throughout both projects, I gained experience working with unfamiliar technical systems, identifying and resolving integration problems and translating engineering concepts into functioning prototypes.

## Technical Skills Demonstrated

| Area | Technologies and Concepts |
|---|---|
| Programming | Python, JavaScript, C++ |
| Embedded Systems | ESP32, microcontroller interfaces, hardware integration |
| Computer Vision | OpenCV, HSV color detection, contour analysis, object tracking |
| Numerical Processing | NumPy, vector geometry, coordinate transformations |
| Communication | USB Serial, UART, Telnet, TCP/IP, HTTP, REST API |
| Application Development | React Native, Expo, Android, AsyncStorage |
| Robotics | Localization, pose estimation, navigation and motor-control integration |
| Engineering | System design, integration, debugging, testing and validation |
| Collaboration | Technical documentation, teamwork and interdisciplinary problem-solving |

## Project Documentation

Each project folder contains detailed technical documentation, including progress reports, design explanations, implementation discussions and testing results.

### R1D4 Documentation

- [R1D4 Project Overview](R1D4-Hospital-Delivery-Robot/)
- [Mid-Project Report](R1D4-Hospital-Delivery-Robot/docs/Mid-Project%20Report.pdf)
- [Mid-Project Presentation](R1D4-Hospital-Delivery-Robot/docs/Mid-Project%20Presentation.pdf)

Additional progress reports are available in the project's `docs` folder.

### HoverBot Documentation

- [HoverBot Project Overview](HoverBot-Swarm-Navigation/)
- [Final Project Report](HoverBot-Swarm-Navigation/docs/Final%20Report.pdf)

Additional progress reports are available in the project's `docs` folder.

## Team and Acknowledgments

These projects were completed collaboratively by **Capstone Group DP 36** at McGill University.

**Team Members:**

- Noorjahan Abonty
- Paolo Bizzarri
- Joseph Hershkop
- Anjali Singhal

**Faculty Advisor:** Joseph Vybihal

Both projects built upon existing robotic platforms and previous development work. We acknowledge the contributions of earlier McGill engineering teams whose hardware, software and technical documentation supported our development.

---

*This repository is intended as a technical portfolio documenting the design, implementation, integration and results of the two capstone projects. It contains project documentation rather than complete standalone source-code repositories.*
