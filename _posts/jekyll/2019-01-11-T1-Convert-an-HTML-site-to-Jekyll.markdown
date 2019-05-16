---
layout: post
title:  "Convert an HTML site to Jekyll"
date:   2019-01-11 10:00:00 +0900
categories: category jekyll
tags: jekyll, convert
toc: true
original_url: 'https://jekyllrb.com/tutorials/convert-site-to-jekyll/'
---

### Jekyll Website 설치
jekyll gem을 [설치](https://jekyllrb.com/docs/installation/){:target="_blank"}하면 기본 디렉토리와 파일들이 구성된다.


**_config.yml** 파일을 수정한다.

```yaml
name: My Jekyll Website
```

**_layouts/default.html** 파일을 추가한다.

```html
<!DOCTYPE html>
<html>
  <body>
     {% raw %}{{ content }}{% endraw %}
  </body>
</html>
```

**index.md** 파일을 수정한다.

```yaml
---
title: My page
layout: default
---

# {% raw %}{{ page.title }}{% endraw %}

Content is written in [Markdown](https://learnxinyminutes.com/docs/markdown/){:target="_blank"}. Plain text format allows you to focus on your **content**.

<!--
HTML elements를 사용할 수 있는데, 이 경우 markdown 문법이 적용되지는 않는다.
-->
```

서버 구동하기:
```sh
jekyll serve
```

`Gemfile` 파일이 있다면, Bundler를 사용할 수 있다:
```sh
bundle exec jekyll serve
```

브라우저로 확인할 경우 경로는 `http://127.0.0.1:4000/` 또는 `http://localhost:4000/`이다.
기본적으로 `_site` 폴더가 `/`이다.



### 1. Create a template for your default layout
원하는 테마를 찾아 소스코드를 다운로드한다.

`_layouts/default.html` 파일을 copy & paste한다. 이 파일이 기본 레이아웃 템플릿이 된다.

경우에 따라서 `CSS`, `JS`, `images` 등도 가져와야 한다.

이때, 경로가 중요하다. Jekyll 사이트와 동일한 assets을 가져오거나, 또는 절대 경로를 사용해야 한다.
(src = "//와 같은 구문은 로컬 브라우저에서 작업하려면 src ="http : //와 같은 접두사가 필요하다.)

Jekyll은 경로 앞에 site URL을 붙여주는 filter 기능을 제공한다:

{% raw %}
```markdown
{{ "/assets/style.css" | relative_url }}
```
{% endraw %}

`relative_url` 필터는 환경설정에 있는 `baseurl`을 경로 앞에 붙여준다.
예를 들면,
baseurl이 **blog**일 경우 `http://mysite.com/blog/`와 같이 도메인의 root 경로가 아니라 서브 경로에 호스팅될 때 유용하다.


`absolute_url` 필터는 `url`과 `baseurl`을 함께 경로 앞에 붙여준다.
{% raw %}
```markdown
{{ "/assets/style.css" | absolute_url }}
```
{% endraw %}

다시 말해서, 환경설정 파일에 다음과 같이 `url`과 `baseurl`이 정의되어 있다면:
```markdown
url: http://mysite.com
baseurl: /blog
```
전체 경로로는 `http://mysite.com/blog/assets/style.css`가 된다.

[Filter 참고](https://jekyllrb.com/docs/liquid/filters/){:target="_blank"}


모든 페이지의 url 속성은 슬래시(/)로 시작하므로 url 또는 baseurl 속성의 끝에는 /를 빼야한다.

url 필터를 넣을 필요는 없다.
또는 전체적으로 `relative_url` 필터를 사용해도 된다.
중요한 것은 assets에 따라 path를 코딩하고, 올바로 연결되었는지 확인해야 한다.



### 2. Identify the content part of the layout

복사한 default.html에서 페이지 콘텐츠가 시작되는 부분(보통 h1 또는 h2 태그)을 찾고,
태그 내부에 나타나는 제목을 {% raw %}`{{ page.title }}`{% endraw %}로 바꾼다.

콘텐츠 부분을 제거하고 (탐색 메뉴, 사이드바, 바닥글 등은 유지) {% raw %}`{{content}}`{% endraw %}로 바꾼다.

화면이 꺠지는지 잘 확인하라.



### 3. Create a couple of files with front matter tags

`index.md`와 `about.md` 파일을 루트 디렉토리에 만들고, 다음과 같이 front matter tags를 넣는다.

```html
---
title: Home
layout: default
---

Some page content here...
```

사실, 위 파일들은 Jekyll이 프로젝트를 생성할 때 이미 다 만들어져 있다.



### 4. Add a configuration file

`_config.yml` 파일이 환경설정 파일이다.
이것도 자동으로 이미 자동으로 생성되어 있다.

title, email, description, username 등은 적당한 것으로 변경한다.



### 5. Test your pages

`jekyll serve` 명령으로 실행해서 화면을 확인한다.



### 6. Configure site variables

`default.html` 파일에서 head 태그에 있는 title 태그 부분을 다음과 같이 변경한다.:
{% raw %}
```html
<title>{{ page.title }} | {{ site.title }}</title>
```
{% endraw %}

`_config.yml`의 title은 site 네임스페이스에서, 해당 페이지의 title은 page 네임스페이스에서 값을 가져올 수 있다.

__참고:__ `_config.yml` 파일을 수정한 경우, Ctrl+C로 중지시키고, 재구동시켜야 변경된 사항이 적용된다.



### 7. Show posts on a page

`_posts` 폴더에 `YYYY-MM-DD-title.md` 또는 `YYYY-MM-DD-title.markdown` 형식으로 만든다.

각각의 post는 다음과 같이 layout을 default로 한다:
```html
---
title: My First Post
layout: default
---

Some sample content...
```

`_layouts` 폴더에 `home.html` 이라는 새로운 템플릿을 만들고, 다음과 같은 로직을 추가한다:
{% raw %}
```html
---
layout: default
---

{{ content }}
<ul class="myposts">
{% for post in site.posts %}
    <li><a href="{{ post.url }}">{{ post.title}}</a>
    <span class="postDate">{{ post.date | date: "%b %-d, %Y" }}</span>
    </li>
{% endfor %}
</ul>
```
{% endraw %}

`blog.md` 파일을 루트 디렉토리에 만들고, `home` layout으로 설정한다:
```html
---
title: Blog
layout: home
---
```

이젠 home.html은 블로그 목록이 나타난다.

#### layout에 관하여
하나의 layout이 다른 layout에 포함될 수 있다.
이때, `front matter tags`의 layout으로 지정된 또다른 layout의 \{\{ content \}\} 태그 안으로 들어가게 된다.



#### [For loops](https://shopify.github.io/liquid/tags/iteration/){:target="_blank"}
[forloop의 object 속성](https://help.shopify.com/en/themes/liquid/objects/for-loops){:target="_blank"}도 존재한다.

다음과 같이 for loop에서 limit 속성을 지정할 수도 있다:
{% raw %}
```html
<ul class="myposts">
{% for post in site.categories.podcasts limit:3 %}
    <li><a href="{{ post.url }}">{{ post.title}}</a>
    <span class="postDate">{{ post.date | date: "%b %-d, %Y" }}</span>
    </li>
{% endfor %}
</ul>
```
{% endraw %}

### 8. Configure navigation

#### 목록수가 많지 않을 때
`site.pages` object를 이용하여 포스트 목록 만들기:
{% raw %}
```html
<ul>
  {% assign mypages = site.pages | sort: "order" %}
  {% for page in mypages %}
  <li><a href="{{ page.url | absolute_url }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>
```
위 내용은 아래와 같이 `front matter tags`에 order 속성이 있을 경우, 순서대로 소팅한다:
```markdown
---
title: My page
order: 2
---
```
{% endraw %}

### 목록수가 많을 때
별도의 페이지에서 포스트 목록을 관리할 수 있다.

루트 디렉토리에 `_data` 폴더를 만들고, `navigation.yml` 파일을 만든 후,
아래와 같은 포스트 목록을 넣는다:
```yml
- title: Sample page 1
  url: /page-1-permalink/

- title: Sample page 2
  url: /page-2-permalink/

- title: Sample page 3
  url: /page-3-permalink/
```

`navigation`이라는 파일명으로 했으므로, `site.data.navigation` object를 for loop로 돌린다.
{% raw %}
```html
<ul>
    {% for link in site.data.navigation %}
    <li><a href="{{ link.url }}">{{ link.title }}</a></li>
    {% endfor %}
</ul>
```
{% endraw %}

__Note:__ 자세한 내용은 [Navigation]({{ site.baseurl }}{% post_url jekyll/2019-01-11-T4-Navigation %}){:target="_blank"}


### 9. Simplify your site with includes
`default.html`의 공통모듈에 해당하는 부분(sidebar, header, fotter 등)들은 다른 파일로 분리할 수 있으며,
*include* 명령으로 루트 디렉토리에 있는 `_includes` 폴더에서 해당 파일을 가져올 수 있다.


```liquid
{% raw %}{% include sidebar.html %}{% endraw %}
```


### 10. RSS feed
루트디렉토리에 `feed.xml`파일을 만들고, 다음과 같은 내용을 넣는다:
{% raw %}
```xml
---
layout: null
---

<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

    <channel>
        <title>{{ site.title }}</title>
        <link>{{ site.url }}</link>
        <description>{{ site.description }}</description>
        <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
        {% for post in site.posts %}
        <item>
            <title>{{ post.title }}</title>
            <link>
                {{ post.url | prepend: site.url }}
            </link>
            <description>
                {{ post.content | escape | truncate: '400' }}
            </description>
            <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
            <guid>
                {{ post.url | prepend: site.url }}
            </guid>
        </item>
        {% endfor %}
    </channel>
</rss>
```
{% endraw %}

**참고:**
[기본 RSS구문](https://www.w3schools.com/xml/xml_rss.asp),
[Liquid Filters](https://help.shopify.com/en/themes/liquid/filters)

RSS 링크걸기:
{% raw %}
```html
<link rel="alternate" type="application/rss+xml"  href="{{ site.url }}/feed.xml" title="{{ site.title }}">
```
{% endraw %}

[jekyll-feed](https://help.github.com/articles/atom-rss-feeds-for-github-pages/) 플러그인을 추가하여 자동으로 생성할 수 있다.



### 11. Add a sitemap
[사이트맵](https://www.sitemaps.org/protocol.html)은 루트 디렉토리에 `sitemap.xml`을 추가한다.
{% raw %}
```xml
---
layout: null
search: exclude
---

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">

    {% for page in site.pages %}
    <url>
        <loc>{{page.url}}</loc>
        <lastmod>{{site.time | date: '%Y-%m-%d' }}</lastmod>
        <changefreq>daily</changefreq>
        <priority>0.5</priority>
    </url>
    {% endfor %}

    {% for post in site.posts %}
    <url>
        <loc>{{post.url}}</loc>
        <lastmod>{{site.time | date: '%Y-%m-%d' }}</lastmod>
        <changefreq>daily</changefreq>
        <priority>0.5</priority>
    </url>
    {% endfor %}

</urlset>
```
{% endraw %}

[jekyll-sitemap](https://help.github.com/articles/sitemaps-for-github-pages/) 플러그인을 추가하여 자동으로 생성할 수 있다.



### 12. Add external services

- 댓글달기 : [DISQUS](https://disqus.com/)
- 뉴스레터: [TinyLetter](https://tinyletter.com/)
- 연락 폼: [WuFoo](https://www.wufoo.com/)
- 검색: [Algolia Docsearch](https://community.algolia.com/docsearch/)
- 정적 사이트를 위한 [Third Parties 서비스](https://learn.cloudcannon.com/jekyll-third-parties/) from CloudCannon.

`front matter tags`가 없는 페이지는 Jekyll이 어떤 처리도 하지 않고, 그대로 `_site` 폴더로 보내진다.
그래서, 루트 디렉토리에 다양한 폴더 구조와 페이지를 만들 수 있다.

만약, layout이 필요 없다면, `layout: null`로 설정하면 된다.



### 13. Conclusion
#### Deploy 하기
[s3_website](https://github.com/laurilehmijoki/s3_website) plugin을 이용하여,
[GitHub Pages](https://pages.github.com/), [Netlify](https://www.netlify.com/), [Amazon AWS S3](https://aws.amazon.com/ko/s3/) 등에 올려라.



### Additional resources
