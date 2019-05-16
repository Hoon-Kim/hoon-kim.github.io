---
layout: post
title:  Step by Step Tutorial
categories: category jekyll
tags: jekyll, tutorial
toc: true
original_url:
---

# Include
`_includes` 폴더에 있는 페이지를 include하며,
다음과 같이 사용한다:

{% raw %}
```liquid
{% include navigation.html %}
```
{% endraw %}

#### Current page highlighting
`page.url`이라는 Liquid objects variable를 사용하여 include 파일에서 현재 페이지를 표시할 수 있다.

`_includes/navigation.html` 파일에 아래와 같이 코딩한다:

{% raw %}
```liquid
<nav>
  <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
    Home
  </a>
  <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
    About
  </a>
</nav>
```
{% endraw %}

> 만약, 컨텐츠 또는 폴더로 메뉴 등을 highlighting 하고 싶다면,
Front matter tags에 새로운 변수를 지정해서 그 변수로 구분하여 사용하는 것이 좋다.
Liquid에서는 문자열에서 특정 문자를 검색하는 함수 등이 없기 때문이다.<br>
또는, Javascript와 CSS class를 이용해서 하는 방법도 있다.



# Data Files
`_data` 폴더에서 YAML, JSON, CSV 파일로 데이터를 관리할 수 있다.
데이터 파일은 소스 코드에서 콘텐츠를 분리하여 사이트를보다 쉽게 관리 할 수 있는 좋은 방법이다.

`_data/navigation.yml` 파일에 아래와 같이 코딩한다:
```yaml
- name: Home
  link: /
- name: About
  link: /about.html
```
`site.data.navigation`로 데이터를 읽는다.
`_includes/navigation.html` 파일 안에 링크 정보를 코딩하는 것 보다 아래와 같이 변수를 읽어서 사용하면 된다:

{% raw %}
```liquid
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```
{% endraw %}



# Assets
CSS, JS, images는 `assets` 폴더 안에 다음과 같이 관리하는 것이 좋다.
```
├── assets
|   ├── css
|   ├── images
|   └── js
```

#### SASS
`_includes/navigation.html` 파일에 CSS class를 다음과 같이 지정한다:

{% raw %}
```liquid
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}class="current"{% endif %}>{{ item.name }}</a>
  {% endfor %}
</nav>
```
{% endraw %}


