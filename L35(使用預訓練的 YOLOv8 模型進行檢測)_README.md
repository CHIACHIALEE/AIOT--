from ultralytics import YOLO
import cv2

# 加載 YOLOv8 模型
model = YOLO('yolov8n.pt')  # Nano 模型

# 圖片檢測
image_path = 'demo.jpg'  # 替換為您的圖片路徑
results = model(image_path)

# 顯示檢測結果
annotated_image = results[0].plot()
cv2.imshow('YOLOv8 Detection', annotated_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
![螢幕擷取畫面 2024-12-30 205722](https://github.com/user-attachments/assets/8f11e8a7-65af-4546-95bc-39f9738af6eb)
![螢幕擷取畫面 2025-01-04 132136](https://github.com/user-attachments/assets/dcddd7d7-4d4d-4f93-8ac5-5ad9ccdbb3e9)
