# 🚗 Vehicle Detection using Computer Vision

A Computer Vision project for detecting and counting vehicles in images and videos using **Python, OpenCV, Roboflow Workflow API, and REST API communication**.

## 📌 Project Overview

This project uses the **Roboflow Workflow API** to perform vehicle detection.

The input image or video frame is converted into **Base64 format** and sent to the Roboflow API using Python's `requests` library.

The API returns vehicle predictions, which are processed using OpenCV to:

- Detect vehicles
- Draw bounding boxes
- Display vehicle classes
- Display confidence scores
- Count detected vehicles
- Process video frames

## 🚀 Features

- Image-based vehicle detection
- Video-based vehicle detection
- Multiple vehicle detection
- Bounding box visualization
- Confidence score display
- Vehicle counting
- Base64 image encoding
- REST API integration
- Frame-by-frame video processing
- Frame skipping to reduce API requests

## 🚘 Supported Vehicle Classes

| Vehicle | Class |
|---|---|
| Car | `car` |
| Truck | `truck` |
| Bus | `bus` |
| Bicycle | `bicycle` |
| Motorcycle | `motorcycle` |

## 🛠️ Technologies Used

- Python
- OpenCV
- Requests
- Roboflow Workflow API
- REST API
- Base64 Encoding
- Google Colab
- Jupyter Notebook

## 🧠 System Architecture

```text
             Input
               │
        ┌──────┴──────┐
        │             │
      Image         Video
        │             │
        │        Extract Frames
        │             │
        └──────┬──────┘
               │
        Base64 Encoding
               │
               ▼
      Roboflow Workflow API
               │
               ▼
       Vehicle Predictions
               │
               ▼
      Extract Predictions
               │
               ▼
       Confidence Filtering
               │
               ▼
       OpenCV Bounding Boxes
               │
               ▼
       Vehicle Count + Label
               │
               ▼
          Final Output
```

## 🔄 Image Detection Pipeline

```text
Upload Image
     ↓
Read Image using OpenCV
     ↓
Convert Image to Base64
     ↓
Send Request to Roboflow API
     ↓
Receive JSON Response
     ↓
Extract Predictions
     ↓
Filter Low Confidence Predictions
     ↓
Calculate Bounding Boxes
     ↓
Draw Bounding Boxes
     ↓
Display Vehicle Class + Confidence
     ↓
Save Detection Result
```

## 🎥 Video Detection Pipeline

```text
Input Video
     ↓
Read Video Frames
     ↓
Every 5th Frame
     ↓
Send Frame to Roboflow
     ↓
Receive Predictions
     ↓
Draw Latest Predictions
     ↓
Write Output Frame
     ↓
Generate Output Video
```

The project sends every **5th frame** to the API to reduce the number of API requests.

## 🔌 Roboflow API Integration

The project communicates with Roboflow using a REST API request.

The image is converted into Base64 and included in the JSON request.

Example:

```python
payload = {
    "api_key": API_KEY,
    "inputs": {
        "image": {
            "type": "base64",
            "value": image_base64
        },
        "classes": "car, truck, bus, bicycle, motorcycle"
    }
}

response = requests.post(
    API_URL,
    headers={"Content-Type": "application/json"},
    json=payload
)
```

## 🎯 Confidence Threshold

The project uses a confidence threshold of:

```text
0.30
```

Predictions below this confidence level are ignored.

```python
if confidence < 0.30:
    continue
```

## 📦 Project Structure

```text
vehicle-detection-computer-vision/
│
├── Vehicle_detection.ipynb
├── README.md
├── requirements.txt
├── .gitignore
├── .env
│
└── output/
    ├── vehicle_detection_result.jpg
    └── vehicle_detection_output.mp4
```

> The `.env` file must not be pushed to GitHub.

## ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/vehicle-detection-computer-vision.git
```

Move into the project directory:

```bash
cd vehicle-detection-computer-vision
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## 🔐 Environment Variables

Create a `.env` file:

```env
ROBOFLOW_API_KEY=your_roboflow_api_key
```

Load the API key in Python:

```python
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv("ROBOFLOW_API_KEY")

if not API_KEY:
    raise ValueError("ROBOFLOW_API_KEY not found")
```

### ⚠️ Security

Never upload your `.env` file to GitHub.

Your `.gitignore` should contain:

```gitignore
.env
```

If an API key has already been exposed publicly, revoke it and generate a new key before making the repository public.

## ▶️ Running the Project

Open:

```text
Vehicle_detection.ipynb
```

The notebook can be run using:

- Google Colab
- Jupyter Notebook
- VS Code with Jupyter extension

### Image Detection

1. Upload an image.
2. Read the image using OpenCV.
3. Convert the image to Base64.
4. Send it to the Roboflow Workflow API.
5. Extract predictions.
6. Filter low-confidence predictions.
7. Draw bounding boxes.
8. Display vehicle class and confidence.
9. Save the detection result.

### Video Detection

1. Upload a video.
2. Read video properties.
3. Extract frames using OpenCV.
4. Send every 5th frame to Roboflow.
5. Receive vehicle predictions.
6. Draw the latest predictions.
7. Write the processed frames.
8. Generate the output video.

## 📊 Detection Logic

Roboflow provides bounding box information using:

```text
x
y
width
height
```

The project converts the center coordinates into corner coordinates:

```python
x1 = int(x - width / 2)
y1 = int(y - height / 2)

x2 = int(x + width / 2)
y2 = int(y + height / 2)
```

OpenCV then draws the bounding box:

```python
cv2.rectangle(
    output,
    (x1, y1),
    (x2, y2),
    (0, 255, 0),
    3
)
```

## 📈 Vehicle Counting

The project maintains separate counters:

```python
counts = {
    "car": 0,
    "truck": 0,
    "bus": 0,
    "bicycle": 0,
    "motorcycle": 0
}
```

The total number of detected vehicles is calculated using:

```python
total = sum(counts.values())
```

## 📁 Output Files

### Image Output

```text
vehicle_detection_result.jpg
```

Contains:

- Detected vehicles
- Bounding boxes
- Vehicle class
- Confidence score

### Video Output

```text
vehicle_detection_output.mp4
```

Contains:

- Vehicle bounding boxes
- Vehicle labels
- Confidence scores
- Vehicle counts

## ⚠️ Current Limitations

- Detection depends on the Roboflow API.
- Internet connection is required.
- API requests may be limited by the available API plan.
- Video processing can take time because frames are sent to an external API.
- Frame skipping may cause some short-lived detections to be missed.
- Detection performance depends on image quality, lighting, camera angle, and the underlying detection model.

## 🔮 Future Improvements

- Add vehicle tracking across video frames
- Improve detection of multiple vehicles
- Add license plate detection
- Add vehicle speed estimation
- Add traffic density analysis
- Add vehicle entry/exit counting
- Add line-crossing detection
- Use a local YOLO model for faster processing
- Optimize video processing performance
- Deploy the application as an online service

## 🎓 Learning Outcomes

Through this project, I learned:

- Computer Vision fundamentals
- Object detection concepts
- OpenCV image and video processing
- REST API integration
- Roboflow Workflow API
- Base64 image encoding
- JSON request/response handling
- Bounding box coordinate conversion
- Confidence thresholding
- Video frame processing
- Vehicle counting
- API-based Computer Vision applications

## 📌 Project Highlights

```text
✔ Image Vehicle Detection
✔ Video Vehicle Detection
✔ Multiple Vehicle Classes
✔ Bounding Boxes
✔ Confidence Scores
✔ Vehicle Counting
✔ REST API Integration
✔ OpenCV Processing
✔ Base64 Encoding
✔ Frame Skipping
```

## 👨‍💻 Author

**Aakash Kumar**

B.Tech CSE – Data Science

Interested in:

- Data Science
- Machine Learning
- Computer Vision
- Generative AI
- Artificial Intelligence

## 📄 License

This project is created for educational and portfolio purposes.

## ⭐ Acknowledgement

Vehicle detection functionality is powered by the **Roboflow Workflow API**, while image and video processing is handled using **OpenCV and Python**.
