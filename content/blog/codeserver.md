---
title: " IPad 에서 VSCode 사용하기"
date: 2021-04-15T23:47:36+09:00
draft: false

#post thumb
image: "images/post/codeserver/00.png"

# meta description
description: "Usage code-server"
math: true

# taxonomies
categories:
  - "DeepLearning"
tags:
  - "Tools"

# post type
type: "post"
---

근래에 12인치 맥북과 아이패드 프로 9.7인치를 중고로 팔았습니다.  
그리고 아이패드 프로 11인치 1세대를 구매했죠...(포기할 수 없었다..)  
그렇기에... 아이패드에서 원격으로 코딩할 방법을 찾아봤죠...  

1. SSH 접속 후 Vim 이용..
2. VSCode 서버 올리기

1번을 그다지...원치 않는 방법이라 **2번**쪽으로 찾아보았습니다.  
여기서 사용한 오픈소스는 [code-server](https://github.com/cdr/code-server) 입니다.  
설명은..링크타고 가시면 될 것 같고...  
바로 세팅법 들어가겠습니다!
이미 code-server 측에서 docker hub에 docker image를 올려놓으셨습니다..!  
그걸 이용하면 후딱 가능!  
우선 아래 Dockerfile을 만듭니다!  

``` Dockerfile
FROM codercom/code-server:latest
USER coder
ENV SHELL=/bin/bash
RUN sudo apt update && sudo apt -y upgrade
RUN sudo apt install -y unzip wget vim
ENV PORT=8080
```

그 후에 `docker build`를 이용하여 image를 만들어주세요!  
``` bash
cd {Dockerfile이 있는 경로}
docker built --tag code-server:latest .
```

다 만드어진 후에는 code-server 설정을 건드려야합니다!  


``` bash
mkdir -p ~/.config/code-server
vim ~/.config/code-server/config.yaml
```

config.yaml에 아래 내용을 적어주세요!

``` yaml
bind-addr: 127.0.0.1:8080
auth: password
password: {비밀번호} # 어지간하면 숫자로 하세요. 다른건 Test 안해봄...
cert: false
```

이러면 세팅은 끝입니다!

실행은 다음 명령어를 사용하시면 됩니다 !

``` bash
# docker 권한이 없으면 sudo 붙이세요.
docker run -it -d --rm --gpus all \
  --name code-server -p 127.0.0.1:8080:8080 \
  -v "$HOME/.config:/home/coder/.config" \
  -v "$PWD:/home/coder/project" \
  -u "$(id -u):$(id -g)" \
  -e "DOCKER_USER=$USER" \
  code-server:latest
```

그러면 이렇게 아이패드에서 코딩을 할 수 있습니다..!  
(당연히 포트 터널링 해주셔야해요...)
{{< figure src="/images/post/codeserver/01.jpeg" >}}

포스팅은 여기까지 입니다..!  
앞으로도 불친절한 포스팅은 계속됩니다.....

## P.S
- 솔직히 뭐할지 모르겠어요.