---
title: "About TensorFlow Callback "
date: 2021-02-22T19:32:50+09:00
draft: false

#post thumb
image: "images/gallery/tf_logo.png"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - "DeepLearning"
tags:
  - "TensorFlow"

# post type
type: "post"
---

> A callback is a powerful tool to customize the behavior of a Keras model during training, evaluation, or inference.

Callback은 TensorFlow로 Model을 학습시키다보면 쓸 수 밖에 없는...도구 입니다..!

몇몇 편하게 쓸 수 있게 제공하는 Callback 함수들도 있습니다.  [참고 링크](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks)

이번 포스팅은 제공하는 Callback 함수가 아닌 **내 입맛대로 Callback 함수**을 만들어보려합니다!

기본적인 틀을 보여드리면 다음과 같습니다!

``` python
class CustomCallback(keras.callbacks.Callback):
    def on_train_begin(self, logs=None):
        
    def on_train_end(self, logs=None):
        
    def on_epoch_begin(self, epoch, logs=None):
        
    def on_epoch_end(self, epoch, logs=None):
        
    def on_test_begin(self, logs=None):
        
    def on_test_end(self, logs=None):
        
    def on_predict_begin(self, logs=None):
        
    def on_predict_end(self, logs=None):
        
    def on_train_batch_begin(self, batch, logs=None):
        
    def on_train_batch_end(self, batch, logs=None):
        
    def on_test_batch_begin(self, batch, logs=None):
        
    def on_test_batch_end(self, batch, logs=None):
        
    def on_predict_batch_begin(self, batch, logs=None):
        
    def on_predict_batch_end(self, batch, logs=None):
```

callback은 `구간` 단위로 함수를 정의합니다!  
여기서 구간이란 전체적인 학습의 시작과 종료, 매 epoch의 시작과 종료, 매 batch의 시작과 종료 등과 같습니다.  
조금 더 자세히 알아보겠습니다.  
|구간|설명|
|:---:|:---:|
|`on_train_begin`, `on_train_end`|`학습 프로세스` 시작, 종료시 수행할 기능|
|`on_epoch_begin`, `on_epoch_end`|`Epoch` 시작, 종료시 수행할 기능|
|`on_test_begin`, `on_test_end`|`Evaluation` 시작, 종료시 수행할 기능|
|`on_predict_begin`, `on_predict_end`|`Prediction` 시작, 종료시 수행할 기능|
|`on_train_batch_begin`, `on_train_batch_end`|`배치 단위의 Train` 시작, 종료시 수행할 기능|
|`on_test_batch_begin`, `on_test_batch_end`|`배치 단위의 Evaluation` 시작, 종료시 수행할 기능|
|`on_predict_batch_begin`, `on_predict_batch_end`|`배치 단위의 Prediction` 시작, 종료시 수행할 기능|

그럼 대충 에제 코드를 돌려보겠습니다. 

``` python
import numpy as np
import tensorflow as tf
from tensorflow.keras import models, layers, callbacks
tf.random.set_seed(777)

x = np.random.normal(0.0, 0.55, (10000, 1))
y = x * 0.1 + 0.3 + np.random.normal(0.0, 0.03, (10000,1))

# plt.plot(x, y, 'r.')
# plt.show()

model = models.Sequential()
model.add(layers.Dense(1, input_shape=(1, )))

model.compile(optimizer='sgd', loss='mse')

class CustomCallback(keras.callbacks.Callback):
    def on_train_begin(self, logs=None):
        print("Train Start! ")

    def on_train_end(self, logs=None):
        print("Train End! ")

    def on_epoch_begin(self, epoch, logs=None):
        print(f"{epoch} Start!")

    def on_epoch_end(self, epoch, logs=None):
        print(f"{epoch} End!")

    def on_train_batch_begin(self, batch, logs=None):
        print(f"{batch} Start!")

    def on_train_batch_end(self, batch, logs=None):
        print(f"{batch} End!")

history = model.fit(x, y, epochs=1, batch_size=1024, callbacks = [CustomCallback()], verbose = 0)
```
위의 코드를 돌리면 다음과 같은 출력이 나옵니다.  
코드는 매우 단순 하니 읽어보시면 바로 해석 될 듯...

``` bash
Train Start! 
Epoch 0 Start!
Batch 0 Start!
Batch 0 End!
Batch 1 Start!
Batch 1 End!
Batch 2 Start!
Batch 2 End!
Batch 3 Start!
Batch 3 End!
Batch 4 Start!
Batch 4 End!
Batch 5 Start!
Batch 5 End!
Batch 6 Start!
Batch 6 End!
Batch 7 Start!
Batch 7 End!
Batch 8 Start!
Batch 8 End!
Batch 9 Start!
Batch 9 End!
Epoch 0 End!
Train End! 
```

그럼 이번엔 실용적인(?) callback을 만들어보겠습니다. 

매 epoch 마다 Loss graph를 그리는 기능을 넣어보겠습니다!

```python
import numpy as np
from matplotlib import pyplot as plt
plt.ion() # ipython 환경이 아닐 시 interactive plot을 할 수 있도록 변경

import tensorflow as tf
from tensorflow.keras import models, layers, callbacks
tf.random.set_seed(777)

x = np.random.normal(0.0, 0.55, (10000, 1))
y = x * 0.1 + 0.3 + np.random.normal(0.0, 0.03, (10000,1))

model = models.Sequential()
model.add(layers.Dense(1, input_shape=(1, )))

model.compile(optimizer='sgd', loss='mse')

class CustomCallback(callbacks.Callback):
    def on_train_begin(self, logs=None):
        plt.figure(figsize=(10, 10))
        plt.xlim([-0.5, self.params['epochs']+0.5])
        print("Train Start! ")

    def on_train_end(self, logs=None):
        print("Train End! ")
        plt.show(block=True)

    def on_epoch_end(self, epoch, logs=None):

        plt.plot(epoch, logs["loss"], 'bo')
        plt.show()
        plt.pause(0.5)

history = model.fit(x, y, epochs=10, batch_size=1024, callbacks = [CustomCallback()])


```

{{< figure src="/images/post/tf_callback/example.gif" >}}


Jupyter, IPython 없이도 Interactive하게 loss graph를 잘 그릴 수 ... 있네요. (솔직히 이번에 처음 해봤음..)

이런 식으로 본인이 원하는 기능을 학습중에 실행 할 수 있도록 만들 수 있습니다!

많은 분들이 본인만의 커스텀 콜백을 편하게 자유롭게 구현해서 쓰시길 바랍니다!

### P.S
- 수면 시간 감소...
- 예민주의보