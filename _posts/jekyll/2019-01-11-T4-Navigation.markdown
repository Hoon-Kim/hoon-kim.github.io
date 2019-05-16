---
layout: post
title:  "Navigation"
date:   2019-01-11 10:00:00 +0900
categories: category jekyll
tags: jekyll, navigation
toc: true
original_url: 'https://jekyllrb.com/tutorials/navigation/'
---

#### Jekyll site에서 페이지 추출하는 2가지 방법
- __YAML__:
  YAML(or JSON or CVS) 파일에서 페이지 데이터를 읽어서 원하는 목록을 추출하기.
- __front matter_
  각각의 페이지에 있는 front matter에서 원하는 프로퍼티를 찾아 목록을 추출하기.


#### 시나리오 형식
`_data` 폴더에 `simplelist.yml`이라는 YAML 파일을 만들어서 사용한다.
모든 시나리오는 YAML, Liquid, Result 의 항목으로 정리된다.



### Scenario 1: Basic List
다음은 기본 목록이다.

##### YAML
```yaml
docs_list_title: ACME Documentation
docs:

- title: Introduction
  url: introduction.html

- title: Configuration
  url: configuration.html

- title: Deployment
  url: deployment.html
```

#### Liquid
{% raw %}
```liquid
<h2>{{ site.data.samplelist.docs_list_title }}</h2>
<ul>
   {% for item in site.data.samplelist.docs %}
      <li><a href="{{ item.url }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>
```
{% endraw %}

#### 결과
<div class="highlight result no_toc_section" data-proofer-ignore>
   <h2>ACME Documentation</h2>
   <ul>
      <li><a href="#">Introduction</a></li>
      <li><a href="#">Configuration</a></li>
      <li><a href="#">Deployment</a></li>
   </ul>
</div>

YAML은 두 가지 중요한 내용을 포함하는데, 바로 데이터에 대한 __mapping__과 __list__이다.

`docs_list_title: ACME Documentation`은 __mapping__이고,
`docs`은 __list__이다.

여기서 hyphen(-)은 list의 Item을 의미한다.



### Scenario 2: Sorted list
제목 기준으로 소팅하기

#### Liquid
{% raw %}
```liquid
{% assign doclist = site.data.samplelist.docs | sort: 'title'  %}
<ol>
{% for item in doclist %}
    <li><a href="{{ item.url }}">{{ item.title }}</a></li>
{% endfor %}
</ol>
```
{% endraw %}

#### 결과
<div class="highlight result" data-proofer-ignore>
   <ol>
      <li><a href="#">Configuration</a></li>
      <li><a href="#">Deployment</a></li>
      <li><a href="#">Introduction</a></li>
   </ol>
</div>

__주의할 점:__ `assign` 또는 `capture` 태그를 사용해서, 변수에 리스르를 변환한 후,
for loop를 돌려야 한다. 다음과 같이 `for`와 `| sort`를 함께 사용하면 안 된다:

{% raw %}
```liquid
{% for item in site.data.samplelist.docs | sort: "title" %}{% endfor %}
```
{% endraw %}



### Scenario 3: Two-level navigation list
다음은 2단계 수준의 목록을 표현하는 방법이다.

##### YAML
```yaml
toc:
  - title: Group 1
    subfolderitems:
      - page: Thing 1
        url: /thing1.html
      - page: Thing 2
        url: /thing2.html
      - page: Thing 3
        url: /thing3.html
  - title: Group 2
    subfolderitems:
      - page: Piece 1
        url: /piece1.html
      - page: Piece 2
        url: /piece2.html
      - page: Piece 3
        url: /piece3.html
  - title: Group 3
    subfolderitems:
      - page: Widget 1
        url: /widget1.html
      - page: Widget 2
        url: /widget2.html
      - page: Widget 3
        url: /widget3.html
```

#### Liquid
{% raw %}
```liquid
{% for item in site.data.samplelist.toc %}
    <h3>{{ item.title }}</h3>
      <ul>
        {% for entry in item.subfolderitems %}
          <li><a href="{{ entry.url }}">{{ entry.page }}</a></li>
        {% endfor %}
      </ul>
  {% endfor %}
```
{% endraw %}

#### 결과
<div class="highlight result no_toc_section" data-proofer-ignore>
    <h3>Group 1</h3>
      <ul>
          <li><a href="#">Thing 1</a></li>
          <li><a href="#">Thing 2</a></li>
          <li><a href="#">Thing 3</a></li>
      </ul>

    <h3>Group 2</h3>
      <ul>
          <li><a href="#">Piece 1</a></li>
          <li><a href="#">Piece 2</a></li>
          <li><a href="#">Piece 3</a></li>
      </ul>

    <h3>Group 3</h3>
      <ul>
          <li><a href="#">Widget 1</a></li>
          <li><a href="#">Widget 2</a></li>
          <li><a href="#">Widget 3</a></li>
      </ul>
