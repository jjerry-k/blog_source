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



# 2021.06.21 추가 내용
허헛 동영상으로...해보았습니다...  
여기엔 두개의 모드가 있습니다. 하나는 Base mode, 다른 하나는 Landscape mode.  
약----간 차이가 있어요 ㅋㅋㅋ

## 실행 코드 
``` python
import cv2 as cv
import numpy as np
import mediapipe as mp
mp_drawing = mp.solutions.drawing_utils
mp_selfie_segmentation = mp.solutions.selfie_segmentation

def change_background(frame, background, mode=0, threshold=0.1):
    """
    frame: [H, W, C]
    background: [H, W, C]

    모두 BGR 컬러
    """
    BG_COLOR = (0, 0, 0) # gray
    MASK_COLOR = (1, 1, 1) # white
    with mp_selfie_segmentation.SelfieSegmentation(model_selection=mode) as selfie_segmentation:
        # model_selection
        # 0: basic model, 1: landscape model
        results = selfie_segmentation.process(cv.cvtColor(frame, cv.COLOR_BGR2RGB))
        condition = np.stack((results.segmentation_mask,) * 3, axis=-1) > threshold
        fg_image = np.zeros(frame.shape, dtype=np.uint8)
        fg_image[:] = MASK_COLOR
        bg_image = np.zeros(frame.shape, dtype=np.uint8)
        bg_image[:] = BG_COLOR
        mask = np.where(condition, fg_image, bg_image)
        output_image = frame*mask + background*(1-mask)
    return output_image

video_path = './selfie_input.mp4'
bg_path = './background.jpg'

cap = cv.VideoCapture(video_path)
bg_img = cv.imread(bg_path)

H = int(cap.get(cv.CAP_PROP_FRAME_HEIGHT))
W = int(cap.get(cv.CAP_PROP_FRAME_WIDTH))
bg_img = cv.resize(bg_img, (W, H))


fourcc = cv.VideoWriter_fourcc(*'DIVX')
out = cv.VideoWriter(f'selfie_output.mp4', fourcc, 30.0, (int(W), int(H)))    

frame_idx = 0
while(cap.isOpened()):
    
    ret, frame = cap.read()
    if ret == False:
        break
    frame = change_background(frame, bg_img, mode=idx, threshold=0.4)
    out.write(frame)
```

|설명|영상|
| :-: | :-: |
| Input | {{< rawhtml >}} 
<video width=50% autoplay loop>
    <source src="/images/post/mediapipe/selfie_input.mp4" type="video/mp4">
</video> 
{{< /rawhtml >}} |
| Base model | {{< rawhtml >}} 
<video width=50% autoplay loop>
    <source src="/images/post/mediapipe/selfie_output_base.mp4" type="video/mp4">
</video> 
{{< /rawhtml >}}|
| Landscape model| {{< rawhtml >}} 
<video width=50% autoplay loop>
    <source src="/images/post/mediapipe/selfie_output_landscape.mp4" type="video/mp4">
</video>
{{< /rawhtml >}}|

## P.S
- ~~추후에 배경 캠 + 배경변경 코드를 올려보겠습니다!~~
- 점점 뭔가 쉽게 만들 수 있을 듯...