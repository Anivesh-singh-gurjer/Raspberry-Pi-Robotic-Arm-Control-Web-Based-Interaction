A robotics and web-integration project built around a Hiwonder ArmPi FPV AI Vision robotic arm running on Raspberry Pi. The project focuses on creating a practical interface for designing and executing robotic-arm motions, and integrating those actions with a web-based emotion-recognition interface.

Project Overview

The system combines three major components:

Robotic Arm Motion Control
PyQt-based Motion/Action Editor
Laravel Web Interface for Emotion-Driven Robot Interaction

The main goal was to move beyond manually controlling individual servo positions and provide a more structured way to create, organize, preview, and execute complete robot actions. The resulting actions can then be triggered through a web interface based on detected human emotions.

Key Features
🤖 Robotic Arm Motion & Action Editor

Developed a custom PyQt-based desktop application for controlling and creating robotic-arm movements on Raspberry Pi.

The interface provides functionality for:

Creating and editing individual robot poses.
Setting servo positions for different joints.
Previewing poses before execution.
Organizing multiple poses into complete actions.
Sequencing robot movements.
Adding delays between movements.
Reordering action steps.
Managing saved poses and action sequences.
Loading and saving robot action configurations.

This provides a more user-friendly alternative to manually controlling the arm through low-level servo commands or highly sensitive graphical controls.

🎬 Action-Based Robot Control

Instead of treating every servo movement as an independent command, the system uses action sequences composed of multiple poses.

For example:

Pose 1
   ↓
Delay
   ↓
Pose 2
   ↓
Delay
   ↓
Pose 3

These sequences can represent higher-level behaviors such as waving, greeting, or other predefined robot motions.

The project also integrates existing ArmPi action-group functionality, allowing saved actions such as:

wave.action
wave_1.action

to be executed by the robot.

🌐 Laravel Web Interface

A dedicated Robot Emotion Recognition webpage was developed as part of the existing Laravel-based emotion-recognition website.

The webpage provides:

Live camera access.
Microphone access.
6-second video recording.
Video upload from the user's computer.
Processing of recorded video clips.
Display of detected emotion.
Display of robot action.
Confidence information.
Robot execution status.
Robot emotion/action history.

The web interface communicates with the Raspberry Pi over the local network to trigger robot actions.

🎭 Emotion → Robot Action Mapping

The system connects emotion-recognition results to predefined robotic actions.

For example:

Detected Emotion
       ↓
   "Anger"
       ↓
 wave_1.action
       ↓
 Raspberry Pi
       ↓
 Robotic Arm

Current action mappings include:

Emotion	Robot Action
Happy	wave.action
Anger	wave_1.action

This mapping can be extended with additional emotions and robot behaviors.

🔄 Continuous / Forever Mode

The web interface is designed to support continuous emotion-driven interaction.

In continuous mode, the camera stream can be processed as a sequence of short clips:

Live Camera
     ↓
6-second Clip
     ↓
Emotion Recognition
     ↓
Robot Action
     ↓
6-second Clip
     ↓
Emotion Recognition
     ↓
Robot Action
     ↓
        ...

This allows the robotic arm to continuously respond to the emotions detected from the live video rather than requiring the user to manually start every individual recording.

📁 Video & Emotion History

Each processed robot-emotion clip can be stored in the database together with its associated information.

The stored record includes:

Video clip
Face prediction
Voice prediction
Text prediction
Whisper transcription
Final fused emotion
Confidence
Robot action
Robot execution status
Robot response
Timestamp

This creates a persistent history connecting:

Video → Emotion → Robot Action → Robot Execution

rather than treating each interaction as a temporary prediction.

System Architecture
                ┌─────────────────────┐
                │    Camera / Video   │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │   Laravel Web App   │
                │ Robot Emotion Page  │
                └──────────┬──────────┘
                           │
                     6-sec video
                           │
                           ▼
                ┌─────────────────────┐
                │ Emotion Recognition │
                │    Python API       │
                └──────────┬──────────┘
                           │
                    Final Emotion
                           │
                           ▼
                ┌─────────────────────┐
                │ Emotion → Action    │
                │      Mapping        │
                └──────────┬──────────┘
                           │
                    Robot Action
                           │
                           ▼
                ┌─────────────────────┐
                │    Raspberry Pi     │
                │     Robot API       │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │   Hiwonder ArmPi    │
                │   Robotic Arm       │
                └─────────────────────┘
Technology Stack
Robotics
Raspberry Pi
Hiwonder ArmPi FPV AI Vision Robotic Arm
Servo-based robotic arm control
Hiwonder SDK
Action-group based robot control
Desktop Application
Python
PyQt
JSON-based pose/action management
Web Application
Laravel
PHP
Blade
HTML
CSS
JavaScript
WebRTC/MediaRecorder APIs
Backend & Communication
REST APIs
HTTP communication
Raspberry Pi local-network communication
Python emotion-recognition API
Database
MySQL
Laravel Eloquent ORM
robot_emotion_records database table
Database Design

The robot interaction history is stored using the robot_emotion_records table.

Conceptually:

robot_emotion_records
│
├── video_path
├── face_prediction
├── voice_prediction
├── text_prediction
├── transcribed_text
├── final_fusion
├── confidence
├── robot_action
├── robot_status
├── robot_response
└── timestamps

This allows the system to maintain a complete record of each robot-emotion interaction.

Project Workflow

A typical interaction follows this process:

1. User opens Robot Emotion Recognition page
                ↓
2. Camera captures live video
                ↓
3. A 6-second clip is recorded
                ↓
4. Clip is uploaded to Laravel
                ↓
5. Laravel sends video to Python emotion API
                ↓
6. Facial, acoustic and textual predictions are generated
                ↓
7. Final emotion is determined
                ↓
8. Emotion is mapped to a robot action
                ↓
9. Laravel sends the emotion/action request to Raspberry Pi
                ↓
10. Raspberry Pi executes the corresponding action
                ↓
11. Emotion + robot action + execution status
    are stored in the database
Purpose

The project demonstrates the integration of robotics, desktop GUI development, web development, REST APIs, computer vision/emotion-recognition pipelines, and physical robot control into a single interactive system.

Rather than only predicting an emotion, the system connects the prediction to a physical response from a robotic arm, creating an end-to-end pipeline from human interaction to machine perception and robotic behavior.

Future Improvements

Potential extensions include:

Adding more emotion-to-action mappings.
Supporting more complex multi-step robot behaviors.
Improving continuous/Forever Mode scheduling.
Queueing multiple robot actions safely.
Adding real-time emotion visualization.
Providing detailed playback of historical interactions.
Adding robot-action customization directly from the web interface.
Supporting multiple robotic arms or Raspberry Pi devices.
Adding authentication and access control for remote robot operation.
