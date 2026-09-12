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
