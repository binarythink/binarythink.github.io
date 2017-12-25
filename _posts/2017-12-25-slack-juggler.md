---
permalink: /slack-juggler
title: "이제 Slack 에 비서(봇)를 고용하세요"
excerpt: "새로 입사한 회사에서는 Slack이 매우 활성화 되어 있고, 성탄절 별다른 약속도 없는 나는 봇을 개발하기로 했다... slack-bot 개발기와 slack-juggler 소개"
header:
  teaser: /assets/images/content/2017/12/slack-juggler/slack.png
  og_image: /assets/images/content/2017/12/slack-juggler/slack.png
  overlay_image: /assets/images/content/2017/12/slack-juggler/slack.png
  overlay_filter: 0.5
  cta_url: "https://github.com/binarythink/slack-juggler"
  cta_label: "Go Repository"
categories:
  - java
tags:
  - java
  - spring boot
  - slack
  - slack bot
  - jira
  - github
  - slack-juggler
toc : true
last_modified_at: 2017-12-05T16:12:05
---
새로 입사한 회사에서는 Slack[^slack] 이 매우 활성화 되어 있다.
슬랙의 각 채널에서 업무를 진행하고 필요한 것들은 제공해주는 app을 넣어 알림을 받는다.
우리가 만든 서비스에서 발생하는 Exception 이나 비즈니스의 특이사항들을 특정 채널에서 계속 포스팅되어
우리의 서비스가 잘 동작하고 있는지, 추후 어떤 부분을 개선해야 할지 면밀히 살필 수 있다.

[^slack]: Slack - https://slack.com/


## 아 불편해 (개발 동기)

입사 후 새로운 환경에서 생소한 코드를 마주하고 복잡한 비즈니스도 이해해가며 2주라는 시간이 정신 없이 흘렀다.

너무나도 좋은 동료분들을 만나 적응하는데 많은 도움을 받고 있지만,  
사소한 것부터 내 컴퓨터가 윈도우에서 맥으로 바뀌었고, 신경써야 하는 서버와 서비스는 더 많아졌으며,
써보고 싶었던 JIRA는 복잡한 프로그램이고 끊임 없이 이슈가 전달된다.  

이와중에 나에게 치명적인 문제가 하나 있었으니 나열된 숫자를 잘 못 읽거나 기억하지 못하는 문제다.
JIRA의 이슈번호라는게 `PRJT-5472` 처럼 프로젝트를 의미하는 약어와 숫자열이 오는데
5분 뒤에 사용할 전화번호도 못외우는 이슈번호만 가지고는 어떤 업무인지 내 업무인건지 단번에 파악하기가 너무 어려웠다.  

내가 진행하는 이슈에 대해서 이야기라도 나눠보려면 이슈번호 + 제목 + 링크를 찾아서 넣어주어야 하는 번거로움이 있다.

> Slack은 API와 마켓이 잘 되어 있어 다른 서비스와 연결이 용이하다. 구글 캘린더에 일정이 등록되면 자동으로 알려주고
> 레포지터리에 커밋이나 이벤트가 발생하면 채널에 알려주는 앱 설치는 클릭 몇 번으로 가능하다.  
>   
> 그런데 왜 도대체 JIRA 이슈번호를 입력하면 알아서 관련 정보를 연결해주는 앱은 없는 걸까?


## 내가 만들어 보자 (개발기)

2주의 적응기를 보내느라 성탄절 약속도 못 잡고 남아도는 시간을 이용해 봇을 개발해 보기로 했다.
이전 회사에서 Slack이 활성화되지는 않았지만 정상적으로 30분마다 웹페이지가 접근 가능한지 확인해서
슬랙에 알려주는 프로그램을 만들어 사용했던 경험이 있어 기술적으로 어렵다고 생각하진 않았다.  

처음 목표는 우리팀 프로젝트의 이슈번호 `SMFT-0000`을 인식해 클릭으로 JIRA 이슈페이지에 접근 할 수 있게 하는 것이였다.
정규표현식을 이용해 대화 내용중 우리팀 이슈번호 형태를 감지하고, `jira.example.com/browser/SMFT-0000` 링크를
걸어주는 일이였다. 매번 쓸때마다 어려운 정규표현식을 만드느라 시간이 걸렸지만 그리 오래 걸리진 않았다.

![첫 성공](/assets/images/content/2017/12/slack-juggler/first_run.png)

> 흠... 만들어보니 욕심이 생긴다. JIRA에서 제공하는 API는 없는 걸까? 이슈번호로 API를 날리면 json으로 응답주는
> 그런거 있잖아 요즘 다 좋은 프로그램들은 API를 가지고 있던데...[^jira-api]  
>   
> 있다. 있어!

[^jira-api]: JIRA API Document - https://developer.atlassian.com/server/jira/platform/

![JIRA API 성공](/assets/images/content/2017/12/slack-juggler/second_run.png)

이제부터는 노가다가 시작된다. 지금 생각하면 여기까지는 생각하지 말았어야 했다.

