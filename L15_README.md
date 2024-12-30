# 匯入 Picamera2 和時間模組
from picamera2 import Picamera2
import time

# 創建 Picamera2 的實例
picam2 = Picamera2()

# 配置攝影機並啟動
picam2.start()

# 延遲 3 秒以確保攝影機穩定
time.sleep(3)

# 捕捉圖片並保存為 'image.jpeg'
picam2.capture_file("image.jpeg")

# 停止攝影機
picam2.stop()
[image](https://github.com/user-attachments/assets/a0e019b6-c950-4931-878c-035e066783b6)
