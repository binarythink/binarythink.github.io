---
permalink: /hello-raspberrypi
title: 안녕 라즈베리 파이 - OS 설치
excerpt: 소형 컴퓨터 라즈베이 파이 구입기동기와 OS 설치를 해봅니다.
header:
  teaser: /asserts/images/content/2018/12/hello-raspberrypi/raspberrypi-02.jpeg
  og_image: /asserts/images/content/2018/12/hello-raspberrypi/raspberrypi-02.jpeg
  overlay_image: /assets/images/content/2017/12/hello-raspberrypi/raspberrypi-02.jpeg
  overlay_filter: 0.5
categories:
  - raspberrypi
tags:
  - 라즈베리파이
  - raspberrypi
toc: true
last_modified_at: 2017-12-30T13:20:00.000Z
---

라즈베리 파이를 구매했습니다.
지난 크리스마스때 만든 `slack-juggler`[^1]를 개발해 사내 `slack`에 적용해 팀원분들이 만족하며 잘 사용해주시고 있고
요구사항도 몇가지 주셔서 시간이 날 때 마다 요구사항을 해결하기 위해 취미로 코딩을 하니 또 코딩이 즐거운 효과가 있습니다.

> 팀에서 slack 과 jira를 사용하고 계시면 [slack-juggler 레포지터리](https://github.com/binarythink/slack-juggler)에 가셔서 한번 사용해보세요 ^^;

[^1]: https://github.com/binarythink/slack-juggler


## 구매 동기

그런데 문제가 있으니 `jira`는 사내에서만 접근 가능하고 이슈번호를 획득해 `jira` 정보를 제공해주는
`slack-juggler`는 외부 서버에 항상 돌고 있으면서 방화벽을 신청하기에는 복잡한 행정적 절차가 있어
작업하는 컴퓨터에 백그라운드 프로세스로 돌려 사용하고 있었습니다.

맥과 익숙해지기 위해 매일 컴퓨터를 들고 출퇴근하다보니 저와 슬랙봇은 같이 출퇴근 하는 존재가 되어 버렸고
새로 입사한 회사는 탄력적 근무제를 시행하나 내 출퇴근은 탄력적일 수 없는 문제가 발생했습니다.

그래서 라즈베리파이를 구입해 `slack-juggler`를 설치하고 사내 와이파이를 물린다는 생각으로 블루투스와
wifi를 지원하는 라즈베리파이3 구입을 결정하였고 어떤 준비물들이 필요한지 검색마저 귀찮아 맘 편하게 스타터킷을 구매했습니다.


## 개봉

![](/assets/images/content/2017/12/hello-raspberrypi/raspberrypi-01.jpeg)

집이라 조명이 부족해 사진이 안나왔다고 하고 싶지만 원래 못 찍습니다.
스타터킷은 판매자마다 차이는 있지만 저의 경우 구성품은

* 라즈베리파이3 모델 B
* 라즈베리파이 정품 케이스
* 2.5A 고속충전기 - 라즈베리파이3 부터는 2.5A의 충전기를 사용해야 한다고 합니다.)
* 방열판 2개
* microSD 카드(32G) – 기본 16G 였으나 옵션으로 추가 구매
* microSD 를 USB-A 로 사용할 수 있게 해주는 젠더

조립은 생각보다 어렵지 않았습니다. 이건 크니까 여기에 붙일까? 구멍이 이쪽으로 나와 있으니 이렇게 넣으면 넣어지나?
케이스가 부러지지는 않을지 조금 걱정은 됐지만 설명서 없이도 척척하니 케이스까지 조립을 쉽게 할 수 있었습니다.

![](/assets/images/content/2017/12/hello-raspberrypi/raspberrypi-02.jpeg)


## OS 설치

### 라즈비안 다운로드
[https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/) 에 가서
라즈베리의 운영체제인 라즈비안을 다운로드 할 수 있습니다.
`RASPBIAN STRETCH WITH DESKTOP` 버전과 `RASPBIAN STRETCH LITE` 버전이 있으며 콘솔 사용이 능숙하신 분들은
`LITE` 버전만을 설치해도 좋으나 저는 GUI환경이 어떻게 구성되어 있는지 보고 싶기도 하고 1GB 메모리로 잘 돌아가는지 궁금해서
`DESKTOP` 버전을 다운로드 했습니다.

### Etcher 로 이미지 굽기
다운받은 이미지는 [https://etcher.io/](https://etcher.io/) 에 접속해 `Etcher` 를 다운로드 합니다.  
이미지를 SD 카드에 설치하는 방법이 여러가지가 있으나 현재 공식홈페이지에 소개하고 있는 방법이기도 하고 가장 쉬운 방법이여서
저도 이 방법을 이용했습니다. GUI 환경으로 컴퓨터에 넣어둔 SD 카드를 찾고 다운로드 받은 이미지 파일을 찾아
몇번 클릭만으로 쉽게 해결되었습니다.

## 누리기
이제 SD카드를 라즈베리파이에 넣고 전원을 넣어주면 몇가지 설정으로 사용가능한 상태가 됩니다.
데스크탑 환경이 생각보다 깔끔하고 빠릿빠릿합니다. 기타 GUI 환경의 프로그램을 돌려본건 아니지만 미니멀과 괘적함의 느낌이 듭니다.

![](/assets/images/content/2017/12/hello-raspberrypi/raspberrypi-03.jpeg)

## 간단한 설정
한글을 자유롭게 입력하기 위해서는 별도의 설정이 필요하나 따로는 안적으려고 합니다. `라즈베리파이 한글입력` 으로만 검색하셔도 많은 자료가 나옵니다.
아래의 명령어를 쳐서 간단히 필요한 설정을 하실 수 있습니다.
```
sudo raspi-config
```
이 부분은 라즈베리파이를 사용하려는 목적에 따라 필요한 설정이 다를 것 이므로 별도 언급은 하지 않겠습니다만 **라즈베리파이3 모델B에서 와이파이의 국가코드를 변경하는 경우 와이파이를 찾을 수 없는 버그가 있으니 wifi 사용국가는 바꾸시지 않길 추천드립니다.**


## 마치며
저는 단순한 JAVA 어플리케이션을 돌리기 위해서 구입했으나, 라즈베리파이에 다양한 활용(GPIO를 이용한 IoT 장비 개발)등 여러가지 활용도가 있어서 좀 더 공부하고 싶기는 합니다. 어쩌면 올해 취미는 파이썬 공부와 라즈베리파이가 될 수도 있겠네요.
