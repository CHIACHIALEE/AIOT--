# 匯入模組
from picamera2 import Picamera2, Preview
import cv2
import numpy as np
import os

# 初始化變數
model = cv2.face.LBPHFaceRecognizer_create()
data_file = 'faces.data'
face_cascade_file = 'haarcascade_frontalface_default.xml'
names = ['ckk']  # 映射標籤至名稱

# 確認 Haar 紋波檢測器檔案是否存在
if not os.path.exists(face_cascade_file):
    raise FileNotFoundError(f"Face cascade file '{face_cascade_file}' not found. Ensure it is in the same directory.")

# 加載 Haar 紋波檢測器
face_cascade = cv2.CascadeClassifier(face_cascade_file)

# 確認訓練數據檔案是否存在
if os.path.exists(data_file):
    try:
        model.read(data_file)
        print('Loaded training data successfully.')
    except Exception as e:
        print(f"Error loading training data: {e}. The model may not be valid.")
else:
    print(f"Training data file '{data_file}' not found. Model cannot predict without training.")

# 初始化 PiCamera2
picam2 = Picamera2()
camera_config = picam2.create_preview_configuration(main={"size": (640, 480)})
picam2.configure(camera_config)
picam2.start()

try:
    while True:
        # 從 PiCamera2 捕捉影像
        frame = picam2.capture_array()

        # 翻轉並調整大小
        frame = cv2.flip(frame, 1)
        frame = cv2.resize(frame, (600, 336))

        # 轉換為灰階
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

        # 偵測臉部
        faces = face_cascade.detectMultiScale(gray, 1.1, 3)
        for (x, y, w, h) in faces:
            frame = cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 3)
            face_img = gray[y:y+h, x:x+w]
            face_img = cv2.resize(face_img, (400, 400))

            # 嘗試執行臉部預測
            try:
                val = model.predict(face_img)
                print('label:{}, conf:{:.1f}'.format(val[0], val[1]))
                if val[1] < 50:
                    cv2.putText(
                        frame, names[val[0]], (x, y - 10), 
                        cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 0), 3
                    )
            except cv2.error as e:
                print(f"Prediction error: {e}. Ensure the model is trained and valid.")

        # 顯示影像
        cv2.imshow('video', frame)
        if cv2.waitKey(1) == 27:  # 按 ESC 退出
            break
except Exception as e:
    print(f"An error occurred: {e}")
finally:
    picam2.stop()
    cv2.destroyAllWindows()

![20241230_21h11m29s_grim](https://github.com/user-attachments/assets/d7b2bce5-63d7-47ad-9051-f6d0aa54c5fe)
