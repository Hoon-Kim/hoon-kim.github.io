---
layout: post
title:  Troubleshooting-Jekyll
categories: category jekyll
tags: jekyll, troubleshooting, PAGES_REPO_NWO, CP949
toc: true
original_url:
---

## No repo name found. Specify using PAGES_REPO_NWO environment variables

### 원인
jekyll-theme-primer 테마를 설치하면서 부터 발생했음.

### 현상
```js
F:/Jekyll/blog/hoon-kim.gjthub.io>jekyll serve
Configuration file: F:/Jekyll/blog/hoon-kim.gjthub.io/_config.yml
            Source: F:/Jekyll/blog/hoon-kim.gjthub.io
       Destination: F:/Jekyll/blog/hoon-kim.gjthub.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
fatal: not a git repository (or any of the parent directories): .git
  Liquid Exception: No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository. in /_layouts/default.html
             ERROR: YOUR SITE COULD NOT BE BUILT:
                    ------------------------------------
                    No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository.
```
### 해결방안

_config.yml 파일에서 아래 추가함.
```sh
repository: "username/repo-name"
```
### 참고사이트
[https://mmistakes.github.io/minimal-mistakes/docs/configuration/#site-base-url](https://mmistakes.github.io/minimal-mistakes/docs/configuration/#site-base-url){:target="_blank"}






## Invalid CP949 character "\xE2" on line 5
jekyll serve를 하는데 오류 발생함.

### 원인
jekyll-theme-primer 테마를 설치하면서 부터 발생했음.<br>
(외부 테마를 설치하기 전에는 발생하지 않았음)

### 현상
```js
F:/Jekyll/blog/hoon-kim.gjthub.io>jekyll serve
Configuration file: F:/Jekyll/blog/hoon-kim.gjthub.io/_config.yml
            Source: F:/Jekyll/blog/hoon-kim.gjthub.io
       Destination: F:/Jekyll/blog/hoon-kim.gjthub.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
fatal: not a git repository (or any of the parent directories): .git
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
  Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/style.scss':
                    Invalid CP949 character "\xE2" on line 5
jekyll 3.8.5 | Error:  Invalid CP949 character "\xE2" on line 5
```

### 해결방안
```sh
chcp 65001
```
### 참고
[https://jekyllrb-ko.github.io/docs/windows/](https://jekyllrb-ko.github.io/docs/windows/){:target="_blank"}
