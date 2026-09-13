# Companion-Robot-for-the-Visually-Impaired

<img width="670" height="554" alt="image" src="https://github.com/user-attachments/assets/9a51bf7e-18f7-4947-9c75-d06ca13f5ee7" />

A mobile companion robot designed to help visually impaired users
navigate indoor environments independently. The robot scans a room,
builds a grid-based map, and guides the user through voice instructions.

Academic Project --- ECE-321
Egypt-Japan University of Science and Technology (E-JUST)
Supervised by Prof. Adel Bedair and Prof. Moataz Abdelwahab

📌 Project Overview

The project aims to guide a visually impaired person through a room.

The robot first scans the surrounding environment, identifies free
and occupied cells, creates a map of the room, and then uses the
generated map to support navigation through voice instructions.

The system integrates three main subsystems:

Obstacle Avoidance System

Mapping Software using the Wavefront Algorithm

Sound Guidance System

🎯 Objectives

The main objective is to provide a robotic companion capable of:

Detecting obstacles around the robot.

Scanning an indoor environment.

Building a grid-based representation of the room.

Estimating robot movement using rotary encoders.

Improving left/right motor synchronization using PID control.

Guiding the user through voice instructions.

The overall concept is to improve independence, safety, and confidence
for visually impaired users.

🧠 System Concept

The robot combines sensing, motion control, mapping, and audio guidance.
<img width="3270" height="2874" alt="PBl project schematic_bb" src="https://github.com/user-attachments/assets/efc47694-75b7-4d12-9416-c6872c01f3d3" />


                 ┌─────────────────────┐
                 │   Ultrasonic        │
                 │     Sensors         │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Obstacle Detection  │
                 └──────────┬──────────┘
                            │
                            ▼
┌───────────────┐   ┌─────────────────────┐
│ Rotary        │──►│  Mapping /          │
│ Encoders      │   │  Wavefront Algorithm│
└───────────────┘   └──────────┬──────────┘
                               │
                               ▼
                     ┌──────────────────┐
                     │ Navigation /     │
                     │ Movement Control │
                     └────────┬─────────┘
                              │
             ┌────────────────┴────────────────┐
             ▼                                 ▼
      ┌──────────────┐                  ┌──────────────┐
      │ DC Motors    │                  │ MP3 Player + │
      │ + PID        │                  │ Speaker      │
      └──────────────┘                  └──────────────┘

🔩 Hardware Components

The prototype presented in the project consists of:

Component                      Quantity

Arduino Uno                           1
HC-SR04 Ultrasonic Sensors            4
L298N Motor Driver                    1
Rotary Angle Encoders                 2
DC Motors                             2
MP3 Player                            1
Speaker                               1
Battery                               1

Main Controller

The Arduino Uno acts as the main controller, coordinating sensor
readings, motor control, mapping, and the overall robot behavior.

Ultrasonic Sensors

Four HC-SR04 ultrasonic sensors are used to scan the environment and
detect obstacles.

The robot uses the sensors to determine the state of surrounding cells,
including the left, right, and front areas during scanning.

Rotary Encoders
<img width="718" height="404" alt="image" src="https://github.com/user-attachments/assets/3f27cbd6-3d9c-4154-8c5f-1d57890cfc53" />

Two rotary encoders are attached to the wheels.

Rotary encoders generate electrical pulses as the shaft rotates. These
pulses are interpreted by the controller to determine the direction and
amount of wheel rotation.

In the project implementation, the encoder readings are used to estimate
the robot's movement through the grid. The presentation describes one
full cell as corresponding to approximately 10 wheel rotations.

Motor Driver and Motors

An L298N motor driver controls the two DC motors.

The motors provide the robot's differential-drive movement, while the
rotary encoders provide feedback for synchronization.

Audio System

An MP3 player and speaker provide the audio/voice guidance component
of the system.

