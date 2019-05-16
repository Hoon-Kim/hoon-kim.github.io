---
layout: post
title:  Concept of Jekyll
date:   2018-12-01 10:00:00 +0900
categories: category jekyll
tags: jekyll, concept
toc: true
original_url:
---

## Jekyll 이란?
static website generator.<br>
Jekyll은 블로그와 같은 정적 웹사이트를 만들어 주는 Ruby 기반의 프레임워크이자 웹서버이다.<br>
원하는 콘텐츠를 markdown 파일로 작성하고, 레이아웃 템플릿을 만들고, 전체 HTML 웹 사이트를 생성한다.<br>
backend 프로그래밍이나 데이터베이스가 필요으며, 웹 서버에 즉각 배포할 수 있다.


#### 특징
이 안에 Markdown, Liquid, HTML, CSS의 기능이 모두 포함되어 있다.<br>
자체가 웹서버이므로 따로 서버 구축을 할 필요가 없다.<br>
데이터베이스가 필요없다.<br>
Javascript, CSS 등을 적용하는데 어떠한 제한도 없다.<br>
디자인 자체를 마음대로 customizing할 수 있다.<br>
다양한 기능의 플러그인을 추가할 수 있다.<br>


#### 장점
All-in-one 형태로 단순하게 설치해서 사용 가능하다.<br>
초보자도 쉽게 접근할 수 있다.<br>
사용자는 즉각 블로그 컨텐트에만 집중할 수 있도록 개발되어져 있다.<br>
기존에 운영중하고 있던 여러가지 다른 블로그들을 migration할 수 있다.<br>
각종 테마 디자인을 적용할 수 있다.<br>
각종 플러그인을 적용하여 다양한 기능을 추가할 수 있다.<br>
GitHub Pages 사이트에 그대로 무료 호스팅이 가능하다.<br>


## 알아야 하는 용어

#### Github Page
Github와 연동되는 개인 블로그 또는 프로젝트를 위한 웹사이트이자 호스팅 서비스이다.<br>


#### gem
Ruby 언어로 만들어진 프로그램 또는 라이브러리

#### RubyGems
Ruby Gem package manager이다.<br>
Ruby 소프트웨어 패키지를 쉽게 다운로드, 설치, 관리(download, install, maintain)한다.
`gem` 명령어를 사용한다.

#### Bundler
Ruby 프로젝트에서 정확한 gem과 필요한 버전을 설치하고, 추적해 주는 일관된 환경을 제공해 준다.<br>
프로젝트마다 종속성을 추적해 준다.<br>
다른 버전의 Jekyll을 실행킬 때 유용하다.<br>
시스템 레벨이나 사용자 레벨에 Jekyll을 설치하고 싶지 않을때(즉, 폴더별로 설치하고 싶을때) 유용하다.

#### Gemfile, Gemfile.lock
    Bundler 관련 파일로서, Jekyll 사이트를 빌드하기 위해 필요한 루비 젬과 그 버전을 기록하는데 사용된다.

#### Markdown
텍스트 기반의 마크업언어로 2004년 존그루버에 의해 만들어졌으며, 쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다.<br>
가볍고, 사용하기 간단하며, 가독성을 높이는 스타일링이 적용된 구문(syntax)이다.<br>
[github.com](https://github.com/){:target="_blank"}에서 `README.md`파일에 적용되어 널리 퍼지게 되었다.

#### Front Matter Tags
파일의 처음에 구성되어야 하며, `---` 라인과 `---` 라인 사이에 `YAML`구문이 들어간다.<br>
layout 구성 및 각종 변수 설정을 하며, Liquid Tags와 연동된다.<br>
Front matter variables(변수)는 `Liquid`에서 사용된다.

#### YAML
인간이 직접 읽고 쓸수 있도록 만들어진 데이터 직렬화 언어(data serialisation language)이다.<br>
들여쓰기 및 XML의 특수기호를 사용하기 때문에, XML과 거의 비슷하다.<br>
Yet Another Markup Language<br>
YAML Ain't Markup Language

#### Liquid
Ryby로 작성되었으며, Shopify가 만든 템플릿 언어이다.<br>
Jelyll에서 확장하여 사용하고 있다.<br>
현재 Liquid, Shopify Liquid, Jekyll Liquid 등으로 3가지 버전이 있다.<br>
objects, tags, filters 등 세 가지 구성요소가 있다:
{% raw %}
  1. objects<br>
    페이지에 컨텐츠를 출력한다.<br>
    `{{` 와 `}}`로 표현
  2. tags<br>
    Control flow, Iteration, Variable assignments 등 로직과 제어흐름을 수행한다.<br>
    `{%` 와 `%}`로 표현
  3. filters<br>
    Liquid Objects의 output을 변경한다.<br>
    Object tags와 함께 사용된다.<br>
    `|`로 표현

{% endraw %}

#### Rouge
Rouge는 순수한 Ruby로 작성된 systax highlighter이다.<br>
100개 이상의 언어를 지원하며,
triple backticks( \`\`\`)으로 둘러싸인 코드블럭을 통해 systax highlighting을 할 수 있다<br>
`rougify` 명령어를 사용한다.

#### Layout
디자인을 말하며, theme와 관련있다.<br>
`_layouts` 폴더에서 html 파일이 관리되며, Jekyll에서 사용된다.<br>
Front matter 변수로 layout이 지정되면,
해당 페이지는 `_layouts` 폴더의 html파일의 {% raw %}`{{ contents }}`{% endraw %} 위치로 삽입된다.

#### 4000
서버포트는 4000번이다.
[http://localhost:4000](http://localhost:4000){:target="_blank"}으로 웹브라우저에서 접속해야 화면을 볼 수 있다.

#### Jekyll에서 사용하는 지정된 폴더
- _layouts
- _posts
- _includes
- _sites
- _sass
- _plugins
- _data
- assets


## FAQ

#### `bundle exec jekyll serve` vs `jekyll serve`?
multiple versions의 플러그인을 사용할 때 버전 충돌이 나므로,
bundle exec를 사용해서 해당 디렉토리에 있는 의존성을 검토해서 실행시키게 한다.<br>
`jekyll serve`일 경우, 해당 디렉토리가 아닌, gem이 설치된 영역에서 플러그인들이 관리된다.
