# R1D4 Hospital Delivery Robot | Android User Interface and Communication System

**McGill University | Electrical Engineering Capstone Design | 2024–2025**

**React Native | Expo | JavaScript | ESP32 | Android | USB Serial | Telnet | TCP/IP**

[← Back to Main Capstone Repository](../README.md)

---

## 1. Project Overview

The R1D4 project focused on developing an interactive Android tablet application for an existing robotic platform designed to transport medical supplies between hospital rooms.

The original R1D4 platform lacked a dedicated user-facing interface through which hospital staff could easily schedule deliveries, manage tasks and monitor the robot's operating status.

Our four-member capstone team developed an Android application using React Native and Expo, providing an interface for scheduling deliveries, managing delivery information, confirming pickups and displaying robot status through animated facial expressions.

A major technical component involved establishing bidirectional communication between the Android tablet, an ESP32 microcontroller and the existing Telnet-based control infrastructure.

The project combined application development, embedded communication, system integration, user-interface design and technical troubleshooting.

## 2. Project Objectives

The main objectives were to:

1. Develop an intuitive touchscreen interface for the R1D4 hospital delivery robot.
2. Enable users to create, edit, delete and monitor scheduled deliveries.
3. Provide pickup confirmation functionality.
4. Display the robot's operating status through animated facial expressions.
5. Establish USB serial communication between the Android tablet and ESP32.
6. Integrate the application with the existing Telnet-based communication system.
7. Implement persistent storage for delivery information.
8. Test and validate application functionality and communication between components.

The completed work focused on developing and validating the tablet interface and its communication infrastructure, rather than rebuilding the original robotic platform.

## 3. Technologies and Tools

| Category | Technologies |
|---|---|
| Mobile Application | React Native, Expo, JavaScript |
| Embedded Hardware | ESP32 microcontroller |
| Embedded Programming | C++ |
| Android Development | Android Studio, Android USB Host functionality |
| Communication | USB Serial, UART, Telnet, TCP/IP |
| Local Data Storage | React Native AsyncStorage |
| Data Management | JSON, UUID |
| User Interface | Figma, Lottie animations |
| Development and Testing | Arduino IDE, Android emulator, physical Android tablet |

## 4. System Architecture

The R1D4 interface was developed around three interconnected components:

### Android Tablet Application

The Android application serves as the primary interface for delivery scheduling, task management and robot status monitoring.

It manages locally stored delivery information and exchanges relevant data with the ESP32.

### ESP32 Microcontroller

The ESP32 serves as the communication bridge between the tablet and the existing robot-control infrastructure.

It communicates with the Android application through USB serial and supports network communication through a Telnet server.

### Telnet Communication and Robot Control

The ESP32 hosts the Telnet server, allowing a remote laptop to connect over Wi-Fi and exchange commands and status information.

The ESP32 also interfaces with the R1D4 robot's control signals through its GPIO connections.

### Communication Architecture

```text
             R1D4 Robot
                  |
                  |
           GPIO Interfaces
                  |
                  v
          ESP32 Controller
             /       \
            /         \
      USB Serial    Wi-Fi / TCP
          |              |
          v              v
    Android Tablet   Telnet Client
   (React Native)       (Laptop)
```

Communication between the tablet and ESP32 is bidirectional.

The application can transmit delivery information, while the ESP32 can relay robot status messages back to the tablet.

The system was designed to work alongside the robot's existing embedded architecture rather than replace its original control system.

## 5. Android Application Development

The application was designed to provide a simple and intuitive interface for users interacting with the delivery robot.

The final application contains four main pages.

### 5.1 Home Page

The Home page serves as the application's main interface.

It displays an animated robot face and provides navigation to the delivery-management features.

The robot's expression changes based on its reported operating state.

### 5.2 Send Delivery

The Send Delivery page allows users to schedule delivery tasks.

Users can enter information such as the delivery location, scheduled time and other relevant delivery details.

The information is stored locally until it is ready to be transmitted for execution.

### 5.3 Manage Deliveries

The Manage Deliveries page displays delivery records and allows users to review their status.

Pending deliveries can be edited or deleted before entering the dispatch window.

This functionality was an important part of ensuring that users could manage scheduled tasks without unnecessarily transmitting updates to the robot.

### 5.4 Confirm Pickup

The Confirm Pickup page allows recipients to confirm collection of a delivered item.

Once confirmed, the corresponding delivery record is updated and moved to the completed category.

### 5.5 Animated Robot Status

The interface includes five animated states:

- Welcome
- Processing
- Traveling
- Ready
- Error

These states provide visual feedback about the robot's current activity.

Lottie animations were incorporated into the React Native interface to display the different expressions.

## 6. Delivery Scheduling and Status Management

The application uses a structured delivery-management workflow.

### Delivery States

