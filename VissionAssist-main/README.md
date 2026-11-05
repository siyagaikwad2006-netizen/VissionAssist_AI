# VisionAssist AI 2.0 👁️‍🗨️
### Real-Time Assistive Vision, High-Precision Perception & Accessible Campus Navigation for Blind and Low-Vision Students

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.x-0175C2?logo=dart&logoColor=white)](https://dart.dev)
[![Google ML Kit](https://img.shields.io/badge/Perception-Google%20ML%20Kit-4285F4?logo=google&logoColor=white)](https://developers.google.com/ml-kit)
[![YOLOv8](https://img.shields.io/badge/Cloud%20Vision-YOLOv8--Nano-00FFFF?logo=ultralytics&logoColor=black)](https://ultralytics.com)
[![Platform](https://img.shields.io/badge/Platform-Android%20(API%2024%2B)-3DDC84?logo=android&logoColor=white)](https://developer.android.com)

---

## 📌 Project Overview

**VisionAssist AI 2.0** is an intelligent, voice-first assistive Android application engineered for blind, visually impaired, and low-vision students. It converts real-time camera imagery into spatial directional audio warnings and tactile haptic vibration pulses, enabling safe and autonomous navigation across educational campuses.

The system features a **Dual-Engine Perception Architecture**:
1. **Edge-Native Perception (Google ML Kit)**: Executes directly on the mobile device with sub-250ms latency for zero-lag obstacle and person detection in offline campus corridors.
2. **Cloud-Native Inference (YOLOv8-Nano)**: Connects to a cloud/LAN FastAPI inference server with DeepSORT centroid tracking and MiDaS monocular depth estimation.

---

## 🌟 Key Features & Technical Highlights

- **High-Precision Obstacle & Person Detection**:
  - **Ground-Plane Pinhole Geometry**: Calculates physical distance ($0.6\text{m} - 3.8\text{m}$) using perspective projection of the object's ground-contact line.
  - **Class-Specific Reference Scale**: Fuses physical dimensions (Person: $1.7\text{m}$, Door: $2.0\text{m}$, Chair: $0.8\text{m}$, Stairs: $1.2\text{m}$, Laptop: $0.3\text{m}$) with bounding box height.
  - **Dedicated Person Pipeline**: Over 40 clothing, facial, body, and posture attributes map to **Person** with top navigational priority (`priority: 12`), backed by upright humanoid aspect ratio verification.
  - **Directional Spatial Tracking**: Identifies whether obstacles are to the `left`, `directly ahead`, or to the `right`, as well as broad obstacles blocking the entire corridor.
- **Dual-Channel Sensory Alerting**:
  - Natural spoken directional warnings in **5 languages** (English, Hindi, Marathi, Spanish, Tamil).
  - Multi-tier tactile hardware vibration pulses: **Critical Stop (700ms)** for $< 0.9\text{m}$, **Warning (400ms)** for $0.9\text{m} - 1.5\text{m}$.
- **Temporal EMA Smoothing & Anti-Spam Cooldown**:
  - Exponential Moving Average filter ($d_t = 0.65 d_{\text{new}} + 0.35 d_{t-1}$) prevents distance jitter.
  - Direction hysteresis prevents flicker between left and ahead.
  - Intelligent audio cooldown silences repetitive alerts for stationary objects while immediately interrupting if an obstacle enters critical proximity ($< 1.5\text{m}$).
- **Dynamic AR HUD Viewfinder (Screen 03)**:
  - Real-time bounding box overlay with targeting reticles, person head locator circles, and overhead class/distance/confidence tags.
  - SafePath perspective corridor guidelines showing the forward walking path.
  - 6 interactive simulation chips (**Person 1.2m Near**, **Person 1.8m Ahead**, **Chair 1.2m**, **Stairs 0.9m**, **Closed Door 2.1m**, **Laptop 0.8m**, and **Clear Path**) for bench testing.
- **Campus Navigation Engine (Nowrosjee Wadia College, Pune)**:
  - Turn-by-turn spoken guidance between 10 major campus buildings.
  - Dual Mode: Real-time GPS waypoint auto-advancement with manual button fallback.
  - Vector canvas map with live user location, heading orientation, and route polyline.
- **Signboard & Notice Reader (On-Device OCR)**:
  - Google ML Kit Latin Text Recognition reads printed circulars, room numbers, and notices aloud.
- **Hands-Free Voice Command Assistant**:
  - Natural multilingual speech triggers (*"Detect"*, *"Read"*, *"Library"*, *"Emergency"*).
- **Emergency SOS Hub**:
  - One-tap security dispatch (Ext. 0199) with live GPS sharing and tactile confirmation.

---

## 🏛️ System Architecture

```
 VisionAssist AI 2.0
 ├── vision_assist_app/          # Flutter Native Android Application
 │   ├── lib/main.dart           # Complete 12-Screen Accessibility UI, AR HUD & Perception Core
 │   ├── lib/wadia_campus_navigation.dart # Campus Graph, Topological Engine & Vector Map
 │   ├── android/                # Native Android configuration, CameraX & Gradle
 │   ├── .github/workflows/      # Automated Cloud CI/CD APK Build Pipeline
 │   └── pubspec.yaml            # TTS, Camera, ML Kit, and Sensor dependencies
 └── VisionAssist_Web/           # Backend MLOps, Model Storage & Audit Pipeline
     ├── app/main.py             # FastAPI service & Fleet Memory Hub
     ├── models/                 # Quantized YOLOv8-Nano weights & metadata
     └── runs/safeloop/          # SAFE-Loop continuous validation & benchmark suite
```

---

## 📱 Quick Start & Installation

1. **Direct APK Installation**:
   - Transfer `VisionAssist_AI.apk` to your Android device and install via your file manager.
2. **Automated Cloud Build (GitHub Actions)**:
   - Push to a GitHub repository to trigger the automated build pipeline in `.github/workflows/build_apk.yml` and download the compiled APK in 3 minutes.
3. **Local Flutter Build**:
   ```bash
   cd vision_assist_app
   flutter pub get
   flutter build apk --release
   ```

---

## 📜 License & Acknowledgments
Built with ❤️ for accessible, inclusive campus mobility.
