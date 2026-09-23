# 🔍 Real-World Object & Face Detection System

An AI-powered real-time computer vision system that combines **YOLOv8 object detection** and **face recognition** to detect objects and identify known/wanted persons using a live camera feed.

The system also provides:

* 🚨 Wanted-person detection and alarm alerts
* 👤 Face recognition
* 🎯 Real-world object detection using YOLOv8
* 📸 Automatic snapshots
* 💾 MongoDB detection logging
* 📊 Flask-based monitoring dashboard
* 🎛️ Remote system controls
* 🔊 Audio alarm system
* 📈 Detection statistics

---

## 🚀 Project Overview

This project is designed as a real-time intelligent surveillance and detection system.

The application captures frames from a camera and processes them using two computer-vision pipelines:

```text
                    Camera
                       │
                       ▼
                 Video Stream
                       │
             ┌─────────┴─────────┐
             │                   │
             ▼                   ▼
      Face Recognition       YOLOv8 Detection
             │                   │
             ▼                   ▼
       Known/Wanted Face     Object Detection
             │                   │
             └─────────┬─────────┘
                       │
                       ▼
                 Detection Event
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Snapshot      MongoDB       Alarm
          │            │            │
          └────────────┼────────────┘
                       ▼
                Flask Dashboard
```

---

# ✨ Features

## 👤 Face Recognition

The system can detect faces from the live camera feed and compare them with known faces stored in the `wanted` directory.

Example:

```text
wanted/
├── person1.jpg
├── person2.jpg
└── person3.jpg
```

The filename is used as the person's name.

For example:

```text
wanted/shivam.jpg
```

can be displayed as:

```text
Shivam
```

The system uses the `face_recognition` library for face encoding and matching.

---

## 🚨 Wanted Person Detection

When a known face matches a person in front of the camera:

* The person is identified.
* A red bounding box is displayed.
* A wanted alert is generated.
* An alarm can be triggered.
* A snapshot can be saved.
* The detection can be stored in MongoDB.

Example:

```text
🎯 MATCH: shivam
🚨 WANTED PERSON DETECTED!
```

---

# 🎯 Real-World Object Detection

The project uses **YOLOv8** for real-time object detection.

The repository contains:

```text
yolov8n.pt
```

which is the YOLOv8 Nano model.

Detected objects are displayed with:

* Bounding boxes
* Object names
* Confidence scores

Example:

```text
person 0.94
cell phone 0.87
bottle 0.82
```

---

# 📸 Automatic Snapshots

The system automatically saves detection images.

The following directories are created automatically:

```text
captures/
├── faces/
└── objects/
```

Example:

```text
captures/
├── faces/
│   ├── face_20260923-120100.jpg
│   └── shivam_20260923-120105.jpg
│
└── objects/
    ├── person_20260923-120110.jpg
    └── bottle_20260923-120115.jpg
```

Snapshots are throttled using cooldown intervals to prevent excessive image creation.

---

# 🔊 Alarm System

The project supports multiple alarm audio files:

```text
alarm.mp3
alarm.wav
simple_alarm.wav
```

The system attempts to play a custom alarm for a detected wanted person.

If a custom audio file is not available, it falls back to the default alarm.

On Windows, the system can also use `winsound` as a fallback.

---

# 💾 MongoDB Integration

Detection information can be stored in MongoDB.

Default configuration:

```text
MongoDB
│
└── facedetection
    │
    └── detections
```

Example detection document:

```json
{
  "timestamp": "20260923-120100",
  "type": "wanted_face",
  "name": "shivam",
  "distance": 0.42,
  "snapshot": "captures/faces/shivam_20260923-120100.jpg",
  "alert": true
}
```

Object detections can contain:

```json
{
  "timestamp": "20260923-120105",
  "type": "object",
  "name": "person",
  "confidence": 0.94,
  "alert": false
}
```

---

# 📊 Monitoring Dashboard

The project includes a Flask dashboard backend.

The dashboard provides APIs for:

```text
/api/stats
/api/detections
/api/detections/hourly
/api/system/control
```

The dashboard can provide information such as:

* Wanted detections
* Object detections
* Total face captures
* Active alerts
* Recent detections
* System status
* Face detection status
* Object detection status
* Alarm status
* MongoDB status

---

# 🎛️ System Controls

The Flask dashboard API supports several control commands.

### Silence Alarm

```text
silence_alarm
```

Temporarily silences the alarm.

### Capture Snapshot

```text
capture_snapshot
```