[Sass](https://sass-lang.com/){:target="_blank"}를 이용하여 다음과 같이 `/assets/css/styles.scss` 파일을 생성한다:
```sass
---
---
@import "main";
```
여기서 `@import "main"`의 의미는 `_sass/` 폴더에서 `main.scss` 파일을 읽어들인다는 뜻이다.

`_sass/main.scss` 파을은 다음고 같다.
```css
.current {
  color: green;
}
```

css 파일을 설정했으니, layout을 만드는 `_layouts/default.html` 파일에서 다음과 같이 css를 추가하면 된다.

{% raw %}
```liquid
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```
{% endraw %}



# Blogging

#### Posts
보통 포스트글은 `_posts` 폴더에 `년-월-일-제목.markdown` 형식으로 파일을 생성하게 된다.<br>
파일명에 `년-월-일`이 없으면, 해당 포스트는 생성되지 않는다.

`_posts/2018-08-20-bananas.md` 파일을 다음과 같이 만든다:
```markdown
---
layout: post
author: jill
---
A banana is an edible fruit – botanically a berry – produced by several kinds
of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size, color,
and firmness, but is usually elongated and curved, with soft flesh rich in
starch covered with a rind, which may be green, yellow, red, purple, or brown
when ripe.
```
`author`는 사용자 변수(custom variable)이며,
`layout` 변수에서 지정된 post 는 다음과 같이 `_layouts/post.html`를 만들면 된다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```
{% endraw %}

#### List posts
루트폴더에 `/blog.html`를 다음과 같이 생성한다:

{% raw %}
```liquid
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

`post.excerpt`는 기본적으로 컨텐츠의 첫번째 문단을 의미한다.


`_data/navigation.yml` 파일에도 Blog 메뉴를 추가한다:
```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
```

#### More posts
이후에 연결되기 때문에, 2개 더 포스트를 추가한다.

`_posts/2018-08-21-apples.md`:
```markdown
---
layout: post
author: jill
---
An apple is a sweet, edible fruit produced by an apple tree.

Apple trees are cultivated worldwide, and are the most widely grown species in
the genus Malus. The tree originated in Central Asia, where its wild ancestor,
Malus sieversii, is still found today. Apples have been grown for thousands of
years in Asia and Europe, and were brought to North America by European
colonists.
```

`_posts/2018-08-22-kiwifruit.md`:
```markdown
---
layout: post
author: ted
---
Kiwifruit (often abbreviated as kiwi), or Chinese gooseberry is the edible
berry of several species of woody vines in the genus Actinidia.

The most common cultivar group of kiwifruit is oval, about the size of a large
hen's egg (5–8 cm (2.0–3.1 in) in length and 4.5–5.5 cm (1.8–2.2 in) in
diameter). It has a fibrous, dull greenish-brown skin and bright green or
golden flesh with rows of tiny, black, edible seeds. The fruit has a soft
texture, with a sweet and unique flavor.
```


# Collections
collections는 posts와는 달리 파일명에 날짜를 넣을 필요가 없다.

#### Configuration
`_config.yml` 파일에 collections 변수를 지정한다:
```yaml
collections:
  authors:
```

#### Add authors
`_콜렉선명` 형식으로 폴더를 생성해야 하므로, `_authors` 폴더를 만든다.

다음과 같이 `jill.md`, `ted.md` 파일을 추가한다.

`_authors/jill.md`:
```markdown
---
short_name: jill
name: Jill Smith
position: Chief Editor
---
Jill is an avid fruit grower based in the south of France.
```

`_authors/ted.md`:
```markdown
---
short_name: ted
name: Ted Doe
position: Writer
---
Ted has been eating fruit since he was baby.
```
이제 `site.authors` collections으로 데이터에 접근할 수 있다.

#### Staff page
`staff.html` 파일을 다음과 같이 생성한다:

{% raw %}
```liquid
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2>{{ author.name }}</h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

`_data/navigation.yml` 파일에도 Staff 메뉴를 추가한다:
```yaml
- name: Home
  link: /
- name: About
  link: /about.html
- name: Blog
  link: /blog.html
- name: Staff
  link: /staff.html
```


#### Output a page
컬렉션에서 데이터 목록은 나오나, 실제로 아직까지는 해당 파일이 빌드되어 있지는 않아서, 링크를 걸 수 없다.

다음과 같이 `_config.yml` 파일에 `output: true` 옵션을 추가해야 파일이 생성된다:
```yaml
collections:
  authors:
    output: true
```

`staff.html` 파일에 링크를 추가한다:
{% raw %}
```liquid
---
layout: default
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```
{% endraw %}

또다른 layout `_layouts/author.html` 파일을 다음과 같이 생성한다:
{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```
{% endraw %}


#### Front matter defaults

authors 컬렉션에 자동으로 author layout을 적용하기 위해 Front matter defaults를 이용하여,
`_config.yml` 파일을 다음과 같이 설정한다:
```yaml
collections:
  authors:
    output: true

defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "default"
```

포스트는 post layout이 적용되고, authors 컬렉션은 author layout이 적용되며, 그외는 default layout이 적용된다.


#### List author’s posts
해당 페이지에서 자신의 게시물 목록만 나오도록 하기 위해, `short_name`이 같은 것만 추출하면 된다.

`_layouts/author.html` 파일을 다음과 같이 수정한다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}

<h2>Posts</h2>
<ul>
  {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
```
{% endraw %}

#### Link to authors page
`_layouts/post.html` 파일도 다음과 같이 수정한다:

{% raw %}
```liquid
---
layout: default
---
<h1>{{ page.title }}</h1>

<p>
  {{ page.date | date_to_string }}
  {% assign author = site.authors | where: 'short_name', page.author | first %}
  {% if author %}
    - <a href="{{ author.url }}">{{ author.name }}</a>
  {% endif %}
</p>

{{ content }}
```
{% endraw %}

Liquid Filter인 `| first` 를 사용해서, 해당 포스트 페이지의 author와 authors 컬렉션의 short_name과 같은 것 중 첫번째 항목만 나타날 것이다.
