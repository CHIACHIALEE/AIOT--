from ultralytics import YOLO
import cv2

# 加載 YOLO 模型
model = YOLO('yolov8n.pt')  # 使用 Nano 模型（輕量快速）

# 圖片路徑
image_path = '8b.jpg'

# 推理
results = model(image_path)

# 可視化檢測結果
annotated_image = results[0].plot()

# 保存檢測結果
cv2.imwrite('detected_8b.jpg', annotated_image)
print("檢測完成，結果已保存至 'detected_8b.jpg'")
![螢幕擷取畫面 2025-01-03 020247](https://github.com/user-attachments/assets/8c54dc82-b568-4899-a636-5446fbd5de18)

