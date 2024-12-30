# 匯入必要模組
import cv2
import numpy as np

# 加載圖像
image = cv2.imread('gii.jpeg')

# 檢查圖像是否成功加載
if image is None:
    print("Error: Unable to load image. Please check the file path.")
    exit()

# 初始化 ORB 特徵檢測算法
orb_feature = cv2.ORB_create()

# 檢測特徵點
orb_kp = orb_feature.detect(image)

# 繪製特徵點
orb_out = cv2.drawKeypoints(image, orb_kp, None, color=(0, 255, 0), flags=cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)

# 在圖像上添加標籤
font = cv2.FONT_HERSHEY_SIMPLEX
loc = (10, 40)
color = (0, 0, 255)
cv2.putText(orb_out, 'ORB Keypoints', loc, font, 1, color, 2, cv2.LINE_AA)

# 顯示結果
cv2.imshow('ORB Keypoints', orb_out)
cv2.waitKey(0)
cv2.destroyAllWindows()
<img width="1001" alt="截圖 2024-12-30 晚上8 53 09" src="https://github.com/user-attachments/assets/24c16a6e-abae-43aa-8218-11ef3a5ca311" />