Requests a manual snapshot.

### Toggle Face Detection

```text
toggle_face_detection
```

Enables or disables face detection.

### Toggle Object Detection

```text
toggle_object_detection
```

Enables or disables object detection.

### Toggle Alarm

```text
toggle_alarm
```

Enables or disables the alarm.

### Emergency Stop

```text
emergency_stop
```

Stops the system.

### Start System

```text
start_system
```

Starts the system again.

---

# 🧠 Technologies Used

| Technology       | Purpose                        |
| ---------------- | ------------------------------ |
| Python           | Main programming language      |
| OpenCV           | Camera and image processing    |
| YOLOv8           | Object detection               |
| Ultralytics      | YOLO implementation            |
| face-recognition | Face detection and recognition |
| NumPy            | Numerical processing           |
| Pillow           | Image processing               |
| Flask            | Dashboard/API server           |
| Flask-CORS       | Cross-origin requests          |
| MongoDB          | Detection storage              |
| PyMongo          | MongoDB Python driver          |
| playsound        | Alarm playback                 |

---

# 📁 Project Structure

```text
detect-real-world-object-from-a-photo-/
│
├── main.py
├── dashboard.py
├── config.py
├── control_state.py
├── control_state.json
├── mongo_debug.py
│
├── requirements.txt
│
├── yolov8n.pt
│
├── alarm.mp3
├── alarm.wav
├── simple_alarm.wav
│
├── camera_test_700.jpg
│
├── wanted/
│   └── your_known_faces.jpg
│
└── captures/
    ├── faces/
    └── objects/
```

The `wanted` and `captures` directories are created/used by the application.

---

# ⚙️ Requirements

Recommended environment:

* Python 3.9–3.11
* Windows/Linux environment with a compatible camera
* Webcam
* MongoDB (for database functionality)

The project dependencies are listed in:

```text
requirements.txt
```

---

# 📦 Installation

## 1. Clone the repository

```bash
git clone https://github.com/shivampajiyar29/detect-real-world-object-from-a-photo-.git
```

Move into the project directory:

```bash
cd detect-real-world-object-from-a-photo-
```

---

## 2. Create a Virtual Environment

Windows:

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

The main dependencies include:

```text
opencv-python
numpy
face-recognition
ultralytics
pillow
flask
flask-cors
pymongo
playsound
scipy
```

---

# 🗄️ MongoDB Setup

Install and start MongoDB locally.

The current project configuration uses:

```text
mongodb://localhost:27017/
```

Database:

```text
facedetection
```

Collection:

```text
detections
```

You can test the MongoDB connection with:

```bash
python mongo_debug.py
```

Expected output when MongoDB is running:

```text
Connected to MongoDB collection facedetection.detections
```

---

# 👤 Adding Known Faces

Create a directory:

```text
wanted/
```

Place known-person images inside it.

Example:

```text
wanted/
├── shivam.jpg
├── person1.jpg
└── person2.jpg
```

The system will load the images when `main.py` starts.

The image should contain a clearly visible face.

If no face is found, the image is skipped.

---

# ▶️ Running the Detection System

Start the main detection application:

```bash
python main.py
```

The application will:

1. Initialize the camera.
2. Load known faces.
3. Load the YOLOv8 model.
4. Connect to MongoDB.
5. Start face recognition.
6. Start object detection.
7. Save detection snapshots.
8. Store detection information in MongoDB.
9. Display the live detection window.

---

# 🎥 Camera Controls

The detection window supports:

```text
Q → Quit
S → Take manual snapshot
```

The default camera index is:

```python
CAMERA_INDEX = 0
```

If the webcam does not open, try another index:

```python
CAMERA_INDEX = 1
```

or:

```python
CAMERA_INDEX = 2
```

---

# 📊 Running the Dashboard

Start the Flask dashboard separately:

```bash
python dashboard.py
```

The dashboard server runs on:

```text
http://localhost:5000
```

The dashboard backend provides REST APIs for system monitoring and control.

---

# 🔌 API Endpoints

## System Statistics

```http
GET /api/stats
```

Returns:

* Detection counts
* System status
* Camera status
* MongoDB status
* Recent detections
* Alarm status

---

## Hourly Detection Statistics

```http
GET /api/detections/hourly
```

Returns detection statistics for the previous 24 hours.

---

## Detection Records

```http
GET /api/detections
```

Supports pagination.

Example:

```text
/api/detections?page=1&limit=20
```

---

## System Control

