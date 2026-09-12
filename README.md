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
# Robotic Arm Motion & Emotion Interface

A comprehensive web-based and hardware integration project connecting **multimodal emotion recognition** with **physical robotic-arm actuation**, built with Laravel, Python, and Raspberry Pi.

---

## 🚀 Overview

This project bridges software-based affective computing and physical robotics. Rather than limiting emotion recognition to a screen display, the system translates recognized emotional states into physical robotic responses in real time. It features a dedicated motion-design interface for creating reusable robotic behaviors and an end-to-end pipeline connecting browser video capture, multimodal inference, emotion-to-action mapping, and hardware execution.

---

## 🛠️ System Architecture & Workflow

```text
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
```

---

## 🦾 Robotic Arm Motion Interface

The project includes a dedicated interface for controlling and designing robotic-arm motions, providing a structured alternative to manually controlling individual servo positions or relying solely on direct joystick/slider controls.

### Motion Management Features
- Loading predefined robotic-arm poses.
- Previewing individual poses.
- Creating sequences of multiple poses and organizing them into action sequences.
- Saving and loading action configurations.
- Reordering motion steps, adding delays, and creating loops for repeated movements.
- Executing predefined action groups and interacting directly with the Raspberry Pi controller.

---

## 🎭 Robot Emotion Recognition

The dedicated Robot Emotion Recognition webpage connects the emotion recognition pipeline with the physical robotic arm via a live camera interface.

### Video Processing Workflow
1. **Camera Capture:** Captures approximately 6-second video clips via the browser.
2. **Backend Upload:** Clips are uploaded to the Laravel backend.
3. **Inference:** Sent to the Python Multimodal Emotion Recognition API.
4. **Action Selection:** The resulting emotion maps to a physical robotic action.
5. **Execution:** Instructions are sent to the Raspberry Pi to control the robotic arm.

```text
Camera ──► 6-second Video Clip ──► Laravel Backend ──► Python Emotion API ──► Multimodal Result ──► Action Selection ──► Raspberry Pi ──► Robotic Arm Motion
```

### Continuous / Forever Mode
The system can operate in a continuous mode where consecutive video segments are captured, processed, and executed automatically, allowing the robot to react dynamically to changing emotional expressions over time.

---

## 🧠 Multimodal Emotion Analysis Integration

The backend communicates with the Python inference service via an HTTP API to evaluate multiple modalities:
- **Facial emotion prediction**
- **Voice/acoustic emotion prediction**
- **Text-based emotion prediction**
- **Whisper speech transcription**
- **Final multimodal fusion emotion & confidence score**

### Emotion-to-Action Mapping
The system uses an extensible mapping layer to associate emotions with specific robotic behaviors:

| Emotion | Robotic Action |
| :--- | :--- |
| **Happy** | `wave.action` |
| **Anger** | `wave_1.action` |
| **Sad** | *Extensible* |
| **Fear** | *Extensible* |
| **Surprise** | *Extensible* |
| **Neutral** | *Extensible* |

---

## 🥧 Raspberry Pi Communication

The Laravel backend communicates with the Raspberry Pi through an HTTP-based interface, transmitting:
- Detected emotion
- Confidence score
- Database record ID

This clean separation allows the web application, machine learning inference system, and robotic controller to operate as independent, modular components.

---

## 🗄️ Database & Interaction History

Each interaction is logged in the `robot_emotion_records` table to maintain a fully traceable relationship between video input, emotion recognition, robot action, and physical execution status.

### Record Schema
- `video_path`
- `face_prediction`
- `voice_prediction`
- `text_prediction`
- `transcribed_text`
- `final_fusion`
- `confidence`
- `robot_action`
- `robot_status`
- `robot_response`
- `timestamps`

---

## 🖥️ Web Application Features

### Main Web Interface
- Live camera access, video recording, and upload
- Dataset video processing
- Multimodal emotion analysis & individual modality predictions
- Speech transcription and video/database management

### Robot Emotion Interface
- Live camera feed & 6-second clip recording
- Real-time emotion analysis, detected emotion, and action status
- Robot emotion history and stored interaction records
- Continuous processing mode toggle

---

## 🧰 Technologies Used

### Software & Backend
- **Laravel / PHP:** Web application framework and backend logic
- **Blade:** Web interface templating
- **JavaScript:** Camera capture, recording, and asynchronous API communication
- **Python:** Emotion recognition inference API
- **MySQL:** Storage of emotion and robot interaction records
- **HTTP/REST APIs:** Inter-component communication

### Machine Learning
- **Facial Emotion Recognition**
- **Speech / Acoustic Emotion Recognition**
- **Text-based Emotion Recognition**
- **OpenAI Whisper:** Speech transcription
- **Multimodal Fusion Engine**

### Hardware
- **Hiwonder ArmPi FPV**
- **Raspberry Pi** (Robot controller)
- Robotic-arm servo motors
- Camera module / USB camera

---

## 🏆 Key Contribution

The primary contribution of this project is the successful end-to-end integration of multimodal emotion recognition with physical robotic-arm interaction. By translating software-predicted emotional states into physical robotic responses and providing a robust motion-design framework, this project establishes a scalable foundation for advanced human-robot interaction (HRI) applications.
