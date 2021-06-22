---
title: "MediaPipe - (6)"
date: 2021-06-21T00:06:14+09:00
draft: false

#post thumb
image: "https://google.github.io/mediapipe/images/mediapipe_small.png"

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

오늘은 `Selfie Segmentation`에 이은 `Face Detection` 이라는 솔루션입니다!

이름만 봐도 뭐하는지 알 수 있습니다. 

바로 코드와 실행 결과 올려드립니다!

## 실행 코드

```python
import cv2 as cv
import mediapipe as mp
mp_face_detection = mp.solutions.face_detection
mp_drawing = mp.solutions.drawing_utils

IMAGE_FILES = './selfie_input.jpg'
with mp_face_detection.FaceDetection(min_detection_confidence=0.5) as face_detection:
    image = cv.imread(IMAGE_FILES)
    results = face_detection.process(cv.cvtColor(image, cv.COLOR_BGR2RGB))

    if results.detections:
        annotated_image = image.copy()
        for detection in results.detections:
            mp_drawing.draw_detection(annotated_image, detection)
    cv.imwrite('./selfie_output.jpg', annotated_image)
  ```

| {{< figure src="/images/post/mediapipe/selfie_input.jpg" >}} |  {{< figure src="/images/post/mediapipe/face_output.jpg" >}} |
| :-: | :-: |

뭔----가 잘 하는 것 같은데...아닌 것 같기도 하네요...

다수의 사람까지 된다고 하니...내일은 동영상으로 한번 해봐야겠습니다!


## P.S
- 매우 대충 쓰는 중