from ultralytics import YOLO
import cv2

# 加載 YOLO 模型
model = YOLO('yolov8n.pt')

# 多張圖片路徑
image_paths = ['IMG_1214.JPG', 'IMG_1668.JPG', 'IIIB2963.JPG']

# 批量處理圖片
for image_path in image_paths:
    results = model(image_path)
    annotated_image = results[0].plot()
    output_path = f'detected_{image_path}'
    cv2.imwrite(output_path, annotated_image)
    print(f"檢測完成，結果已保存至 {output_path}")

    ![螢幕擷取畫面 2025-01-03 033829](https://github.com/user-attachments/assets/3d0f4596-6307-4641-a723-e69c7bb879b6)
![螢幕擷取畫面 2025-01-03 033840](https://github.com/user-attachments/assets/2a259a27-7d40-43ce-884d-89c7c72f78ff)
![螢幕擷取畫面 2025-01-03 033848](https://github.com/user-attachments/assets/850480cd-aa1a-4fa8-b590-e743c4030d69)
