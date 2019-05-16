---
layout: post
title:  Develop this blog
categories: category blog
tags: blog
toc: true
original_url:
---

## 관련 정보
이 블로그를 만들 때 다음과 같은 정보를 활용하였다.

- [HTML5boilerplate](https://html5boilerplate.com/)
- [Initializr](http://www.initializr.com/)
- [Bootstrap vs. Boilerplate](https://www.upwork.com/hiring/development/bootstrap-vs-boilerplate/)
- [Bootswatch](https://bootswatch.com/)

`HTML5boilerplate`는 HTML5 기반의 front-end template이다.
이것은 초기에 개발을 쉽고 빨리 할 수 있도록 많은 모듈들이 정의되어 있다.

`Initializr`는 `HTML5boilerplate`와 `Bootstrap`을 함께 적용한 HTML5 templates generator이다.
현재, 업데이트된지 오래되어 이 `Initializr`를 참고하여, 직접 `HTML5boilerplate`와 `Bootstrap`을 합쳐서 작업을 했다.
이때, `Bootswatch`는 무료 테마를 제공하고 있어서 실제로는 `Bootswatch`를 적용했다.

## 작업과정
`HTML5boilerplate`의 파일들을 복사해서 가져와서 `Jekyll` 디렉토리에 다음과 같이 적용을 했다:
1. css, javascript는 /assets 폴더에 넣는다.(HTML5 Boilerplate v6.1.0의 main.css는 제외했다.)
2. `Bootswatch` 관련 파일을 /assets 폴더에 넣는다.
3. index.html 파일의 내용을 기반으로 default layout을 만들다.
4. HTML 소스코드의 모든 경로를 Jekyll 폴더에 맞게 수정한다.(여기서 시간이 가장 많이 걸린다.)
5. 공통 모듈을 쪼개서 각각 include할 수 있게 정의하여 `_include` 폴더에 구성을 한다.
6. `jekyll-toc` plugin을 설치하고 환결설정한 후 html 코드를 넣었다.
7. `jekyll-algolia` 설치


## 구동시키기
```sh
chcp 65001
jekyll algolia
jekyll serve
```
