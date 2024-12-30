import cv2
import numpy as np

# 加載圖像並調整尺寸
src = cv2.imread('demo.jpg', -1)
if src is None:
    print("圖像加載失敗，請檢查檔案路徑。")
    exit()
src = cv2.resize(src, (403, 302))

# 將圖像轉換為灰階並應用高斯模糊
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (9, 9), 2)  # 增加模糊核大小以平滑雜訊

# 使用霍夫圓變換檢測圓形
circles = cv2.HoughCircles(
    gray,
    cv2.HOUGH_GRADIENT,
    dp=1.2,               # 略放大累加器影像解析度
    minDist=40,           # 圓心間的最小距離
    param1=100,           # Canny邊緣檢測的高閾值
    param2=40,            # 累加器閾值，調整檢測的靈敏度
    minRadius=10,         # 最小圓半徑
    maxRadius=50          # 最大圓半徑
)

# 繪製檢測結果
if circles is not None:
    circles = circles[0].astype(int)  # 提取第一個維度並轉為整數
    out = src.copy()
    for x, y, r in circles:
        # 繪製圓形
        cv2.circle(out, (x, y), r, (0, 0, 255), 2)
        # 繪製圓心
        cv2.circle(out, (x, y), 2, (0, 255, 0), 3)
    # 合併原圖與結果
    src = cv2.hconcat([src, out])
else:
    print("未檢測到任何圓形。")

# 顯示結果
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
cv2.imshow('image', src)
cv2.waitKey(0)
cv2.destroyAllWindows()
![螢幕擷取畫面 2024-12-30 204243](https://github.com/user-attachments/assets/90447aa9-2354-4783-b90e-9328f438568a)
