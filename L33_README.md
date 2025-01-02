import cv2
import time
from picamera2 import Picamera2

# File paths for model files
PB_FILE = "opencv_face_detector_uint8.pb"
PBTXT_FILE = "opencv_face_detector.pbtxt"

# Verify model files exist
import os
if not (os.path.exists(PB_FILE) and os.path.exists(PBTXT_FILE)):
    raise FileNotFoundError(
        f"Error: Missing model files '{PB_FILE}' or '{PBTXT_FILE}'. Ensure they are in the working directory."
    )

# Load the pre-trained DNN model
model = cv2.dnn_DetectionModel(PBTXT_FILE, PB_FILE)
model.setInputParams(size=(300, 300), scale=1.0)

# Initialize PiCamera2
picam2 = Picamera2()
camera_config = picam2.create_preview_configuration(main={"size": (640, 480)})
picam2.configure(camera_config)
picam2.start()

# Display settings
FONT = cv2.FONT_HERSHEY_SIMPLEX
WINDOW_WIDTH = 600

try:
    print("Starting face detection. Press ESC to exit.")

    while True:
        start_time = time.time()

        # Capture frame from the camera
        frame = picam2.capture_array()
        aspect_ratio = frame.shape[1] / frame.shape[0]
        window_height = int(WINDOW_WIDTH / aspect_ratio)
        frame = cv2.resize(frame, (WINDOW_WIDTH, window_height))
        frame = cv2.flip(frame, 1)

        # Ensure frame is in BGR format
        if frame.shape[2] == 4:
            frame = cv2.cvtColor(frame, cv2.COLOR_RGBA2BGR)

        # Perform face detection
        classes, confidences, boxes = model.detect(frame, confThreshold=0.5)

        for (class_id, confidence, box) in zip(classes, confidences, boxes):
            x, y, w, h = box
            fps = 1 / (time.time() - start_time)
            label = f"FPS: {fps:.1f}, Conf: {confidence * 100:.2f}%"

            # Draw detection box and label
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
            cv2.putText(frame, label, (x, y - 10 if y - 10 > 0 else y + 20), 
                        FONT, 0.5, (0, 255, 0), 1)

        # Display the frame
        cv2.imshow("Face Detection", frame)

        # Break loop on ESC key
        if cv2.waitKey(1) == 27:
            break

except Exception as e:
    print(f"An error occurred: {e}")

finally:
    # Cleanup resources
    cv2.destroyAllWindows()
    picam2.stop()
    print("Face detection terminated.")


![20250102_22h20m12s_grim](https://github.com/user-attachments/assets/d0c977a0-e5eb-4196-941d-9b58eb498754)
