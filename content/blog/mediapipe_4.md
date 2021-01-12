---
title: MediaPipe - (4)
date: 2021-01-12T01:14:36+09:00
draft: false

#post thumb
image: "/images/post/mediapipe/holi_example.gif"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - "DeepLearning"
tags:
  - "Tools"

# post type
type: "post"
---

최근 MediaPipe의 버전이 올라가면서 python API에 추가된 솔루션이 있습니다!

`holistic` 이라는 솔루션입니다!

이는 **pose estimation, face landmarks, hand tracking** 을 동시에 하는 솔루션이에요!

## 실행 코드

```python
import time
import cv2 as cv
import mediapipe as mp
mp_drawing = mp.solutions.drawing_utils
mp_holistic = mp.solutions.holistic

holistic = mp_holistic.Holistic(static_image_mode=True)

start = time.time()
image = cv.imread('/Users/jerry/Git/files/mediapipe/test_01.jpeg')
image_hight, image_width, _ = image.shape
print(image_hight, image_width)

# Convert the BGR image to RGB before processing.
results = holistic.process(cv.cvtColor(image, cv.COLOR_BGR2RGB))

# Draw pose, left and right hands, and face landmarks on the image.
annotated_image = image.copy()
mp_drawing.draw_landmarks(
    annotated_image, results.face_landmarks, mp_holistic.FACE_CONNECTIONS)
mp_drawing.draw_landmarks(
    annotated_image, results.left_hand_landmarks, mp_holistic.HAND_CONNECTIONS)
mp_drawing.draw_landmarks(
    annotated_image, results.right_hand_landmarks, mp_holistic.HAND_CONNECTIONS)
mp_drawing.draw_landmarks(
    annotated_image, results.pose_landmarks, mp_holistic.POSE_CONNECTIONS)
print(time.time()-start)
cv.imshow('MediaPipe Holistic', annotated_image)
cv.imwrite('result.jpeg', annotated_image)
cv.waitKey(0)
cv.destroyAllWindows()
holistic.close()
```

중간에 이미지를 읽고 모든 과정을 수행하는데 까지 얼마나 걸리는지 시간을 재보는 코드도 추가를 했습니다. 

테스트로 사용한 이미지 크기는 2301x1534 인데요. 

제 맥 기준 (i5-8500B) 처리하는데 약 0.17초 걸리네요.. 빠르다..

아무튼 이를 이용해서 빠르게 어플리케이션을 만들고 싶으신 분들껜 매우 유용해 보입니다! (특히나 대학생 프로젝트로!)

| {{< figure src="/images/post/mediapipe/holi_input.jpeg" >}} |  {{< figure src="/images/post/mediapipe/holi_output.jpeg" >}} |
| :-: | :-: |


## P.S
- 흠...간단히 해볼만한 어플리케이션이 뭐가 있을까....