</div>

이중 loop를 사용했다.



### Scenario 4: Three-level navigation list
다음은 3단계 수준의 목록을 표현하는 방법이다.

##### YAML
```yaml
toc2:
  - title: Group 1
    subfolderitems:
      - page: Thing 1
        url: /thing1.html
      - page: Thing 2
        url: /thing2.html
        subsubfolderitems:
          - page: Subthing 1
            url: /subthing1.html
          - page: Subthing 2
            url: /subthing2.html
      - page: Thing 3
        url: /thing3.html
  - title: Group 2
    subfolderitems:
      - page: Piece 1
        url: /piece1.html
      - page: Piece 2
        url: /piece2.html
      - page: Piece 3
        url: /piece3.html
        subsubfolderitems:
          - page: Subpiece 1
            url: /subpiece1.html
          - page: Subpiece2
            url: /subpiece2.html
  - title: Group 3
    subfolderitems:
      - page: Widget 1
        url: /widget1.html
        subsubfolderitems:
          - page: Subwidget 1
            url: /subwidget1.html
          - page: Subwidget 2
            url: /subwidget2.html
      - page: Widget 2
        url: /widget2.html
      - page: Widget 3
        url: /widget3.html
```

#### Liquid
{% raw %}
```liquid
<div>
{% if site.data.samplelist.toc2[0] %}
  {% for item in site.data.samplelist.toc2 %}
    <h3>{{ item.title }}</h3>
      {% if item.subfolderitems[0] %}
        <ul>
          {% for entry in item.subfolderitems %}
              <li><a href="{{ entry.url }}">{{ entry.page }}</a>
                {% if entry.subsubfolderitems[0] %}
                  <ul>
                  {% for subentry in entry.subsubfolderitems %}
                      <li><a href="{{ subentry.url }}">{{ subentry.page }}</a></li>
                  {% endfor %}
                  </ul>
                {% endif %}
              </li>
          {% endfor %}
        </ul>
      {% endif %}
    {% endfor %}
{% endif %}
</div>
```
{% endraw %}

#### 결과
<div class="highlight result no_toc_section" data-proofer-ignore>
   <div>
      <h3>Group 1</h3>
      <ul>
         <li><a href="#">Thing 1</a></li>
         <li><a href="#">Thing 2</a></li>
         <ul>
            <li><a href="#">Subthing 1</a></li>
            <li><a href="#">Subthing 2</a></li>
         </ul>
         <li><a href="#">Thing 3</a></li>
      </ul>
      <h3>Group 2</h3>
      <ul>
         <li><a href="#">Piece 1</a></li>
         <li><a href="#">Piece 2</a></li>
         <li><a href="#">Piece 3</a></li>
         <ul>
            <li><a href="#">Subpiece 1</a></li>
            <li><a href="#">Subpiece2</a></li>
         </ul>
      </ul>
      <h3>Group 3</h3>
      <ul>
         <li><a href="#">Widget 1</a></li>
         <ul>
            <li><a href="#">Subwidget 1</a></li>
            <li><a href="#">Subwidget 2</a></li>
         </ul>
         <li><a href="#">Widget 2</a></li>
         <li><a href="#">Widget 3</a></li>
      </ul>
   </div>
</div>


### Scenario 5: Using a page variable to select the YAML list

페이지 마다 사이드바 목록이 다를 경우, 페이지에 사이드바 이름을 저장하고, 해당 이름의 목록을 가져오게 한다.

##### Page front matter
```yaml
---
title: My page
sidebar: toc
---
```

#### Liquid
{% raw %}
```liquid
<ul>
    {% for item in site.data.samplelist[page.sidebar] %}
      <li><a href="{{ item.url }}">{{ item.title }}</a></li>
    {% endfor %}
</ul>
```
{% endraw %}

#### 결과
<div class="highlight result" data-proofer-ignore>
   <ul>
      <li><a href="#">Introduction</a></li>
      <li><a href="#">Configuration</a></li>
      <li><a href="#">Deployment</a></li>
   </ul>
</div>

