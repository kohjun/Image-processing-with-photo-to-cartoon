# Image-processing-with-photo-to-cartoon

**카툰 스타일로 이미지를 변환시키는 방법**

## 원본 이미지
import cv2
import matplotlib.pyplot as plt

### 이미지를 불러옵니다.
img = cv2.imread('cat2.jpeg')

### 이미지 표시
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.axis('off')
plt.show()

![image](https://github.com/kohjun/Image-processing-with-photo-to-cartoon/assets/82298792/966098c6-308a-4f30-8389-64bc531f3b3c)


## 자신의 알고리즘으로 만화같은 느낌이 잘 표현되는 이미지 만들기
import cv2
import base64
from IPython.display import HTML

### 이미지를 불러옵니다.
image = cv2.imread('cat2.jpeg')

### 에지 검출을 위해 이미지를 그레이스케일로 변환합니다.
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

### 에지 검출을 위해 가우시안 블러를 적용합니다.
blurred_image = cv2.medianBlur(gray_image, 5)

### 에지를 검출합니다.
edges = cv2.adaptiveThreshold(blurred_image, 255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 9, 9)

### 컬러 이미지를 블러처리 합니다.
color = cv2.bilateralFilter(image, 9, 300, 300)

### 에지를 컬러 이미지에 합성합니다.
cartoon = cv2.bitwise_and(color, color, mask=edges)

### 이미지를 HTML로 변환합니다.
_, encoded_image = cv2.imencode('.png', cartoon)
html_image = "<img src='data:image/png;base64," + base64.b64encode(encoded_image).decode() + "'/>"

### HTML을 표시합니다.
HTML(html_image)
![image](https://github.com/kohjun/Image-processing-with-photo-to-cartoon/assets/82298792/5ab2fa30-586b-4ca8-820e-a32e1e97d4a9)


## 자신의 알고리즘으로 만화 같은 느낌이 잘 표현되지 않는 이미지 만들기
import cv2
import matplotlib.pyplot as plt

### 이미지를 불러옵니다.
img = cv2.imread('cat2.jpeg', cv2.IMREAD_GRAYSCALE)

### 초기값 설정
threshold = 127
adaptive_type = cv2.ADAPTIVE_THRESH_MEAN_C
adaptive_blocksize = 99
adaptive_C = 4

### 가우시안 블러를 적용합니다.
blurred_img = cv2.GaussianBlur(img, (5, 5), 0)

### 이진화 기법 적용
_, binary_user = cv2.threshold(blurred_img, threshold, 255, cv2.THRESH_BINARY_INV)

### 이미지 및 이진화 결과 표시
plt.figure(figsize=(8, 6))
plt.imshow(binary_user, cmap='gray')
plt.title('Simple Thresholding with Gaussian Blur')
plt.axis('off')
plt.show()
![image](https://github.com/kohjun/Image-processing-with-photo-to-cartoon/assets/82298792/639c7813-36ef-40f6-b81a-656e504326da)

## 자신의 알고리즘의 한계에 대해 작성
주피터 노트북에서는 GUI 백엔드를 지원하지 않으므로 matplot을 이용해 표현할 수 밖에 없는 한계가 있었고, 만화스타일을 표현하는 과정에서도
내가 생각하는 스타일을 표현하는 과정에서 어떤 기법을 사용해야 나오는지 찾는 과정도 어려웠다. 또한 선명도나 명암 대비 같은 부분이나 음영이나 색조를 표현하는 것도
코드로 표현하기에는 어떤 수치가 어울리는지 찾는 과정에 많은 시간이 걸렸다.
