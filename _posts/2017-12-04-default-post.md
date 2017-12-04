---
author: KYJ
permalink: /default-post
title: "기본적인 포스팅 형식"
excerpt: "컨텐츠의 일관성을 유지하기 위해 기본이 되는 포스팅 형식을 추가합니다."
header:
  #image: /assets/images/common/teaser.jpg
  og_image: /assets/images/common/teaser.jpg
  overlay_image: /assets/images/common/teaser.jpg
  overlay_filter: 0.5
  #caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  #cta_url: "https://unsplash.com"
categories:
  - Markup
tags: 
  - some tag
  - test
toc : true
---

글에 대한 간략한 내용을 소개합니다. 되도록 한 문단으로 끝낼 수 있도록 작성합니다.

## 제목 subject

포스트 내에서 사용할 제목은 `h2` 태그부터 시작합니다.  
toc 는 컨텐츠에 따라 설정하거나 뺄 수 있습니다. `toc`

### 서브 제목

* 목록1
  * 서브 목록1
  * 서브 목록2
* 목록 2

### 서브 제목 2

123123123

## 인용문구

인용문구는 인용 형식을 그대로 사용하되 출처를 `<cite>` 태그로 명시할 수 있도록 한다

> 행복은 인터페이스라 구현은 스스로 해야한다.  
> <cite>장 CTO</cite>


## 문서 기본 설정

```yaml
permalink: /default-post
title: "기본적인 포스팅 형식"
excerpt: "컨텐츠의 일관성을 유지하기 위해 기본이 되는 포스팅 형식을 추가합니다."
header:
  image: /assets/images/page-header-image.png
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  cta_url: "https://unsplash.com"
toc : true
``` 

## Syntax Highlighting

```java
class Test {
    public static void main(String[] args) {
        System.out.println("Hello World!!");
    }
}
```