| Status | Description |
|---|---|
| Pending | A scheduled delivery that has not yet entered the dispatch window |
| Sent | A delivery that has entered the dispatch window and has been transmitted for execution |
| Confirmed | A delivery whose pickup has been confirmed by the recipient |

### Scheduling Logic

An important design improvement involved changing when delivery information was transmitted.

Initially, the system was designed to send delivery data as soon as a task was scheduled.

However, this complicated the process of editing or deleting deliveries because the robot would need to maintain and update records for tasks that might occur much later.

The team revised the approach to keep pending deliveries in the tablet application until closer to their scheduled time.

The final logic:

1. Stores scheduled delivery information locally.
2. Checks pending deliveries approximately every minute.
3. Identifies deliveries scheduled within the next five minutes.
4. Sends the required delivery information through the communication interface.
5. Restricts editing and deletion once the delivery enters the dispatch window.

This design reduces unnecessary data transmission and limits the information that the robot must store.

## 7. Persistent Local Data Storage

The application uses React Native AsyncStorage to maintain delivery information across sessions.

Each delivery record is assigned a unique identifier using UUID functionality.

The storage implementation supports:

- Saving new delivery records.
- Retrieving scheduled deliveries.
- Editing eligible pending deliveries.
- Deleting eligible pending deliveries.
- Tracking delivery status.
- Preserving information after the application is closed or restarted.

The use of local storage avoids requiring an external database for the application's delivery-management functionality.

It also allows the tablet interface to maintain scheduled delivery information independently of a continuous network connection.

## 8. USB Serial Communication

One of the major technical challenges was establishing communication between the Android application and the ESP32 microcontroller.

The team selected a physical USB serial connection between the tablet and ESP32.

This approach was chosen partly because wireless connectivity can be unreliable in some environments, including areas where a mobile robot may temporarily lose network access.

### 8.1 Communication Setup

The Android application establishes communication through a USB serial interface.

The process includes:

1. Detecting connected USB devices.
2. Requesting permission to access the ESP32 connection.
3. Opening the serial port.
4. Configuring the communication parameters.
5. Sending delivery-related data.
6. Receiving messages from the ESP32.
7. Processing incoming information and updating the application.

The final React Native application uses the `react-native-usb-serialport-for-android` package.

### 8.2 Message Encoding and Decoding

Outgoing delivery information is formatted for transmission over the serial interface.

The application converts outgoing messages into the representation expected by the USB communication package.

Incoming messages are decoded and interpreted according to predefined command values.

These values are associated with robot operating states, allowing the application to update its global state and display the appropriate animation.

### 8.3 Communication Validation

During development, a separate Android test application was used to verify USB serial communication.

This allowed the team to test message transfer independently of the complete React Native interface.

An early issue occurred when the receiving application captured only part of an incoming serial message.

The team investigated buffering and message parsing, implementing handling for newline-terminated messages to reconstruct complete data.

This testing helped establish the communication workflow needed for the final application.

## 9. ESP32 and Telnet Integration

The ESP32 provides the connection between the Android application and the robot's existing control infrastructure.

The existing embedded codebase required investigation and debugging before the communication system could be integrated successfully.

### Telnet Communication

The ESP32 hosts a Telnet server that can be accessed by a remote laptop over a compatible Wi-Fi network.

This allows remote commands and status information to be exchanged with the embedded system.

### Bidirectional Data Flow

The communication architecture supports two directions of data transfer:

**Tablet to ESP32**

The Android application sends delivery information over USB serial.

The ESP32 receives and processes the transmitted data.

**ESP32 to Tablet**

The ESP32 transmits status information to the application.

The application interprets incoming messages and updates the user interface.

During development, the team also demonstrated communication between the ESP32 and a Telnet client running on a laptop.

This provided a way to validate communication between the application and the existing network-based control system.

## 10. Engineering Challenges and Solutions

### Challenge 1: Selecting the Application Architecture

The team initially evaluated developing a web interface instead of a dedicated Android application.

A web interface offered flexibility but also introduced concerns related to connectivity and integration with the existing robotic system.

After evaluating the alternatives and discussing the requirements with the project advisor, the team selected a tablet-based Android application.

React Native and Expo were chosen to support application development and hardware integration.

### Challenge 2: Working with Existing ESP32 Code

The ESP32 firmware originated from previous development work.

Before adding new communication functionality, the team needed to understand the existing codebase and address issues affecting its operation.

This involved configuring the development environment, testing serial output and verifying that movement-related commands could be exchanged through the system.

### Challenge 3: Establishing Reliable Communication

The application required reliable data exchange between an Android tablet and an embedded microcontroller.

The team investigated USB Host functionality, device permissions, serial-port configuration and message handling.

A dedicated test application was used to isolate communication issues before integrating the complete user interface.

### Challenge 4: Managing Serial Messages

Early testing identified problems with incomplete message reception.

The team investigated the handling of serial input, message buffering and newline-based message termination.

