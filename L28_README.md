<img width="1515" alt="image" src="https://github.com/user-attachments/assets/c729ff81-c5f6-45b9-bccd-f51fa07389fc" />import cv2

# 加载图片
img1 = cv2.imread('IMG_5433.jpg')
img2 = cv2.imread('IMG_5434.jpg')

# 检查图片是否加载成功
if img1 is None or img2 is None:
    print("Error: Could not load one or both images.")
    exit()

# 初始化特征检测器 (使用 SIFT 替代 SURF)
feature = cv2.SIFT_create()

# 检测并计算特征点与描述子
kp1, des1 = feature.detectAndCompute(img1, None)
kp2, des2 = feature.detectAndCompute(img2, None)

# 使用 BFMatcher 进行特征匹配
bf = cv2.BFMatcher()
matches = bf.knnMatch(des1, des2, k=2)

# 筛选优良匹配点
good = []
for m, n in matches:
    if m.distance < 0.55 * n.distance:
        good.append(m)

print('Matching points :{}'.format(len(good)))

# 绘制匹配结果
img3 = cv2.drawMatches(img1, kp1, img2, kp2, good, None,
                       flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS)

# 调整输出图片大小
height, width, channel = img3.shape
ratio = float(height) / float(width)
img3 = cv2.resize(img3, (1024, int(1024 * ratio)))

# 显示结果
cv2.imshow('video', img3)
cv2.waitKey(0)
cv2.destroyAllWindows()
<img width="1515" alt="截圖 2024-12-30 晚上8 44 05" src="https://github.com/user-attachments/assets/19575974-3e77-4465-960e-f303d807e3dc" />
