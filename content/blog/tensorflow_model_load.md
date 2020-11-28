---
title: "TensorFlow 모델 로드 에러.."
date: 2020-11-28T11:45:10+09:00
draft: false

#post thumb
image: "/images/post/tensorflow_model_load/Untitled.png"

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

TensorFlow를 쓰다보면 종종 model load 할때 error 가 발생할때가 있습니다.

에러에 따라  대처법을 적어보려고 합니다. 

**해당 방법은 모델을 json 으로 저장했을때 유효한 방법입니다!**

## Value error:unknown layer : functional

json 안에 내용이

``` json
{"class_name": "Functional", 
  "config": {
    "name": "functional_1", 
    "layers": [{"class_name": "InputLayer" ...
```

이렇게 되어있다면

```json
{"class_name": "Model", 
  "config": {
    "name": "model_1", 
    "layers": [{"name": "input_1", "class_name": "InputLayer" ...
```

이렇게 `"Functional" 을 "Model"`로 `"functional_1"을 "model_1"`으로 변경 후 싫행하시면 됩니다. 

## TypeError: ('Keyword argument not understood:', 'groups')

json 에서 'groups' 라는 내용을 찾아봅니다. 

다음과 같이 있네요.

{{< figure src="/images/post/tensorflow_model_load/Untitled.png" >}}

단순히 json 안에 모든 `"groups": ,` 를 지워주세요. 

그 후 실행하시면 됩니다!

## P.S
- 간단 포스팅 끝.
- TF 2.4 에선 해결되려나...