# 🤖 ArmPi Robotic Arm Motion Control & Emotion-Responsive Interface

An integrated robotic-arm control and web-based emotion interaction system built around the **Hiwonder ArmPi FPV AI Vision Robotic Arm** and a **Laravel-based web application**.

The project focuses on creating, managing, and executing robotic-arm motions through a dedicated desktop interface, while also connecting the robot with a web-based emotion recognition interface that allows detected emotions to trigger corresponding robot actions.

---

## 📌 Project Overview

This project extends the capabilities of the ArmPi robotic arm by developing a complete workflow for **robot motion creation, action management, emotion-based triggering, and interaction through a web interface**.

The system consists of two major components:

1. **ArmPi Studio** – A custom desktop interface for creating and organizing robotic-arm poses and motion sequences.
2. **Robot Emotion Web Interface** – A Laravel web page that connects video-based emotion recognition with the robotic arm, allowing the robot to perform predefined actions according to the detected emotion.

The overall workflow is:

```text
Camera / Video
      ↓
6-Second Video Clip
      ↓
Multimodal Emotion Recognition
      ↓
Detected Emotion
      ↓
Emotion → Robot Action Mapping
      ↓
Raspberry Pi Robot Controller
      ↓
ArmPi Robotic Arm
      ↓
Action Execution
      ↓
Record Emotion + Action + Video
