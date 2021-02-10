---
title: "M1 Macbook Pro 열흘 사용기"
date: 2021-02-10T20:43:43+09:00
draft: true

#post thumb
image: "https://www.apple.com/v/mac/m1/a/images/overview/m1_hero__gayysked51ym_large.jpg"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - Living
tags:
  - Review

# post type
type: "post"
---

약 한달? 전 회사에 여분의 맥북 프로 신형이 생겨서 CTO님께 넌지시 말씀을 드렸습니다. 
> Jerry: 저 맥북 제가 써도 될까요..?  
> CTO: 그럼요! 쓰세요!  

.....다음 날

> CTO: Jerry님, 저거 말고 M1 Macbook Pro는 어때요?  
> Jerry: M1 Macbook Pro요? 저는 상관없습니다! (오히려 좋아, 오히려 개이득)  
> CTO: 원하는 사양 알려주세요.  
> Jerry: 메모리 16GB, 저장장치 512GB, 영문 자판 부탁드립니다.  
> CTO: 2주 후 도착!

M1 Macbook Pro를 수령하고 열심히 세팅을 했습니다.  
하지만 기대하던.....TensorFlow는 세팅이 잘 안되더라구요..  
어찌 어찌 결국 설치를 했습니다.   
설치 방법은 다음에 포스팅을...  

굉~~장히 기대를 하며 mnist classifiction을 돌렸습니다.  
10 epochs을 기준으로 cpu는 66초...  그렇다면 gpu는...?
**80초** (...?)


{{< figure src="/images/post/silicon_mac/hmmteresting.gif" >}}

모르겠다....

어쨌든! 열흘 간 사용을 해봤을 때 장단점을 간단히 적어보자면...

#### 장점
- 깨우기 속도 겁나 빠름.
- Native app 실행 속도 겁나 빠름.
- 배터리 겁나 오래감. (면접시 1시간 정도 써도 5% 달았나..?)

#### 단점
- M1 과 Big Sur의 호환성 이슈
- M1 과 프로그램의 호환성 이슈
- 환경 셋업이 꽤나 번거로움

그냥 가벼운 용도로는 매우 훌륭하지만 개발 용도로는 아직..시기상조로...생각됩니다.

### P.S
- 우리회사 맥북 사주는 회사
- 하지만......(더 이상 말하지 않겠다)