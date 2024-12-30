import cv2

ESC = 27
cap = cv2.VideoCapture(0)
ratio = cap.get(cv2.CAP_PROP_FRAME_WIDTH) / cap.get(cv2.CAP_PROP_FRAME_HEIGHT)
WIDTH = 400
HEIGHT = int(WIDTH / ratio)

fourcc = cv2.VideoWriter_fourcc(*'MP4V')
out = cv2.VideoWriter('video.mp4', fourcc, 30, (WIDTH, HEIGHT))

while True:
    ret, frame = cap.read()
    frame = cv2.resize(frame, (WIDTH, HEIGHT))
    frame = cv2.flip(frame, 1)

    out.write(frame)

    cv2.imshow('frame', frame)
    if cv2.waitKey(1) == ESC:
        break

cap.release()
out.release()
cv2.destroyAllWindows()
![螢幕擷取畫面 2024-12-30 201756](https://github.com/user-attachments/assets/600a6dd3-3800-473b-9466-f67311a81022)
