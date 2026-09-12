# Raspberry Pi Robotic Arm Emotion Interaction System

## Overview

This project extends a multimodal emotion recognition system into a physical human–robot interaction platform using a **Hiwonder ArmPi FPV robotic arm powered by Raspberry Pi**.

The system connects the emotion recognition pipeline with a web-based interface and the robotic arm, allowing the robot to recognize emotions from short video clips and automatically perform predefined physical actions corresponding to the detected emotion.

The project also includes a dedicated interface for designing, organizing, previewing, and executing robotic-arm motion sequences.

---

## Project Objectives

The main objectives of this project are:

- Integrate multimodal emotion recognition with a physical robotic system.
- Provide a web interface for interacting with the robotic arm.
- Capture and process short video clips for emotion recognition.
- Map recognized emotions to predefined robotic actions.
- Automatically send the appropriate action command to the Raspberry Pi.
- Record emotion recognition results together with the corresponding robot actions.
- Provide a persistent history of robot–emotion interactions.
- Develop an interface for creating and managing robotic-arm motion sequences.

---

## System Architecture

The overall system consists of three major components:

```text
                    ┌─────────────────────────┐
                    │       Web Interface     │
                    │                         │
                    │  Camera / Video Input   │
                    │  Robot Emotion Page     │
                    │  Motion Control UI      │
                    └────────────┬────────────┘
                                 │
                                 │ HTTP
                                 ▼
                    ┌─────────────────────────┐
                    │    Laravel Backend      │
                    │                         │
                    │ Emotion Controller       │
                    │ Robot Emotion Controller│
                    │ Database Management      │
                    └────────────┬────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
          ┌──────────────────┐      ┌──────────────────┐
          │ Emotion Analysis │      │ Raspberry Pi     │
          │ Python API       │      │ Robotic Arm      │
          │                  │      │                  │
          │ Face             │      │ Action Groups    │
          │ Voice            │      │ Motion Execution │
          │ Text             │      │                  │
          │ Multimodal Fusion│      │ ArmPi FPV        │
          └──────────────────┘      └──────────────────┘
```
Robotic Arm Motion Interface

A major component of this project is a dedicated interface for controlling and designing robotic-arm motions.

The interface provides a more structured alternative to manually controlling individual servo positions. Instead of relying only on direct joystick or slider-based control, users can create reusable motion sequences composed of predefined poses.

Motion Management

The motion interface supports functionality such as:

Loading predefined robotic-arm poses.
Previewing individual poses.
Creating sequences of multiple poses.
Organizing poses into action sequences.
Saving and loading action configurations.
Reordering motion steps.
Adding delays between motion steps.
Creating loops for repeated movements.
Executing predefined action groups.
Interacting with the Raspberry Pi robotic-arm controller.

This makes it possible to design higher-level behaviors rather than controlling each servo independently.

Robot Emotion Recognition

A dedicated Robot Emotion Recognition webpage was developed to connect the emotion recognition pipeline with the physical robotic arm.

The webpage provides a live camera interface where users can capture short video clips. Each clip is sent to the backend for multimodal emotion analysis.

The system then determines the recognized emotion and selects a corresponding robotic action.

For example:
```
Video Clip
    │
    ▼
Emotion Recognition
    │
    ▼
Final Emotion
    │
    ├── Happy ──────► wave.action
    │
    ├── Anger ──────► wave_1.action
    │
    └── Other ──────► No mapped action
```
The mapping between emotions and robotic actions can be extended to support additional emotions and action groups.

Video Processing

The robot emotion webpage supports video input through the browser camera.

The system can capture approximately 6-second video clips, which are then uploaded to the Laravel backend.

The workflow is:
```
Camera
  │
  ▼
6-second Video Clip
  │
  ▼
Laravel Backend
  │
  ▼
Python Emotion Recognition API
  │
  ▼
Multimodal Emotion Result
  │
  ▼
Robot Action Selection
  │
  ▼
Raspberry Pi
  │
  ▼
Robotic Arm Motion
```
The system can also be extended to continuously process consecutive clips, allowing the robot to operate in a continuous emotion-recognition mode.

Multimodal Emotion Analysis Integration

The robotic-arm system communicates with the existing multimodal emotion recognition pipeline through an HTTP API.

The backend sends the captured video to the Python inference service, which processes the available modalities.

The returned information can include:

Facial emotion prediction.
Voice/acoustic emotion prediction.
Text-based emotion prediction.
Whisper speech transcription.
Final multimodal fusion emotion.
Confidence score.

The Laravel backend receives these predictions and stores the results in the database.

Emotion-to-Action Mapping

The system uses an emotion-to-action mapping layer to determine which robotic motion should be executed.

The current implementation includes mappings such as:

Emotion	Robot Action
Happy	wave.action
Anger	wave_1.action

This architecture makes the system extensible.