This improved the reliability of communication between the tablet and ESP32.

### Challenge 5: Efficient Delivery Management

Sending delivery information immediately after scheduling created unnecessary complexity when tasks were modified.

The system was redesigned to retain pending deliveries locally and transmit only the information required shortly before execution.

This simplified editing and deletion while reducing the amount of information transmitted to the robot.

## 11. Testing and Validation

Testing was performed throughout development to validate individual components and the integrated system.

### Application Testing

- Evaluated application screens using Android development tools.
- Tested delivery creation and scheduling.
- Verified editing and deletion of pending deliveries.
- Checked pickup confirmation and delivery-status transitions.
- Tested locally stored delivery records.
- Verified animated interface states.

### Communication Testing

- Tested serial communication with the ESP32.
- Verified USB connection and device permissions.
- Checked outgoing and incoming message handling.
- Tested Telnet communication with a remote laptop.
- Verified data exchange between the tablet, ESP32 and Telnet infrastructure.

### System Integration

The team tested the application on the designated Android tablet and investigated issues that occurred when integrating the software and communication components.

These tests supported the final deployment of the application onto the tablet.

## 12. Project Results

The team successfully completed the Android tablet application and integrated it with the communication infrastructure developed for the R1D4 platform.

The completed application supported:

- Delivery scheduling and management.
- Editing and deletion of eligible pending tasks.
- Pickup confirmation.
- Persistent local storage.
- Animated robot status displays.
- USB serial communication with the ESP32.
- Communication with the existing Telnet-based infrastructure.

The final application was built and installed on the designated Android tablet.

The project demonstrated a functioning interface and communication prototype intended for integration with the wider R1D4 robotic system.

Full hospital deployment and complete integration with all R1D4 robot hardware remained outside the demonstrated scope of the capstone work.

## 13. My Contributions

As an equal member of the four-person capstone team, I contributed throughout the design, development, integration and testing of the R1D4 interface project.

My involvement included:

- **System Design:** Participating in technical discussions, evaluating a website versus an Android application and helping define the interface and communication requirements.
- **Application Development:** Contributing to the React Native and Expo application, including delivery-management functionality and interface improvements.
- **Task Management:** Working on the Manage Deliveries and Confirm Pickup pages, including the organization of delivery information and edit, delete and confirmation functionality.
- **Embedded Communication:** Participating in ESP32 integration, USB serial communication testing and understanding data exchange with the Telnet-based infrastructure.
- **Testing and Debugging:** Helping investigate communication and integration problems, validating application behavior and testing the system across hardware and software components.
- **System Integration:** Collaborating on the integration of the tablet interface with the ESP32 and existing R1D4 communication architecture.
- **Technical Documentation:** Contributing to progress reports, design discussions, implementation documentation and project presentations.

This project strengthened my skills in Android application development, embedded communication, hardware-software integration, troubleshooting and collaborative engineering development.

## 14. Project Documentation

The following documents provide additional information about the system architecture, implementation, testing and development progress.

- [Mid-Project Report](docs/Mid-Project%20Report.pdf) — Detailed technical report covering the initial architecture, communication development, design decisions and testing.
- [Mid-Project Presentation](docs/Mid-Project%20Presentation.pdf) — Presentation covering the R1D4 interface, communication architecture and project development.
- [Progress Report 1](docs/Progress%20Report%201.pdf) — Initial project objectives and planning.
- [Progress Report 2](docs/Progress%20Report%202.pdf) — Interface design, initial architecture decisions and development planning.
- [Progress Report 3](docs/Progress%20Report%203.pdf) — Early Android application development and ESP32 communication testing.
- [Progress Report 4](docs/Progress%20Report%204.pdf) — Application progress, Telnet integration and remaining communication challenges.

The final capstone report also contains additional information about the completed R1D4 interface, including persistent storage, delivery scheduling logic and final application results.

## 15. Technical Skills Demonstrated

- React Native and Expo application development
- JavaScript programming
- ESP32 microcontroller integration
- C++ and embedded firmware interaction
- Android USB Host functionality
- USB serial and UART communication
- Telnet and TCP/IP networking
- Persistent local data storage
- User-interface development
- Delivery scheduling and state management
- Hardware-software integration
- Technical testing and debugging
- Collaborative engineering problem-solving

## 16. Acknowledgments

This project was completed collaboratively by **Capstone Group DP 36** at McGill University under the guidance of Professor Joseph Vybihal.

The R1D4 robotic platform and portions of the existing hardware and communication infrastructure were developed by earlier McGill engineering teams.

Our capstone work focused on developing the tablet-based user interface and integrating it with the existing embedded communication infrastructure.

---

*This repository documents the engineering design, implementation and results of the R1D4 interface project. Complete standalone application and firmware source code is not included.*

**[← Return to Main Capstone Repository](../README.md)**
