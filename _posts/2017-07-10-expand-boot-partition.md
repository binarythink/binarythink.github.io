---
permalink: /expand-boot-partition
title: "커널업데이트를 위한 boot 파티션 확보"
excerpt: "CentOS 의 커널 업데이트시 boot 용량이 충분하지 않을 경우, package-cleanup 명령을 이용해 이전 커널을 삭제하여 공간을 확보하는 방법에 대해 알아봅니다."
header:
  teaser: /assets/images/content/2017/07/expand-boot-partition/teaser.jpg
  og_image: /assets/images/content/2017/07/expand-boot-partition/teaser.jpg
  overlay_image: /assets/images/content/2017/07/expand-boot-partition/teaser.jpg
  overlay_filter: 0.5
categories:
  - environment
tags: 
  - environment
  - centos
  - kernel
  - 커널
  - 부트파티션 용량
toc : true
---
CentOS 의 커널 업데이트시 boot 용량이 충분하지 않을 경우, package-cleanup 명령을 이용해 이전 커널을 삭제하여 공간을 확보하는 방법에 대해 알아봅니다.

## 현상
`yum` 을 이용해 커널을 업데이트 하려하였으나, `yum update` 시 아래와 같은 오류를 확인할 수 있다.  
`/boot 에 최소 27MB 용량이 필요하다`고 하다고 안내하며 업데이트가 되지 않는다.

```
Transaction Check Error:
  installing package kernel-2.6.32-696.3.1.el6.x86_64 needs 27MB on the /boot filesystem

Error Summary
-------------
Disk Requirements:
  At least 27MB more space needed on the /boot filesystem.
```

## yum 설정 변경
`/etc/yum.conf` 파일의 `installonly_limit` 항목을 수정해서 커널을 몇 개까지 boot 파티션을 보관할지 지정한다.

아래와 같이 설정하면 최근 2개의 커널만 boot 파티션에 보관한다(그 중 하나는 현재 구동 중임)
```
installonly_limit=2
```

## package-cleanup
`package-cleanup` 명령을 사용하여 예전 커널을 지운다. 
```
$ package-cleanup --oldkernels --count=2
```

`package-cleanup`은 `yum-utils` 패키지에 포함되어 있으므로 서버에서 `package-cleanup` 명령을 사용할 수 없는 경우 `yum install yum-utils` 명령을 이용해 설치해준다.