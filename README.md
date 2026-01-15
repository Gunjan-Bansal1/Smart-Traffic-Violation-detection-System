# Smart Traffic Violation Detection System

## 📌 Project Overview
The **Smart Traffic Violation Detection System** is an AI-powered computer vision project designed to automate the monitoring of traffic violations. Utilizing Deep Learning models (YOLO) and computer vision techniques (OpenCV), the system analyzes video feeds to detect types of traffic non-compliance in real-time, specifically focusing on **red light jumping**, **over-speeding**, and **wrong lane driving**.

This project aims to replace manual, error-prone surveillance with an efficient, automated, and scalable solution for modern smart cities.

---

## 📖 Table of Contents
- [Problem Statement](#-problem-statement)
- [Objectives & Scope](#-objectives--scope)
- [System Architecture](#-system-architecture)
- [Key Features](#-key-features)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Violation Rules](#-violation-rules)
- [How to Run](#-how-to-run)
- [Future Enhancements](#-future-enhancements)

---

## ❓ Problem Statement
Modern cities are equipped with numerous CCTV cameras, yet traffic management relies heavily on manual monitoring. This approach is:
- **Inefficient**: authorities cannot monitor every feed 24/7.
- **Error-Prone**: human fatigue leads to missed violations.
- **Non-Scalable**: as traffic volume increases, manual verification becomes impossible.

Major causes of road fatalities/injuries include riding without a helmet and jumping red lights. An automated system is required to enforce these rules strictly.

---

## 🎯 Objectives & Scope
**Objective**: Build an automated pipeline to process traffic video, detect violations, and generate visual evidence.

**Scope**:
1.  **Red Light Violation**: Detect vehicles crossing the stop line when the signal is Red.
2.  **Over Speed Violation**: Identify vehicles exceeding the calculated dynamic speed threshold.
3.  **Wrong Lane Violation**: Detect vehicles driving in prohibited or non-designated lanes.
4.  **Evidence Storage**: Save structured records and snapshots of the violation.

*> Note: License Plate Recognition (ALPR) and automatic challan generation are currently out of scope for this version.*

---

## 🏗️ System Architecture

The system follows a sequential pipeline to process video data:

graph TD
    A[Input Video Feed] --> B[Video Preprocessing]
    B --> C[Object Detection (YOLO)]
    C --> D[Object Tracking]
    D --> E{Violation Rule Engine}
    E -->|Rule 1| F[Red Light Check]
    E -->|Rule 2| G[Speed Check]
    E -->|Rule 3| H[Lane Compliance Check]
    F -->|Violation Detected| I[Generate Evidence]
    G -->|Violation Detected| I
    H -->|Violation Detected| I
    I --> J[Save Record & Image]
    I --> K[Dashboard Visualization]

1.  **Input**: Raw CCTV footage (`mp4`).
2.  **Preprocessing**: Frame extraction, resizing, and normalization using OpenCV.
3.  **Detection**: YOLO model identifies objects: `Car`, `Bus`, `Motorbike`, `Truck`, `Traffic Light`.
4.  **Tracking**: Assigns unique IDs to vehicles to track movement across frames (preventing double counting) using Euclidean distance.
5.  **Rule Engine**: Logic blocks check spatial and temporal compliance (e.g., speed calculation between frames, polygon inclusion for lanes).
6.  **Output**: Violation logs (`csv`) and annotated images.

---

## ✨ Key Features
- **Real-Time Detection**: Processes video frames efficiently.
- **Multi-Class Detection**: Identifies vehicles, pedestrians, and traffic signals simultaneously.
- **Robust Rule Engine**: Deterministic logic to reduce false positives.
- **Evidence Logging**: Automatically saves high-resolution snapshots of the infringing vehicle with timestamps.

---

## 🛠️ Technology Stack
- **Language**: Python
- **Computer Vision**: OpenCV (`cv2`)
- **Deep Learning Model**: YOLO (You Only Look Once) - for object detection.
- **Data Manipulation**: Pandas, NumPy
- **Environment**: Google Colab / Jupyter Notebook

---

## 📂 Project Structure
```bash
Smart_Traffic_Violation_Detection/
├── data/
│   ├── raw_videos/          # Input video files
│   └── processed_frames/    # Intermediate processing frames
├── models/
│   └── yolo/                # YOLO model weights and configs
├── notebooks/
│   └── Traffic_Violation.ipynb  # Main project code (Colab/Jupyter)
├── outputs/
│   └── violation_records/   # Generated CSV logs and evidence images
├── project_docs/            # Documentation files
│   ├── problem_statement.md
│   ├── project_workflow.md
│   ├── system_architecture.md
│   └── violation_rules.md
└── README.md                # Project documentation
```

---

## 📜 Violation Rules

### 1. Red Light Violation
- **Logic**: IF `Traffic Light` state is **RED** AND a `Vehicle` (Car/Bus/Bike) crosses the defined **Stop Line** region (y-coordinate threshold) in the frame.
- **Action**: Flag as violation.

### 2. Over Speed Violation
- **Logic**: Calculates the Euclidean distance of a vehicle's centroid between consecutive frames to determine relative speed (pixels/sec). IF speed > **Dynamic Threshold** (defined as the 95th percentile of observed traffic speeds).
- **Action**: Flag as "Over-Speeding".

### 3. Wrong Lane Violation
- **Logic**: Usage of pre-defined polygons (Regions of Interest) to map "Allowed" and "Wrong" lanes. IF a vehicle's center point coordinates fall inside the **Wrong Lane Polygon**.
- **Action**: Flag as "Wrong Lane" violation.

*> Principles: All violations must be deterministic. Probabilistic guesses are avoided to ensure fair enforcement.*

---

## 🚀 How to Run

1.  **Open the Notebook**:
    Navigate to `notebooks/Traffic_Violation.ipynb` and open it in Google Colab or a local Jupyter environment.

2.  **Setup Environment**:
    Run the initial cells to mount Google Drive (if using Colab) and install dependencies:
    ```python
    !pip install ultralytics opencv-python-headless pandas numpy
    ```

3.  **Prepare Data**:
    Upload your test video to `data/raw_videos/`. Update the `VIDEO_PATH` variable in the notebook if necessary.

4.  **Execute Phase Blocks**:
    Run the notebook cells sequentially to:
    - Load the model.
    - Process the video.
    - Run the detection loop.
    - View the generated outputs in `outputs/`.

---

## 🔮 Future Enhancements
- **Automatic Number Plate Recognition (ANPR)**: To read license plates for ticketing.
- **Night Vision Support**: Enhancing model for low-light conditions.
- **Speed Detection**: Estimating vehicle speed using frame-to-frame tracking.
- **Cloud Integration**: Real-time uploading of violations to a central server.
