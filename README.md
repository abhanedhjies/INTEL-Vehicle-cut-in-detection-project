# Vehicle Cut-in Detection

This repository contains a real-time vehicle cut-in detection system using YOLOv8 for object detection, distance estimation, and time-to-collision calculation. It enhances driver safety by alerting potential collision risks based on detected vehicle proximity and speed during cut-in scenarios.

## Objectives

1. **Real-time Cut-in Detection**: Implement a system capable of detecting vehicles in real-time using YOLOv8 object detection.
2. **Distance Estimation**: Calculate the distance of detected vehicles from the camera using focal length and known object width.
3. **Collision Risk Assessment**: Assess collision risk based on vehicle proximity to predefined distance thresholds.
4. **Time-to-Collision (TTC) Calculation**: Compute the time-to-collision metrics for vehicles approaching dangerous proximity levels during cut-in events.
5. **Visual Warning System**: Provide visual annotations and warnings on video streams to alert drivers of high collision risk scenarios.
6. **Integration and Deployment**: Prepare the system for integration into existing vehicle safety systems or standalone deployment scenarios.

## Features

- **Real-time Object Detection**: Leverages YOLOv8 for high-performance vehicle detection in real-time.
- **Distance Calculation**: Utilizes camera calibration data to estimate the distance of detected vehicles.
- **Collision Risk Analysis**: Determines collision risk using predefined safety thresholds.
- **TTC Metrics**: Calculates time-to-collision to provide timely warnings.
- **Visual Warnings**: Displays warnings and annotations directly on the video feed.
- **Easy Integration**: Designed for seamless integration into existing vehicle systems or as a standalone solution.

## Getting Started

### Prerequisites

- Python 3.6+
- YOLOv8
- OpenCV
- Numpy
- Other dependencies specified in `requirements.txt`

### Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/vehicle-cut-in-detection.git
    ```
2. Navigate to the project directory:
    ```sh
    cd vehicle-cut-in-detection
    ```
3. Install the required dependencies:
    ```sh
    pip install -r requirements.txt
    ```

### Usage

1. **Run the Detection System**:
    ```sh
    python detect.py
    ```
2. **Configure Parameters**: Modify the configuration file `config.yaml` to set camera parameters, detection thresholds, and other settings.

### Example

```python
import cv2
from yolo import YOLOv8
from distance import calculate_distance
from ttc import compute_ttc

# Initialize YOLOv8 model
model = YOLOv8("yolov8.weights")

# Capture video feed
cap = cv2.VideoCapture("video.mp4")

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break

    # Detect vehicles
    detections = model.detect(frame)

    for detection in detections:
        x, y, w, h = detection
        distance = calculate_distance(w)
        ttc = compute_ttc(distance, speed)

        # Draw bounding box and distance
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
        cv2.putText(frame, f"Distance: {distance:.2f}m, TTC: {ttc:.2f}s", (x, y-10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

    # Display the frame
    cv2.imshow("Cut-in Detection", frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss any changes or improvements.

### License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Acknowledgments

- YOLOv8: [YOLOv8 GitHub](https://github.com/ultralytics/yolov8)
- OpenCV: [OpenCV GitHub](https://github.com/opencv/opencv)

