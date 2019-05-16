---
layout: post
title:  jekyll-algolia plugin options
categories: category jekyll
tags: jekyll, jekyll-algolia, plugins, options
toc: true
original_url: https://community.algolia.com/jekyll-algolia/options.html
---

모든 옵션 설정은 `_config.yml` 파일의 `algolia` section에 추가해야 한다.

#### extensions_to_index
default로 모든 `HTML` 파일과 `markdown` 파일만 indexing한다.
추가로 다른 언어(예, [AsciiDoc](http://www.methods.co.nz/asciidoc/) 또는 [Textile](https://github.com/textile))를 적용할 경우,
다음과 같이 설정한다.
```yaml
algolia:
  # Also index AsciiDoc and Textile files
  extensions_to_index:
    - html
    - md
    - adoc
    - textile
```

#### files_to_exclude
indexing을 생성하지 않을 소스 파일 목록을 정의한다.<br>
default로 루트에 있는 `index.html` 및 `index.md`는 indexing에서 제외된다.<br>
색인 제외를 위한 설정은 다음과 같이 하며,
한번에 여러 개의 파일을 제외하기 위해 glob patterns(`*` and `**`)을 사용할 수도 있다:

```yaml
algolia:
  # Exclude more files from indexing
  files_to_exclude:
    - index.html
    - index.md
    - excluded-file.html
    - _posts/2017-01-20-date-to-forget.md
    - subdirectory/*.html
    - **/*.tmp.html
```

#### nodes_to_index
각 페이지의 내용을 여러 덩어리(chunk)르 분리하는 방법을 정의한다.<br>
default로 `P` 태그가 정의되어 있다. 즉, 컨텐츠에서 <p> 단락을 기준으로 하나의 레코드가 생성된다.<br>
`<blockquote>, <li> 또는 <div class = "paragraph">`와 같은 다른 요소를 색인하려면 다음과 같이 정의한다:
```yaml
algolia:
  # Also index quotes, list items and custom paragraphs
  nodes_to_index: 'p,blockquote,li,div.paragraph'
```

#### settings
Algolia 색인에 특정 설정을 전달할 수 있다.
```yaml
algolia:
  settings:
    highlightPreTag: '<em class="custom_highlight">'
    highlightPostTag: '</em>'
```
여기서 정의 된 설정은 Algolia Dashboard를 통해 수동으로 정의한 설정보다 우선한다.


#### indexing_batch_size
한번의 업데이트 배치 수행으로 처리해야 할 `수`를 정의한다.
default로 1000이다.<br>
각 실행마다 많은 업데이트를 할 경우 이 숫자를 변경할 수 있는데, 인터넷 상태가 좋지 않을 경우라면, 이 숫자를 줄이는게 좋다.<br>
배치 사이즈가 작을 수록 큰 것을 보내기가 거 쉽기 때문이다.
```yaml
algolia:
  # Send fewer records per batch
  indexing_batch_size: 500
```

#### max_record_size
하나의 레코드의 최대 크기 (바이트)를 조정할 수 있다.<br>
고급 옵션으로 Community plan(무료계정)에서는 효과가 없다.<br>
유료 Algolia 플랜을 사용하는 경우 최대 20Kb의 레코드를 푸시 할 수 있다 (무료 커뮤니티 플랜으로 10Kb가 아닌).
```yaml
algolia:
  # Recommended setting for paid plans
  max_record_size: 20000
```

***Note:*** 허용 된 한도를 초과하는 레코드를 푸시하면 푸시가 API에 의해 거부된다. 이로 인해 불완전한 데이터가 업로드 될 수 있다.