__참고:__ [Expressions and Variables](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#expressions-and-variables){:target="_blank"},
[Stack Overflow answer](https://stackoverflow.com/questions/4968406/javascript-property-access-dot-notation-vs-brackets/4968448#4968448){:target="_blank"}



### Scenario 6: Applying the active class for the current page
보고 있는 페이지에 highlight를 주고 싶을 때

##### CSS
```css
.result li.active a {
    color: lightgray;
    cursor: default;
  }
```

#### Liquid
{% raw %}
```liquid
{% for item in site.data.samplelist.docs %}
    <li class="{% if item.url == page.url %}active{% endif %}">
      <a href="{{ item.url }}">{{ item.title }}</a>
    </li>
{% endfor %}
```
{% endraw %}

#### 결과
<style>
.result li.active a {
    color: lightgray;
    cursor: default;
  }
</style>
<div class="highlight result" data-proofer-ignore>
   <ul>
      <li class=""><a href="#">Introduction</a></li>
      <li class=""><a href="#">Configuration</a></li>
      <li class="active"><a href="#">Deployment</a></li>
   </ul>
</div>



### Scenario 7: Including items conditionally

페이지의 조건에 따라 목록을 다르게 표현할 때 사용한다.

##### YAML
```yaml
docs2_list_title: ACME Documentation
docs2:

- title: Introduction
  url: introduction.html
  version: 1

- title: Configuration
  url: configuration.html
  version: 1

- title: Deployment
  url: deployment.html
  version: 2
```

#### Liquid
{% raw %}
```liquid
<ul>
  {% for item in site.data.samplelist.docs2 %}
    {% if item.version == 1 %}
      <li><a href="{{ item.url }}">{{ item.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
```
{% endraw %}

#### 결과
<div class="highlight result" data-proofer-ignore>
   <ul>
      <li><a href="#">Introduction</a></li>
      <li><a href="#">Configuration</a></li>
   </ul>
</div>



### Scenario 8: Retrieving items based on front matter properties

페이지 목록을 YAML 파일로 관리하고 싶지 않을때,
각각의 페이지 또는 Collection에 있는 `front matter`를 통해서 얻는 방법이 있다.

참고로, Collection을 사용하려면, 미리 `_config.yml`에 정의를 해야 한다:
```yaml
collections:
  docs:
    output: true
```

`_docs`라는 폴더에 6개의 문서 __Collection__(Sample 1, Sample 2, Topic 1, Topic 2, Widget 1, and Widget 2)가 있다는 것을 가정한다.


각 페이지의 `front matter tag`는 다음과 같다:
##### YAML
```yaml
---
Title: Sample 1
category: getting-started
order: 1
---

---
Title: Sample 2
category: getting-started
order: 2
---

---
Title: Topic 1
category: configuration
order: 1
---

---
Title: Topic 2
category: configuration
order: 2
---

---
Title: Widget 1
category: deployment
order: 1
---

---
Title: Widget 2
category: deployment
order: 2
---
```

#### Liquid
{% raw %}
```liquid
<h3>Getting Started</h3>
<ul>
    {% for doc in site.docs %}
      {% if doc.category == "getting-started" %}
        <li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
      {% endif %}
    {% endfor %}
</ul>
```
{% endraw %}

#### 결과
<div class="highlight result no_toc_section" data-proofer-ignore>
   <h3>Getting Started</h3>
   <ul>
      <li><a href="#">Sample1</a></li>
      <li><a href="#">Sample2</a></li>
   </ul>
</div>




#### 카테고리를 Grouping하기

#### Liquid
{% raw %}
```liquid
{% assign mydocs = site.docs | group_by: 'category' %}
{% for cat in mydocs %}
    <h2>{{ cat.name | capitalize }}</h2>
    <ul>
      {% assign items = cat.items | sort: 'order' %}
      {% for item in items %}
        <li><a href="{{ item.url }}">{{ item.title }}</a></li>
      {% endfor %}
    </ul>
{% endfor %}
```
{% endraw %}

#### 결과
<div class="highlight result no_toc_section" data-proofer-ignore>
   <h2>Getting-started</h2>
   <ul>
      <li><a href="#">Sample2</a></li>
      <li><a href="#">Sample1</a></li>
   </ul>
   <h2>Configuration</h2>
   <ul>
      <li><a href="#">Topic2</a></li>
      <li><a href="#">Topic1</a></li>
   </ul>
   <h2>Deployment</h2>
   <ul>
      <li><a href="#">Widget2</a></li>
      <li><a href="#">Widget1</a></li>
   </ul>
</div>

### 참고 URL
- [Liquid](https://shopify.github.io/liquid/){:target="_blank"}
- [Liquid Filters](https://jekyllrb.com/docs/liquid/filters/){:target="_blank"}
- [Tags Filters](https://jekyllrb.com/docs/liquid/tags/){:target="_blank"}
