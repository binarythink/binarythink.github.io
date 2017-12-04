---
permalink: /default-post
title: "기본적인 포스팅 형식"
excerpt: "컨텐츠의 일관성을 유지하기 위해 기본이 되는 포스팅 형식을 추가합니다."
author: Cornelius Fiddlebone
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

본 컨텐츠는 블로그 운영시에 사용할 기본 문서 형식에 대한 내용을 다루고 있습니다.
컨텐츠의 일관성을 유지해 전달하고자 하는 내용이 구조적으로 전달 될 수 있도록 노력하고 있습니다.
일반 사용자에게는 해당되지 않는 내용입니다. *컨텐츠에 대한 간략한 요약을 한문단으로 작성한다*


## 제목 subject

포스트에서 사용할 제목은 `<h2>` 태그부터 시작합니다.  
`toc: true` 일때 제목 태그를 수집하여 자동으로 목차가 구성되며 `toc` 는 글의 성격에 따라
뺄 수도 있습니다.


## 문서 작성시 기본 header
```yaml
permalink: /default-post
title: "기본적인 포스팅 형식"
excerpt: "컨텐츠의 일관성을 유지하기 위해 기본이 되는 포스팅 형식을 추가합니다."
author: Cornelius Fiddlebone
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
```

## 글의 형식

### 인용(quote)

인용문구는 인용 형식을 그대로 사용하되 출처를 `<cite>` 태그로 명시하도록 합니다.

> 행복은 인터페이스라 구현은 스스로 해야한다.  
> <cite>장 CTO</cite>

### 목록

* 순서가 없는 목록1
  * 서브 목록1
  * 서브 목록2
* 순서가 없는 목록2
  * 서브 목록1

1 순서가 있는 목록1
  1 서브 목록1
  1 서브 목록2
1 순서가 있는 목록2
  1 서브 목록1


### 링크

[my repository](http://github.com/binarythink)


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