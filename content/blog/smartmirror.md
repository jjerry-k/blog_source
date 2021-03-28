---
title: "Raspberry Pi를 이용한 Smart Mirror 만들기"
date: 2021-03-28T01:41:53+09:00
draft: False

#post thumb
image: "https://www.raspberrypi.org/wp-content/uploads/2011/10/Raspi-PGB001.png"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - "Living"
tags:
  - "Hardware"

# post type
type: "post"
---

이번 생일에 Raspberry Pi Zero WH 를 선물로 받았습니다. (제가 선물로 달라고 했음.)  
뭐 이래저래 써보고 있는데 이걸로 무엇을 만들어볼까 고민을 해봤습니다.  
Face Detector..? FPS가 너무 안나옴....  
음....Smart Mirror나 만들어보자 싶었죠!  
책상에 놔두면 날씨, 시간, 달력같은거 확인하기 좋으니까요!  
그럼 과정을 차례로 적어보겠습니다!  

## 준비물
- Raspberry Pi Zero WH
- SD 카드, 어댑터
- 7 인치 1024x600 LCD [구매 링크](https://ko.aliexpress.com/item/1005001709776554.html?spm=a2g0s.9042311.0.0.71ef4c4dKcphsI)
- micro 5핀 OTG 케이블
- mini Hdmi 컨버터

{{< figure src="/images/post/smartmirror/01.jpeg" >}}

## 제작 과정

### OS 설치

[http://emmanuelcontreras.com/how-to/how-to-create-a-magic-mirror-2-with-pi-zero-w/](http://emmanuelcontreras.com/how-to/how-to-create-a-magic-mirror-2-with-pi-zero-w/)

위 링크로 가셔서 스크롤을 내리시다보면 다음 사진의 파란색 상자 부분이 보입니다!  
클릭해서 이미지 다운로드!!

{{< figure src="/images/post/smartmirror/Untitled.png" >}}

https://www.balena.io/etcher/  
저는 Flashing Tool 로 balena Etcher를 사용했습니다.  
PC에 SD카드를 연결 후 Tool에서 다운로드한 이미지와 SD 카드를 선택하고 Flash 버튼 클릭!
{{< figure src="/images/post/smartmirror/Untitled 1.png" >}}
{{< figure src="/images/post/smartmirror/Untitled 2.png" >}}

설치가 완료되면 SD 카드로 가서 `SSH` 라는 빈 파일을 생성해주세요.  
(`touch SSH` 이런 식으로...)  
그리고 wifi를 미리 잡아주려합니다.
`wpa_supplicant.conf` 라는 파일을 생성하고 다음과 같이 적어줍니다!  
``` bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
  ssid="네트워크 이름"   # ex) ssid="test_2g"
  psk="네트워크 비밀번호" # ex) psk="test"
}
```
당연히 아시겠지만 `"네크워크 이름"`, `"네트워크 비밀번호"`는 사용하실 wifi SSID와 비밀번호 적는겁니다..!  
그 후 다음과 같이 두 파일이 생성되어 있는 것을 확인해주세요!

{{< figure src="/images/post/smartmirror/Untitled 4.png" >}}

### 초기 세팅
PC에서 SD 카드를 제거하고 Raspberry Pi, SD카드, LCD를 연결해줍니다.  
{{< figure src="/images/post/smartmirror/02.jpeg" >}}
{{< figure src="/images/post/smartmirror/03.jpeg" >}}

연결 후 전원을 인가하고 한...4~5분? 지나면 디스플레이에 화면이 뜹니다!
{{< figure src="/images/post/smartmirror/04.jpeg" >}}
{{< figure src="/images/post/smartmirror/05.jpeg" >}}

이제는 Raspberry Pi에 SSH로 접속해서 직접 세팅을 할겁니다!
``` bash
ssh pi@{ip} # 초기 비밀번호는 raspberry
# ex) ssh pi@192.168.0.137
```
접속해서는 passwd 를 이용해서 root와 pi 계정의 비밀번호를 변경...하셔도 되고 안하셔도 됩니다! ㅎㅎ  
그 후에 `pm2 stop all` 이라는 명령어를 실행해주세요!  
설치한 이미지가 기본적으로 MagicMirror 가 실행되도록 해놓은 것이기 때문에 실행을 잠시 중단하는 명령어입니다!
{{< figure src="/images/post/smartmirror/Untitled 5.png" >}}

그 후엔 이것저것 추가적인 Raspberry Pi 세팅을 해줄건데요!  
`sudo raspi-config` 라고 입력을 해줍니다!  
(사진이 누락되었네요...)  
그러면 다음과 같은 화면이 나오는데요!  
여기선 Filesystem 확장, Timezone 변경 두가지를 할겁니다!

**Advanced Options -> Expand Filesystem**
{{< figure src="/images/post/smartmirror/Untitled 6.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 7.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 8.png" >}}

**Localisation Options -> Change Timezone**  

{{< figure src="/images/post/smartmirror/Untitled 13.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 9.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 10.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 14.png" >}}

{{< figure src="/images/post/smartmirror/Untitled 15.png" >}}

두 사항을 변경하셨으면 reboot를 해주세요!
{{< figure src="/images/post/smartmirror/Untitled 11.png" >}}

### MagicMirror2 Update

MagicMirror도 꽤나 많은 사항이 Update 되었기 때문에....올려줘야겠죠!

``` bash
cd ~/MagicMirror
rm -rf package*.json
git pull
npm install
```

Raspberry Pi Zero 라서 다음과 같은 에러메세지가 뜨게 됩니다!  
하지만 무시하세요... reboot 하면 정상 동작합니다...
{{< figure src="/images/post/smartmirror/Untitled 12.png" >}}

그리고 마지막으로 리부팅을 하면! 끝입니다!


이번 포스팅은 Raspberry Pi Zero와 MagicMirror 를 이용한 Smart Mirror 설치 및 초기 세팅에 대해 포스팅을 해봤습니다!  
다음엔 입맛대로 바꾸는 방법에 대해 포스팅을 해보겠습니다!  

### P.S
- 요즘 번아웃 + 현타의 향연입니다..
- 살려주세요오오오오오!!!