Additional mappings can be added without redesigning the entire application:
```
Emotion
   │
   ▼
Action Mapping
   │
   ├── Happy    → wave.action
   ├── Anger    → wave_1.action
   ├── Sad      → ...
   ├── Fear     → ...
   ├── Surprise → ...
   └── Neutral  → ...
Raspberry Pi Communication
```
The Laravel backend communicates with the Raspberry Pi through an HTTP-based interface.

After emotion recognition, the backend sends information such as:

Detected emotion.
Confidence score.
Database record ID.

The Raspberry Pi receives the request and executes the corresponding robotic-arm action.

This separation allows the web application, machine-learning inference system, and robotic controller to operate as independent components.
```
Laravel
   │
   │ HTTP Request
   │
   ▼
Raspberry Pi
   │
   ▼
Robot Controller
   │
   ▼
Action Group
   │
   ▼
Servo Motors
Database and Interaction History
```
Each robot emotion interaction is stored in a dedicated database table:

robot_emotion_records

Each record can contain information including:

Video clip path.
Facial prediction.
Voice prediction.
Text prediction.
Whisper transcription.
Final fused emotion.
Confidence score.
Robot action.
Robot execution status.
Response returned by the Raspberry Pi.
Timestamp.

Example record structure:

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

This allows the system to maintain a history of interactions between the detected emotion and the physical action performed by the robot.

Robot Emotion History

The Robot Emotion Recognition webpage also provides an interaction history.

Previously processed clips can be displayed together with their associated:

Detected emotion.
Robot action.
Processing timestamp.
Execution status.

Individual records can also be opened to inspect the corresponding interaction in more detail.

This creates a traceable relationship between:

Video
   ↓
Emotion Recognition
   ↓
Robot Action
   ↓
Robot Execution
Continuous / Forever Mode

The system can also operate in a continuous mode.

Instead of manually starting every recording, the camera continuously captures consecutive video segments.

For example:

Camera Stream
     │
     ├── Clip 1 ──► Emotion ──► Robot Action
     │
     ├── Clip 2 ──► Emotion ──► Robot Action
     │
     ├── Clip 3 ──► Emotion ──► Robot Action
     │
     ├── Clip 4 ──► Emotion ──► Robot Action
     │
     └── ...        ...          ...

Each segment is independently processed and associated with its own database record.

This allows the robotic arm to react repeatedly to changing emotional expressions over time.

Web Application Features

The project integrates the robotic system into the existing Laravel web application.

Main Web Interface

The main interface provides:

Live camera access.
Video recording.
Video upload.
Dataset video processing.
Multimodal emotion analysis.
Individual modality predictions.
Final fusion result.
Speech transcription.
Video/database management.
Robot Emotion Interface

The dedicated robot page provides:

Live camera feed.
6-second clip recording.
Emotion analysis.
Current detected emotion.
Current robot action.
Robot execution status.
Robot emotion history.
Stored interaction records.
Integration with Raspberry Pi actions.
Continuous processing mode.
Technologies Used
Software
Laravel — Web application and backend API.
PHP — Backend logic.
Blade — Web interface templates.
JavaScript — Camera capture, recording, and asynchronous API communication.
Python — Emotion recognition inference API.
MySQL — Storage of emotion and robot interaction records.
HTTP/REST APIs — Communication between application components.
Machine Learning

The robotic system integrates with the project's existing multimodal emotion recognition pipeline, combining:

Facial emotion recognition.
Speech/acoustic emotion recognition.
Text-based emotion recognition.
Whisper-based speech transcription.
Multimodal fusion.
Hardware
Hiwonder ArmPi FPV
Raspberry Pi
Robotic-arm servo motors.
Camera module / USB camera.
Raspberry Pi-based robot controller.
Project Workflow

The complete workflow can be summarized as:

                    USER
                     │
                     ▼
              ┌──────────────┐
              │ Live Camera  │
              └──────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Video Segment │
             │   ~6 seconds  │
             └───────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Laravel       │
             │ Backend       │
             └───────┬───────┘
                     │
                     ▼
          ┌──────────────────────┐
          │ Multimodal Emotion   │
          │ Recognition API      │
          └──────────┬───────────┘
                     │
                     ▼
              ┌──────────────┐
              │ Final Emotion│
              └──────┬───────┘
                     │
                     ▼
              ┌──────────────┐
              │ Action Mapper│
              └──────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Raspberry Pi  │
             └───────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Robotic Arm   │
             │ Action Group  │
             └───────┬───────┘
                     │
                     ▼
             ┌───────────────┐
             │ Database      │
             │ Interaction   │
             │ History       │
             └───────────────┘
Key Contribution

The primary contribution of this project is the integration of multimodal emotion recognition with physical robotic-arm interaction.

Rather than limiting emotion recognition to a software prediction displayed on a screen, the system translates the recognized emotional state into a physical robotic response.

The project therefore provides an end-to-end pipeline connecting:

Video → Multimodal Emotion Recognition → Emotion-to-Action Mapping → Raspberry Pi → Robotic Arm → Recorded Interaction History

Additionally, the motion-design interface provides a practical method for creating and managing reusable robotic behaviors, making the system suitable for further development into more complex human–robot interaction applications.