🗺️ Room Mapping
<img width="806" height="675" alt="image" src="https://github.com/user-attachments/assets/a11645e9-5be8-4fc7-bcf5-682d9a933ad9" />


The robot scans the room and represents it as a grid.

During the scanning process, the robot moves through odd-numbered
columns while detecting:

The current cell

The left cell

The right cell

The front cell

The robot iterates over most cells twice to reduce the mapping
error.

Cell Representation

Symbol   Meaning

``       Not yet detected
O      Free
X      Obstacle

A simplified representation is:

0  1  2  3  4  5  6  7  8  9
--------------------------------
O  O  X  O  O  O  X  O  O  O
O  X  X  O  O  O  X  O  O  O
O  O  O  O  X  O  O  O  X  O
...

The resulting grid provides the basis for navigation and path planning.

⚙️ Wavefront Algorithm
<img width="1593" height="708" alt="image" src="https://github.com/user-attachments/assets/f7e7c00b-ea5f-4b09-b731-c8339903baf4" />

The mapping/navigation software is based on the Wavefront Algorithm.

The algorithm operates on the grid representation of the environment and
can be used to determine a route through free cells while avoiding
detected obstacles.

The project architecture therefore combines:

Environment
    ↓
Ultrasonic Scanning
    ↓
Grid Map
    ↓
Wavefront Algorithm
    ↓
Navigation Decisions
    ↓
Motor Control

🎛️ PID Motor Synchronization

A PID controller is used to compensate for differences between the two
motors.

Because two DC motors may not rotate at exactly the same speed under
identical commands, their encoder readings are compared and the motor
control is adjusted according to the synchronization error.

The controller consists of three terms:

Proportional (P)

Adjusts the output according to the current error.

Integral (I)

Accumulates previous errors to reduce steady-state error.

Derivative (D)

Responds to the rate of change of the error, helping control
overshoot and improve system response.

Conceptually:

Encoder Left  ───────┐
                     ├──► Error ───► PID Controller ───► Motor Control
Encoder Right ───────┘

The PID controller is particularly important for keeping the two motors
synchronized while the robot moves through the mapping grid.

🔄 Overall Operation
<img width="803" height="510" alt="image" src="https://github.com/user-attachments/assets/9725904c-2f41-40e4-84c0-9970abea871f" />

The expected operating sequence is:

Start the robot.

Scan the surrounding environment using the four ultrasonic
sensors.

Read wheel movement using the two rotary encoders.

Update the grid map with detected free and occupied cells.

Repeat the scan to reduce mapping errors.

Apply the Wavefront Algorithm to the generated map.

Control the DC motors to navigate through the environment.

Use PID feedback to compensate for motor synchronization errors.

Provide voice guidance through the MP3 player and speaker.

🧩 System Architecture

                    ┌───────────────────┐
                    │    Arduino Uno    │
                    │  Main Controller  │
                    └───────┬───────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌──────────────┐     ┌──────────────┐    ┌──────────────┐
│ 4 × HC-SR04  │     │ 2 × Rotary   │    │ MP3 Player + │
│   Sensors    │     │   Encoders   │    │   Speaker    │
└──────────────┘     └──────┬───────┘    └──────────────┘
                             │
                             ▼
                       ┌───────────┐
                       │ PID Motor │
                       │ Control   │
                       └─────┬─────┘
                             │
                             ▼
                       ┌───────────┐
                       │  L298N    │
                       │  Driver   │
                       └─────┬─────┘
                             │
                       ┌─────┴─────┐
                       ▼           ▼
                    DC Motor    DC Motor


🚀 Future Improvements

The original project identified several possible improvements:

Fine-tune the PID controller parameters.

Use the Robot Operating System (ROS) for improved control over
multiple units.

Implement Dijkstra's Algorithm to find the shortest route to a
specified destination.

Apply a Kalman filter to rotary encoder readings.

These improvements could make the robot's control, localization, and
path planning more robust.