```http
POST /api/system/control
```

Example request:

```json
{
  "command": "toggle_alarm"
}
```

Supported commands include:

```text
silence_alarm
capture_snapshot
toggle_face_detection
toggle_object_detection
toggle_alarm
emergency_stop
start_system
```

---

# ⚙️ Configuration

Main configuration can be found in:

```text
config.py
```

Important settings include:

```python
CAMERA_INDEX = 0
CAMERA_WIDTH = 640
CAMERA_HEIGHT = 480
CAMERA_FPS = 30

YOLO_WEIGHTS = "yolov8n.pt"
YOLO_CONF = 0.5
YOLO_IMGSZ = 320

FACE_TOLERANCE = 0.6

MONGO_URI = "mongodb://localhost:27017/"
MONGO_DB = "facedetection"
MONGO_COLLECTION = "detections"
```

---

# 🛡️ Detection Logic

## Face Recognition

The system uses face encodings to compare detected faces against the known faces.

The matching threshold is controlled by:

```python
FACE_TOLERANCE = 0.6
```

A lower tolerance generally means stricter matching.

---

## Object Detection

YOLOv8 processes camera frames and returns:

```text
Object
Confidence
Bounding Box
```

The configured confidence threshold is:

```python
YOLO_CONF = 0.5
```

---

# ⚡ Performance Optimization

The system includes several optimizations:

* Processes every second frame for detection.
* Resizes frames before detection.
* Uses YOLOv8 Nano.
* Uses HOG-based face detection.
* Uses background threads for camera processing.
* Uses snapshot cooldowns.
* Uses alarm cooldowns.

These techniques help reduce CPU usage and improve real-time performance.

---

# 🐛 Troubleshooting

## Camera does not open

Try changing:

```python
CAMERA_INDEX = 0
```

to:

```python
CAMERA_INDEX = 1
```

Also make sure another application is not using the webcam.

---

## MongoDB connection failed

Make sure MongoDB is running.

The default connection is:

```text
mongodb://localhost:27017/
```

Test using:

```bash
python mongo_debug.py
```

---

## YOLO model error

Make sure this file exists in the project root:

```text
yolov8n.pt
```

The repository already includes this model.

---

## No face detected

Make sure the image inside `wanted/`:

* Contains a visible face.
* Is not extremely blurry.
* Has sufficient lighting.
* Uses `.jpg`, `.jpeg`, or `.png`.

---

## Alarm does not play

Check that one of these files exists:

```text
alarm.wav
simple_alarm.wav
alarm.mp3
```

On Windows, the application also attempts to use `winsound` as a fallback.

---

# ⚠️ Current Repository Notes

The current GitHub repository contains the core Python detection application, YOLO model, audio files, configuration, and MongoDB integration.

However, the dashboard code references:

```python
render_template('dashboard.htm')
```

Therefore, a corresponding Flask template must be available under the expected `templates` directory for the dashboard homepage to render correctly.

The repository should also be configured with the required runtime environment before deployment.

---

# 🔐 Security Recommendations

Before deploying this system publicly:

* Move MongoDB credentials to environment variables.
* Do not expose MongoDB directly to the internet.
* Add authentication to dashboard control APIs.
* Validate API requests.
* Restrict CORS in production.
* Protect stored face images.
* Avoid committing private/personally identifiable data.
* Add rate limiting to control endpoints.
* Use HTTPS for remote dashboard access.

---

# 🚀 Future Improvements

Possible improvements include:

* Web-based live camera streaming
* User authentication
* Role-based access control
* Mobile-responsive dashboard
* Real-time WebSocket updates
* Email/SMS alerts
* Telegram notifications
* Cloud MongoDB support
* Multiple camera support
* Person tracking
* Object-specific alerts
* Improved face recognition models
* GPU acceleration
* Docker deployment
* REST API authentication
* Detection history filtering
* Advanced analytics
* Export reports as CSV/PDF

---

# 📌 Project Purpose

This project demonstrates the integration of multiple AI and software technologies into a real-time computer vision system:

```text
Computer Vision
       +
Face Recognition
       +
YOLO Object Detection
       +
MongoDB
       +
Flask REST APIs
       +
Real-Time Camera Processing
       +
Alert System
```

It can serve as a foundation for an intelligent surveillance and computer-vision application.

---

# 👨‍💻 Author

**Shivam Pajiyar**

GitHub:

https://github.com/shivampajiyar29

---

# 📄 License

No license is currently specified for this repository.
