from ultralytics import YOLO
import cv2

# 加載 YOLO 模型
model = YOLO('yolov8n.pt')

# 開啟攝影機
cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break
    results = model(frame)
    annotated_frame = results[0].plot()
    cv2.imshow("YOLOv8 Real-Time Detection", annotated_frame)

    # 按下 'q' 鍵退出
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
![螢幕擷取畫面 2025-01-03 034645](https://github.com/user-attachments/assets/afc2b684-be1d-45e3-b2c4-e8d9b84c2705)