> 예쁘게 보여주고 싶어...  
> Slack에 다른 앱들은 알록달록하게 예쁘게 보여준단 말이야! [^slack-message]

[^slack-message]: Slack Messages Document - https://api.slack.com/docs/messages

![JIRA Message Design](/assets/images/content/2017/12/slack-juggler/jira.png)


아이디어를 구현해가다 보니 스스로 요구사항이 점점 많아졌고 욕심히 생긴다...
~~성탄절 이브의 새벽을 이렇게 써버렸다~~


## 자랑하고 싶다 (GitHub 공개)

더 문제는 잠시 휴식을 취하고 나니 봇 이름부터 여러가지 아이디어가 떠오른다.  

KBS 방영드라마 `저글러스`, 즐겨보지는 않지만 3주 전쯤에 1회차를 재미있게 봤다.
비서들의 이야기인데 `저글러스` 는 어떤 의미인지 검색해보니
저글러스 공식 홈페이지에 이렇게 소개되어 있다.

> Jugglers!  
> 양손과 양발로 수십 가지 일을 하면서도 보스의 가려운 부분을 긁어줄 줄 아는 저글링 능력자 언니들을 아는가?  
> 어디선가, 보스에게, 무슨 일이 생기면 반드시 나타나는 오피스 히로인즈!  
> 그 이름하야, 저글러스!

내 봇에 `slack-juggler` 라는 괜찮은 이름도 지었고 메시지 모양을 만드느라
수 백번은 이슈번호를 Slack에 날린것 같다. 자랑하고 싶었고 누군가 필요로 하는 사람이 있을 것 같아
GitHub에 공개했다.

레포지터리를 단장하고 이걸 필요로 하는 사람이 이용할 수 있도록 기능과 설정방법을 README에 정리하다보니
이런 생각이 든다.

> 프로퍼티만 변경해서 바로 사용할 수 있으면 좋겠다.

또 코드를 수정한다.

이렇게 친절하게 문서까지 만들어 놓고 보니 언젠가는 내 레파지토리에도 설치 방법부터 코드 문의까지
다양한 이슈가 올라오겠다는 생각이 든다.

> 어! 깃허브 이슈도 이슈번호로 조회해서 Slack에서 볼 수 있으면 좋겠다. 일해라 개발자여 이번에는 GitHub 이슈다!!  

~~이렇게 성탄절 이브도 다 간다~~

![GitHub Message Design](/assets/images/content/2017/12/slack-juggler/github.png)


## 리누스 토발즈 이야기...

<div style="max-width:854px"><div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://embed.ted.com/talks/lang/ko/linus_torvalds_the_mind_behind_linux" width="854" height="480" style="position:absolute;left:0;top:0;width:100%;height:100%" frameborder="0" scrolling="no" allowfullscreen></iframe></div></div>

IT분야에 조그만 관심이 있다면 알만한 리누스 토발즈 이야기를 해볼까 한다.
그는 자신의 필요로 리눅스 커널을 만드는 개인적인 프로젝트를 진행했고 자신이 생각하기에 충분히 멋져지자 인터넷에 소스코드를 공개한다.
영상에서 볼 수 있지만 그가 인터넷에 공개한 이유는 단순히 자신의 결과물에 대해서 이야기를 나눠보고 싶을 뿐이였다.

> 나는 1년 반 동안 이걸 만들었어 이것에 대해 이야기 해보자

공개된 소프트웨어는 그것을 필요로 하는 사람에게서 개선방향이 나오고, 관심있는 여러개발자들이 참여해
다듬어지고 완성되어 간다. 여러 개발자가 보내오는 패치파일을 일일이 검토하고 적용하는게 불편한 그는 리눅스 커널의
버전관리를 위해 Git 만들어 공개했고 현재 많은 개발자들이 Git을 통해 자신의 소프트웨어를 관리하고 협업한다.

요즘 대부분의 회사들은 Git을 이용해 오픈소스에 기여한 경험이 있는 사람을 우대사항의 조건에 넣기도 한다.


## Slack-Juggler 공개와 기대

나는 내 레포지터리에 많은 프로젝트가 있진 않다. 대부분 인터넷에 흔히 돌아다니는 소스코드들을
나중에 쉽게 볼 수 있도록 소스코드를 저장하는 나를 위한 공간이 대부분이였다.
그러다 처음으로 함께 하고 싶은 공간을 만들었고, 이것이 인기가 있던지 없던지 어떤 필요로 하는 사람으로
부터 아이디어가 나오고, 얼굴은 본적 없지만 다른 능력이 있는 개발자의 코드가 합쳐지는 새로운 경험을 할 수 있길 기대한다.


## Slack-Juggler 저장소 바로가기

<div class="github-card" data-github="binarythink/slack-juggler" data-width="400" data-height="150" data-theme="default"></div>
<script src="//cdn.jsdelivr.net/github-cards/latest/widget.js"></script>
