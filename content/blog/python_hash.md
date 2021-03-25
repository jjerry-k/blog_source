---
title: "Python hashlib"
date: 2021-02-10T00:46:48+09:00
draft: false

#post thumb
image: "https://www.python.org/static/img/python-logo.png"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - Python
tags:
  - Hashlib

# post type
type: "post"
---

Python 표준 라이브러리 중 하나인 [hashlib](https://docs.python.org/3/library/hashlib.html) 사용법을 **간단히** 알아보려 합니다. 

이름에서 알 수 있듯이 Hash 관련된 라이브러리입니다. 

그럼 바로 예제 코드를 훑어보겠습니다. 

우선 어떤 hash 알고리즘을 지원하는지 확인해보겠습니다. 

``` python 3
import hashlib
print(hashlib.algorithms_available)
# {'sha384', 'sha224', 'sha1', 'sha3_384', 'sha3_512', 'shake_128', 'sha256', 'blake2b', 
#  'md5', 'blake2s', 'sha3_224', 'sha3_256', 'sha512', 'shake_256'}
```

꽤나 다양합니다...

그럼 이를 어떻게 사용하는지 확인해보겠습니다. 

``` python 3
import hashlib

md5 = hashlib.md5()
md5.update(b"Hello, Python")
print(md5.hexdigest())
# b14116c1129c4ea326be06eefb11add6
```

텍스트 데이터를 넣을 땐 byte로 변환해서 넣어주셔야합니다! 

그래서 string 앞에 `b`라는 prefix가 붙은거에요!

그럼 이번엔 다음 이미지를 처리해보겠습니다. 

{{< figure src="/images/post/python_hash/test.jpeg" >}}

``` python 3
import hashlib

with open('./imgs/test.jpeg', 'rb') as f:
    md5 = hashlib.md5()
    md5.update(f.read())
print(md5.hexdigest())
# 54d6703c01705d184bdb3ba1e3e14085
```

정말 간단하게 hash 값을 얻을 수 있습니다. 

저는 이를 이용해서 주로 **중복 데이터 처리, 파일 비식별화** 등에 적용을 합니다! 

간단한 포스팅이지만 도움이 되었으면 좋겠네요! (어짜피 제 일기같은 포스팅입니다...)

### P.S
- 새해 복 많이 받으세요!
- 연휴동안....논문 좀 읽어야겠다...