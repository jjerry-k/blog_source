---
title: "MediaPipe - (5)"
date: 2021-06-20T23:06:14+09:00
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

요즘 MediaPipe에 대해 tracking을 안하다보니 어떤 솔루션이 새로 나왔는지 잘 몰랐습니다... 오랜만에 들어갔더니 세 가지 솔루션이 추가가 되었더라구요!

차근차근 예제를 돌려보겠습니다!

이번엔 `Selfie Segmentation` 이라는 솔루션입니다!

단순히 말하면 Human segmentation 이에요!

## 실행 코드

```python
import cv2
import numpy as np
import mediapipe as mp
mp_drawing = mp.solutions.drawing_utils
mp_selfie_segmentation = mp.solutions.selfie_segmentation

IMAGE_FILE = './selfie_input.jpg'
BG_COLOR = (0, 0, 0)
MASK_COLOR = (1, 1, 1)

with mp_selfie_segmentation.SelfieSegmentation(model_selection=0) as selfie_segmentation:
    
    image = cv2.imread(IMAGE_FILE)
    image_height, image_width, _ = image.shape
    
    results = selfie_segmentation.process(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))

    condition = np.stack((results.segmentation_mask,) * 3, axis=-1) > 0.1
    
    fg_image = np.zeros(image.shape, dtype=np.uint8)
    fg_image[:] = MASK_COLOR
    bg_image = np.zeros(image.shape, dtype=np.uint8)
    bg_image[:] = BG_COLOR
    output_image = np.where(condition, fg_image, bg_image)
    cv2.imwrite('./selfie_output.jpg', output_image*image)
  ```

| {{< figure src="/images/post/mediapipe/selfie_input.jpg" >}} |  {{< figure src="/images/post/mediapipe/selfie_output.jpg" >}} |
| :-: | :-: |

음....  
예시 사진이 너무 단순한 사진이다 보니...  
굉장히 변별력이 없는 듯하군요...  
웹캠이 준비되는 대로 다시 올려봐야겠습니다.

## P.S
- 추후에 배경 캠 + 배경변경 코드를 올려보겠습니다!