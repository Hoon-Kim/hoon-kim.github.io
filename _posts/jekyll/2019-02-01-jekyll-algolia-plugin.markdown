---
layout: post
title:  jekyll-algolia plugin Tutorial
categories: category jekyll
tags: jekyll, jekyll-algolia, plugins
toc: true
original_url:
---

정적인 웹페이지인 Jekyll 사이트에서 검색 기능을 구현하기 위해서
Algolia 플러그인([jekyll-algolia](https://github.com/algolia/jekyll-algolia){:target="_blank"})을 설치한다.

## Algolia Webpage

1. 계정 생성
[Algolia](https://www.algolia.com/){:target="_blank"} 사이트에서 새로운 계정을 생성하고 로그인한다.

2. Dashboard
[Algolia Dashboard](https://www.algolia.com/dashboard){:target="_blank"}에서 `Create app` 버튼을 클릭하여 새로운 app을 생성한다.

3. App 생성
붉은색 동그라미 항목을 입력한 후 `Create` 버튼을 클릭한다:

    ![Create app]({{ "/assets/posts/algolia/create_application.png" | relative_url }} "Create app")

4. Region
현재 네트워크 상태에 따라 일본 또는 홍콩을 선택한다:

    ![Choose region]({{ "/assets/posts/algolia/choose_region.png" | relative_url }} "Choose region")

5. Dashboard
이제는 Dashboard 화면을 보게 된다. 왼쪽 메뉴에서 `API keys`를 클릭하여 키 정보를 확인한다.
이 값들은 나중에 환경설정에서 사용하게 된다:

    ![API keys]({{ "/assets/posts/algolia/api_keys.png" | relative_url }} "API keys")



## Plugin

#### Install
***reference:*** [Getting started](https://community.algolia.com/jekyll-algolia/getting-started.html){:target="_blank"}

`Gemfile`에 있는 Bundler group에 플러그인 목록에 `jekyll-algolia` 플러그인을 추가한다:
```ruby
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
  gem "jekyll-toc"
  gem 'jekyll-algolia'
```

그 다음, 아래와 같이 플러그인을 설치한다.
```sh
bundle install
```
`Bundle complete! 6 Gemfile dependencies, 41 gems now installed.` 메시지가 나온다.

`jekyll help` 명령을 하면, `subcommand` 목록에 `algolia`가 나온다.

#### Configuration
`_config.yml` 파일에 다음을 추가한다.
이 때, [Algolia dashboard](https://www.algolia.com/api-keys){:target="_blank"}를 참고해서 `application_id`와 `search_only_api_key`값을 입력한다:

```yaml
algolia:
  application_id: 'Application ID'
  index_name:     'whatever name you want'
  search_only_api_key: 'Search-Only API Key'
```
***Note:*** '(quotation mark)를 넣으면 안 된다.

#### Pushing data
DOS 창에서 character set을 변경한다.
```sh
chcp 65001
```

DOS 창에서 환경변수로 `ALGOLIA_API_KEY`를 세팅하고, indexing을 생성(Pushing data)하기 위해 다음을 실행한다:
```sh
set ALGOLIA_API_KEY=키값
jekyll algolia
```
이때 키값은 algolia에서 생성한 프로젝트의 `Admin API Key`값이다.(algolia 웹사이트에서 확인)

> 설명에는 아래와 같이 있는데, 이때 키값에 '(quotation mark)를 넣으면 안 된다:
> ```sh
ALGOLIA_API_KEY='your_admin_api_key' bundle exec jekyll algolia
```

***주의!***
> 보안상 ALGOLIA_API_KEY 환경 변수를 사용해야 한다. 비밀로 유지되어야 하며, 공유해서는 안된다.<br>
> 그러나, 편의상 소스디렉토리에 `_algolia_api_key` 파일을 생성하고, 그 안에 키값을 넣어서 사용할 수도 있다.<br>
> ALGOLIA_API_KEY 환경 변수가 없을 경우, 자동으로 `_algolia_api_key` 파일을 찾아 읽을 것이다.<br>
> 이 경우, 버전 관리 시스템에서 이 `_algolia_api_key` 파일을 절대로 커밋해서는 안된다.
> `.gitignore` 파일에 `_algolia_api_key`을 추가하라.


## Front-end

***reference:*** [Blog search](https://community.algolia.com/jekyll-algolia/blog.html){:target="_blank"}


#### Search page
검색 페이지인 `search.md` 파일을 생성하고 다음과 같이 코딩한다:
{% raw %}
```html
---
layout: default
---
<div class="home">
  {{ content }}
  <h1 class="page-heading">Posts</h1>
  <div id="search-searchbar"></div>
  <div class="post-list" id="search-hits">
    {% for post in site.posts %}
      <div class="post-item">
        {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h2>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h2>
        <div class="post-snippet">{{ post.excerpt }}</div>
      </div>
    {% endfor %}
  </div>
  {% include algolia.html %}
  <p class="rss-subscribe">subscribe <a href="{{ '/feed.xml' | relative_url }}">via RSS</a></p>
</div>
```
{% endraw %}

(실제로는 `site.posts`로 데이터를 가져오는 부분이 필요없다. algolia가 처리해 주기 때문이다.)

##### Adding widgets:
    `#search-searchbar`는 입력창이 표시되는 곳이며, `#search-hits`는 검색결과가 표시되는 곳이다.
    다음에 나오는 javascript 코드에서 id값이 일치되어야 한다.


#### Algolia code
include되는 `algolia.html`는 다음과 같이 작성한다:

{% raw %}
```html
<script src="https://cdn.jsdelivr.net/npm/instantsearch.js@2.6.0/dist/instantsearch.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/moment.min.js"></script>
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/instantsearch.js@2.6.0/dist/instantsearch.min.css">
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/instantsearch.js@2.6.0/dist/instantsearch-theme-algolia.min.css">

<script>
const search = instantsearch({
  appId: '{{ site.algolia.application_id }}',
  apiKey: '{{ site.algolia.search_only_api_key }}',
  indexName: '{{ site.algolia.index_name }}'
});

const hitTemplate = function(hit) {
  let date = '';
  if (hit.date) {
    date = moment.unix(hit.date).format('MMM D, YYYY');
  }

  let url = `{{ site.baseurl }}${hit.url}#${hit.anchor}`;

  const title = hit._highlightResult.title.value;

  let breadcrumbs = '';
  if (hit._highlightResult.headings) {
    breadcrumbs = hit._highlightResult.headings.map(match => {
      return `<span class="post-breadcrumb">${match.value}</span>`
    }).join(' > ')
  }

  const content = hit._highlightResult.html.value;

  return `
    <div class="post-item">
      <span class="post-meta">${date}</span>
      <h2><a class="post-link" href="${url}">${title}</a></h2>
      {{#breadcrumbs}}<a href="${url}" class="post-breadcrumbs">${breadcrumbs}</a>{{/breadcrumbs}}
      <div class="post-snippet">${content}</div>
    </div>
  `;
}

search.addWidget(
  instantsearch.widgets.searchBox({
    container: '#search-searchbar',
    placeholder: 'Search into posts...',
    poweredBy: true // This is required if you're on the free Community plan
  })
);

search.addWidget(
  instantsearch.widgets.hits({
    container: '#search-hits',
    templates: {
      item: hitTemplate
    }
  })
);

search.start();
</script>

<style>
.ais-search-box {
  max-width: 100%;
  margin-bottom: 15px;
}
.post-item {
  margin-bottom: 30px;
}
.post-link .ais-Highlight {
  color: #111;
  font-style: normal;
  text-decoration: underline;
}
.post-breadcrumbs {
  color: #424242;
  display: block;
}
.post-breadcrumb {
  font-size: 18px;
  color: #424242;
}
.post-breadcrumb .ais-Highlight {
  font-weight: bold;
  font-style: normal;
}
.post-snippet .ais-Highlight {
  color: #2a7ae2;
  font-style: normal;
  font-weight: bold;
}
.post-snippet img {
  display: none;
}
</style>
```
{% endraw %}


##### Templating:

    검색결과는 실제로 raw JSON format으로 리턴된다.
    그러므로, 프로그램적으로 `hitTemplate` 함수를 만들어서 적당히 layout을 구성해야 한다.

##### 날짜:
    기본적으로 Algolia 인덱스에 UNIX timestamp로 저장되어 표시되므로 날짜 형식을 변경해야 한다.
    여기서는 [moment.js](https://momentjs.com/docs/){:target="_blank"}를 활용했다.
