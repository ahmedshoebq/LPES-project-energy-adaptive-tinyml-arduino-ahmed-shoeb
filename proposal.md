# Project Proposal

## Title
Energy-Adaptive TinyML: Dynamic Model Selection Based on 
Available Power on Arduino Nano 33 BLE Sense Rev2

## Student
Ahmed Shoeb — University of Trento (2025-2026)

## Course
Low-power Embedded Systems — Prof. Kasim Sinan Yildirim

---

## Problem Statement

Always-on embedded AI devices must operate continuously on 
limited battery. Current TinyML deployments use a single fixed 
model that consumes the same energy regardless of how much 
power remains. This is wasteful and can cause the device to 
fail prematurely.

## Proposed Solution

We implement a three-tier adaptive inference system:
- When battery is high — run a large accurate model
- When battery is medium — run a medium balanced model  
- When battery is low — run a small fast model

The system switches automatically in real time with no user 
intervention.

## Why This Is Novel
- Directly addresses the core course theme of low-power operation
- Applicable to real wearable and IoT devices
- Results provide concrete energy-accuracy trade-off data

## Application

Gesture recognition using the built-in IMU on Arduino Nano 33 
BLE Sense Rev2. Four classes: rest, shake, updown, circle.

## Expected Outcome

A working Arduino system that:
1. Classifies gestures correctly using all three model tiers
2. Switches models automatically as battery level drops
3. Prints active model, prediction, confidence and inference 
   time to Serial Monitor


## Timeline

| Week | Milestone |
|---|---|
| 1 | Data collection + model training |
| 2 | TFLite export + Arduino deployment |
| 3 | Evaluation + measurements |
| 4 | Report writing + submission |

## GitHub Repository
https://github.com/ahmedshoebq/LPES-project-energy-adaptive-tinyml-arduino-ahmed-shoeb/new/main
