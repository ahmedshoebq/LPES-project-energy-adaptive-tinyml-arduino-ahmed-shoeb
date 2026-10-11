# LPES-project-energy-adaptive-tinyml-arduino-ahmed-shoeb
# Energy-Adaptive TinyML on Arduino Nano 33 BLE Sense Rev2

**Course:** Low-power Embedded Systems  
**University:** University of Trento  
**Professor:** Prof. Kasim Sinan Yildirim  
**Student:** Ahmed  Shoeb
**Matricola:** 265924
**Date**:

---

## Overview

Most embedded AI systems use a single fixed model regardless of 
available power. This project challenges that assumption.

We implement a gesture recognition system that **dynamically switches 
between three neural network models** of different sizes based on 
simulated battery level. As power decreases, the system trades 
accuracy for energy efficiency — automatically, in real time, 
entirely on-device.

This addresses a fundamental problem in always-on IoT and wearable 
devices: **how do you keep a device intelligent when its battery is 
dying?**

---

## The Core Idea
Battery > 70% → Large model → High accuracy, slow inference

Battery 30-70% → Medium model → Good accuracy, medium inference

Battery < 30% → Small model → Basic accuracy, fast inference

The device never stops working. It just becomes more efficient.

---

## Hardware

| Component | Details |
|---|---|
| Board | Arduino Nano 33 BLE Sense Rev2 |
| MCU | nRF52840, Cortex-M4, 64 MHz |
| RAM | 256 KB |
| Flash | 1 MB |
| Sensor | BMI270 IMU (built-in) — accelerometer + gyroscope |
| Connection | USB only — no external wiring needed |

---

## Gesture Classes

| Class | Motion |
|---|---|
| Rest | Board held still |
| Shake | Hand shaking left-right |
| Up-Down | Hand moving up and down |
| Circle | Hand drawing circles in air |

---

## Three Model Tiers

| Model | Architecture | Parameters | Size | Target accuracy |
|---|---|---|---|---|
| Large | Dense(128)→Dense(64)→Dense(32)→Output | ~14,000 | ~55 KB | ~92% |
| Medium | Dense(64)→Dense(32)→Output | ~4,000 | ~18 KB | ~87% |
| Small | Dense(32)→Output (int8) | ~2,000 | ~8 KB | ~80% |

---

## Pipeline
IMU (BMI270)
↓

50-sample sliding window at 50 Hz
↓

60 features extracted per window:

Time domain: mean, std, rms, min, max (×6 axes = 30)

Frequency domain: PSD bins k=1..5 (×6 axes = 30)
↓

Feature normalization (StandardScaler)
↓

Dynamic model selection based on battery level
↓

TFLite inference on Arduino
↓

Predicted gesture + confidence + active model → Serial Monitor

## Dynamic Switching Logic

```cpp
if (battery_level > 70) {
    // Use large model — best accuracy
    run_inference(model_large);
} else if (battery_level > 30) {
    // Use medium model — balanced
    run_inference(model_medium);
} else {
    // Use small model — survival mode
    run_inference(model_small);
}
```

Battery level is simulated as a decreasing variable in code.
Future work: connect to actual battery monitor IC.

---

## Evaluation Metrics

|         Metric      |Large|Medium|Small|
|---------------------|-----|-----|------|
| Accuracy (%)        | TBD | TBD | TBD |
| Inference time (ms) | TBD | TBD | TBD |
| Model size (KB)     | TBD | TBD | TBD |
| RAM usage (KB)      | TBD | TBD | TBD |

*Results updated as experiments complete.*

---

## Repository Structure
energy-adaptive-tinyml-arduino/
│
├── README.md
│
├── data/
│ ├── rest.csv
│ ├── shake.csv
│ ├── updown.csv
│ └── circle.csv
│
├── training/
│ └── energy_adaptive_training.ipynb
│
├── models/
│ ├── model_large.h
│ ├── model_medium.h
│ └── model_small.h
│
├── arduino/
│ ├── data_collection/
│ │ └── data_collection.ino
│ └── inference/
│ └── energy_adaptive_inference.ino
│
└── report/
└── paper.pdf


---

## How to Reproduce

### Step 1 — Collect data
1. Open `arduino/data_collection/data_collection.ino`
2. Change `LABEL` to each gesture name
3. Upload to Arduino Nano 33 BLE
4. Hold board in hand and perform gesture
5. Save Serial Monitor output as CSV

### Step 2 — Train models
1. Open `training/energy_adaptive_training.ipynb` in Google Colab
2. Upload all 4 CSV files
3. Run all cells
4. Three model header files download automatically

### Step 3 — Deploy
1. Place all three `.h` files in the inference sketch folder
2. Paste scaler values from notebook output
3. Upload `energy_adaptive_inference.ino` to Arduino
4. Open Serial Monitor at 115200 baud
5. Perform gestures and watch model switch as battery depletes

---

## Key Concepts

| Concept | Role in project |
|---|---|
| TinyML | Running neural networks on microcontrollers |
| Dynamic model selection | Choosing model size based on available energy |
| Feature extraction | Converting IMU signals into model inputs |
| TFLite quantization | Compressing models for embedded deployment |
| Int8 quantization | Reducing Small model size using 8-bit weights |
| Energy-accuracy trade-off | Core research question of the project |

---

## Libraries

| Library | Platform | Purpose |
|---|---|---|
| `Arduino_BMI270_BMM150` | Arduino | IMU sensor interface |
| `TensorFlowLite` | Arduino | On-device inference |
| `arm_math.h` | Arduino | CMSIS DSP math operations |
| `tensorflow` | Colab | Model training |
| `scikit-learn` | Colab | Preprocessing and evaluation |
| `numpy / pandas` | Colab | Data handling |

---

## Results Summary

Full results will be added upon experiment completion.
Preliminary expectation: Small model runs 4-5x faster than Large model
with ~12% accuracy drop — acceptable for a low-battery survival mode.

---

## References

- Pete Warden & Daniel Situnayake — *TinyML* (O'Reilly, 2019)
- TensorFlow Lite for Microcontrollers documentation
- ARM CMSIS DSP Library documentation
- Arduino Nano 33 BLE Sense Rev2 datasheet
- Banbury et al. — *MLPerf Tiny Benchmark* (2021)

---























