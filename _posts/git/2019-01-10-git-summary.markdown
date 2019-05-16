---
layout: post
title:  Git 요약
categories: category git
tags: git
toc: true
original_url:
---


  개념
=========


용어
------
##### VCS(Version Control System) : 버전 관리 시스템

##### CVCS(Centralized Version Control Systems): 중앙집중식 버전 관리 시스템
  CVS, Subversion, Perforce, Bazaar 등

##### DVCS(Distributed Version Control Systems): 분산형 버전 관리 시스템
  BitKeeper 등



차이점
------
##### CVCS
  - 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리
  - 변경된 파일들의 다른 점(Differences)을 관리
  - 새로운 버전은 변경된 파일만을 관리함

##### Git
  - 데이터를 파일 시스템 스냅샷(Snapshots)의 연속(스트림)으로 취급한다.
  - commit하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 관리
  - 어떤 파일이 변경되지 않았다면, 이전 상태의 링크만 저장한다.
  - 히스토리 조회 시 서버 접속 없이 로컬에서 조회한다.
  - 로컬 저장소에 저장되므로 Offline에서도 commit이 가능하다.


파일의 3가지 중요한 상태(States)
------------------
##### 1. Committed
  데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것

##### 2. Modified
  수정한 파일을 아직 로컬 데이터베이스에 commit하지 않은 것

##### 3. Staged
  현재 수정한 파일을 곧 commit할 것이라고 표시한 상태

> **참고:**
> * Tracked 파일: Git의 관리 대상
>     - Unmodified 파일: 수정하지 않음
>     - Modified 파일: 수정했음.
>     - Staged 파일: commit으로 저장소에 기록할 파일의 상태.
> * Untracked 파일: Git의 관리대상이 아님. 방금 생성한 파일. git add를 해야 관리 대상이 됨.
>
**Note:** commit을 하기 위해서는 Modified 파일을 Staged 상태로 만들고, 이 Staged 파일을 commit하게 된다.


파일이 존재하는 영역(Areas)
------------------
##### 1. Working directory(작업 디렉토리)
  - 프로젝트의 특정 버전을 Checkout한 것임
  - 파일 추가/수정 등을 하는 작업 공간
  - Modified 상태의 파일
  - *(물리적으로는 .gitignore에서 명시된 무시되는 파일(폴더)도 있음)*

##### 2. Staging Area
  - 단순한 파일이고 곧 commit할 파일에 대한 정보를 저장.
  - Staged 상태의 파일

##### 3. .git directory(Repository)
  - 이 파일들은 Committed 상태
  - Committed 상태의 파일
  - Unmodified 상태의 파일





--------------------------------------------------------------------------------

 사용하기
=========

저장소 만들기
------------
##### 1. 폴더 생성
    cd /c/user/my_project

##### 2. 초기화(생성된 폴더에서 수행)
    $ git init

  .git 폴더가 생성된다.<br>
  저장소에 필요한 skeleton(뼈대)이 생성되며, 아직은 어떤 파일도 관리되지 않는다.<br>

##### 3. 파일관리
```sh
$ git add *.code
$ git add LICENSE
$ git commit -m 'initial project version'
```

저장소에 파일을 추가하고, commit한다.<br>
이제부터 파일 버전 관리가 시작되었다.<br>



기존 저장소를 Clone 하기
-----------------------
##### git clone 명령
다른 프로젝트에 참여하려거나(Contribute) Git 저장소를 복사하고 싶을 때 사용.<br>
프로젝트 히스토리를 전부 받아온다.<br>
다음은 libgit2 디렉토리를 생성하고, 그 안에 .git 폴더를 생성한다.<br>

    $ git clone https://github.com/libgit2/libgit2

##### 다른 이름으로 clone 하기
다음은 mylibgit 디렉토리를 생성하고, 그 안에 .git 폴더를 생성한다.<br>

    $ git clone https://github.com/libgit2/libgit2 mylibgit



수정하고 저장하기
----------------



파일상태 확인
------------
##### git status 명령
아래에서 신규로 파일 하나를 생성해서 상태를 보면 Untracked 상태임을 확인할 수 있다.<br>
이 파일은 관리대상이 아니며, 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 commit하지 않는다.<br>

    $ touch README
    $ git status
    On branch master
    Your branch is up to date with 'origin/master'.

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            README

    nothing added to commit but untracked files present (use "git add" to track)


아래와 같이 .gitignore 파일에 README 파일을 추가하면, README 파일은 Untracked 상태에서 제외되며<br>
.gitignore 파일이 수정되었으므로, Modified 상태가 되며, commit해야할 파일이 된다.<br>

    $ git add README
    $ git status
    On branch master
    Your branch is up to date with 'origin/master'.

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   .gitignore

    no changes added to commit (use "git add" and/or "git commit -a")



파일 Tracking
------------
##### git add 명령
현재 시점의 파일을 다음 commit에 추가한다는 의미임.<br>
파일을 add 하고, 상태를 보면, commit하기 위한 변경된 목록이 나타난다.<br>
이 상태가 Staged 상태이다.<br>

    $ git add README
    $ git status
    On branch master
    Your branch is up to date with 'origin/master'.

    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            new file:   README

여기서 commit 하게 되면, git add를 실행한 시점의 파일들만 commit되어 저장소에 히스토리로 남게 된다.




메세지에 따른 Status
------------

##### 1. 'Untracked files'
  - Untracked 상태
  - 아직 관리대상이 아님
  - 새로운 파일을 생성했을 경우임
  - git add 할 것인지 판단해야 함.

##### 2. 'Changes not staged for commit'
  - Modified 상태
  - 수정한 파일이 Tracked 상태이지만, 아직 Staged 상태는 아니라는 것.
  - git add 명령을 실행해야 함.

##### 3. 'Changes to be committed'
  - Staged 상태
  - commit 명령을 실행해야 함



Short Status
------------
    $ git status -s

Staged 상태로 추가한 파일 중 새로 생성한 파일 앞에는 `A` 표시가, 수정한 파일 앞에는 `M` 표시가 붙는다.<br>
**Note:** 2자리 문자이며, 공백도 포함되어 있다.

<code>A&nbsp;</code>
  새로 생성한 파일이며, git add했고, commit 대상이다.<br>
<code>M&nbsp;</code>
  기존 파일을 수정했으며, git add했고, commit 대상이다.<br>
<code>&nbsp;M</code>
  기존 파일을 수정했으며, git add 안했음.<br>
<code>MM</code>
  기존 파일을 수정했으며, git add했고, 또 수정함. commit 대상이다.<br>
<code>??</code>
  Untracked 파일

**Note:** `MM`일 경우, commit하면 첫번째 수정만 반영되며, 두번째 수정 건은 한번 더 git add 및 commit을 해야만 반영된다.



Staged와 Unstaged 상태의 변경 내용을 보기
---------------------------------------
##### git diff 명령

    $ git diff

  git diff 명령을 실행하면 수정했지만, 아직 staged 상태가 아닌 파일 내용을 비교한다.<br>
  Unstaged 상태인 것들만 보여준다.<br>
  수정한 파일을 모두 Staging Area에 넣었다면 git diff 명령은 아무것도 출력하지 않는다.<br>

    $ git diff --staged

  staged 상태에 있는 파일 내용 비교한다.<br>
  Staging Area에 넣은 파일의 변경 부분을 보고 싶을 떄.<br>
  --staged 와 --cached 는 같은 옵션임.<br>

**참고:**<br>
파일을 Stage 한 후에 다시 수정해도 git diff 명령을 사용할 수 있다.<br>
이때는 Staged 상태인 것과 Unstaged 상태인 것을 비교한다.



Commit
------
##### git commit 명령
Unstaged 상태의 파일은 commit되지 않는다.<br>
git add 명령으로 추가하지 않은 파일은 commit하지 않는다.<br>

    $ git commit -m "메시지 내용"



Staging Area 생략하고, Commit
----------------------------
##### git commit -a 명령
Tracked 상태의 파일을 자동으로 Staging Area에 넣는다.<br>
그래서, git add 명령을 실행하는 수고를 덜 수 있다.<br>

    $ git commit -a -m 'added new benchmarks'



파일 삭제
--------
##### git rm 명령어
git rm 명령으로 Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제하는 것) commit해야 한다.<br>
워킹 디렉토리에 있는 파일도 삭제하기 때문에 실제로 파일도 지워진다.<br>

Git 명령을 사용하지 않고 단순히 워킹 디렉터리에서 파일을 삭제하고 git status 명령으로 상태를 확인하면 Git은 현재 “Changes not staged for commit” (즉, Unstaged 상태)라고 표시해준다.<br>
그리고 git rm 명령을 실행하면 삭제한 파일은 Staged 상태가 된다.<br>

이미 파일을 수정했거나 Staging Area에(역주 - Git Index라고도 부른다) 추가했다면 -f 옵션을 주어 강제로 삭제해야 한다.

    $ git rm --cached README
하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않게 한다.

    $ git rm log/\*.log
log/ 디렉토리에 있는 .log 파일을 모두 삭제한다.

    $ git rm \*~
이 명령은 ~ 로 끝나는 파일을 모두 삭제한다.



파일 이름 변경
-------------
    $ git mv file_from file_to

아래와 동일

    $ mv README.md README
    $ git rm README.md
    $ git add README



Commit 히스토리 조회하기
--------------------
저장소의 commit 히스토리를 시간순으로 보여준다

    $ git log

##### -p(--patch)
각 commit의 diff 결과를 보여준다. -2는 최근 두 개의 결과만

    $ git log -p -2

##### --stat 옵션
 각 commit의 통계 정보를 조회할 수 있다.<br>
어떤 파일이 수정됐는지, 얼마나 많은 파일이 변경됐는지, 또 얼마나 많은 라인을 추가하거나 삭제했는지 보여준다.

    $ git log --stat

##### --pretty 옵션
oneline 옵션은 각 commit을 한 라인으로 보여준다.<br>
많은 commit을 한 번에 조회할 때 유용하다.<br>
short, full, fuller 옵션도 있는데 이것은 정보를 조금씩 가감해서 보여준다.

    $ git log --pretty=oneline

##### --format 옵션
나만의 포맷으로 결과를 출력하고 싶을 때 사용한다.<br>
Git을 새 버전으로 바꿔도 결과 포맷이 바뀌지 않는다.

    $ git log --pretty=format:"%h - %an, %ar : %s"

##### --graph 옵션
oneline 옵션과 format 옵션은 --graph 옵션과 함께 사용할 때 더 빛난다.<br>
이 명령은 브랜치와 머지 히스토리를 보여주는 아스키 그래프를 출력한다.

    $ git log --pretty=format:"%h %s" --graph

##### --since 나 --until 옵션
시간을 기준으로 조회하는 옵션
지난 2주 동안 만들어진 commit들만 조회하는 명령
"2008-01-15", "2 years 1 day 3 minutes ago" 같이 상대적인 기간을 사용 가능

    $ git log --since=2.weeks

##### -S 옵션
코드에서 추가되거나 제거된 내용 중에 특정 텍스트가 포함되어 있는지를 검색
어떤 함수가 추가되거나 제거된 commit만을 찾아보려면 아래와 같은 명령을 사용

    $ git log -S function_name

##### -- 옵션
파일 경로로 검색하는 옵션
디렉토리나 파일 이름을 사용하여 그 파일이 변경된 log의 결과를 검색

    $ git log -- path1 path2



##### Table 3. git log 조회 범위를 제한하는 옵션


| 옵션                | 내용                               |
| ------------------ | ------------------------------------------------ |
|`-(n)`              | 최근 n 개의 commit만 조회한다.                     |
|`--since, --after`  | 명시한 날짜 이후의 commit만 검색한다.               |
|`--until, --before` | 명시한 날짜 이전의 commit만 조회한다.               |
|`--committer`       | 입력한 커미터의 commit만 보여준다.                  |
|`--grep`            | commit 메시지 안의 텍스트를 검색한다.               |
|`-S`                | commit 변경(추가/삭제) 내용 안의 텍스트를 검색한다.  |



**예제)**
Merge commit을 제외한 순수한 commit 중 gitster가 2008년 10월에 Git 소스코드 저장소에서 테스트 파일을 수정한 commit들

    $ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
       --before="2008-11-01" --no-merges -- t/



되돌리기
-------
한 번 되돌리면 복구할 수 없기에 주의해야 한다.<br>
되돌린 것은 복구할 수 없다.<br>
너무 일찍 commit했거나 어떤 파일을 빼먹었을 때 그리고 commit 메시지를 잘못 적었을 때 한다.<br>
“앗차, 빠진 파일 넣었음”, “이전 commit에서 오타 살짝 고침” 등의 commit을 만들지 않겠다는 말이다.

파일 수정 작업을 하고 Staging Area에 추가한 다음 --amend 옵션을 사용하여 commit을 재작성 할 수 있다.<br>
이전의 commit은 일어나지 않은 일이 되는 것이고 당연히 히스토리에도 남지 않는다.

    $ git commit --amend

**예제)**

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend

여기서 실행한 명령어 3개는 모두 commit 한 개로 기록된다. 두 번째 commit은 첫 번째 commit을 덮어쓴다


##### 파일 상태를 Unstage로 변경하기

    $ git reset HEAD CONTRIBUTING.md

git reset 명령은 매우 위험하다. --hard 옵션과 함께 사용하면 더욱 위험하다.<br>
하지만, 위에서 처럼 옵션 없이 사용하면 워킹 디렉토리의 파일은 건드리지 않는다.


##### Modified 파일 되돌리기
최근 commit된 버전으로(아니면 처음 Clone 했을 때처럼 워킹 디렉토리에 처음 Checkout 한 그 내용으로) 되돌리는 방법

$ git checkout -- CONTRIBUTING.md

git checkout -- [file] 명령은 꽤 위험한 명령이라는 것을 알아야 한다.<br>
원래 파일로 덮어썼기 때문에 수정한 내용은 전부 사라진다.<br>
수정한 내용이 진짜 마음에 들지 않을 때만 사용하자.



리모트 저장소
------------
##### 리모트 저장소 확인하기

    $ git remote
    origin

  'origin'이라는 이름으로 리모트 저장소가 등록되어 있다.

    $ git remote -v
      origin	https://github.com/schacon/ticgit (fetch)
      origin	https://github.com/schacon/ticgit (push)


##### 리모트 저장소 추가하기
기존 워킹 디렉토리에 새 리모트 저장소를 추가

    $ git remote
    origin
    $ git remote add pb https://github.com/paulboone/ticgit
    $ git remote -v
    origin	https://github.com/schacon/ticgit (fetch)
    origin	https://github.com/schacon/ticgit (push)
    pb	https://github.com/paulboone/ticgit (fetch)
    pb	https://github.com/paulboone/ticgit (push)

이제 URL 대신에 pb 라는 이름을 사용할 수 있다.

    $ git fetch pb
    remote: Counting objects: 43, done.
    remote: Compressing objects: 100% (36/36), done.
    remote: Total 43 (delta 10), reused 31 (delta 5)
    Unpacking objects: 100% (43/43), done.
    From https://github.com/paulboone/ticgit
     * [new branch]      master     -> pb/master
     * [new branch]      ticgit     -> pb/ticgit


##### Fetch
리모트 저장소에서 데이터를 가져오려면,

    $ git fetch <remote>

이 명령은 로컬에는 없지만, 리모트 저장소에는 있는 데이터를 모두 가져온다.<br>
그러면 리모트 저장소의 모든 브랜치를 로컬에서 접근할 수 있어서 언제든지 Merge를 하거나 내용을 살펴볼 수 있다.<br>
저장소를 Clone 하면 명령은 자동으로 리모트 저장소를 “origin” 이라는 이름으로 추가한다.<br>
그래서 나중에 git fetch origin 명령을 실행하면 Clone 한 이후에(혹은 마지막으로 가져온 이후에) 수정된 것을 모두 가져온다.<br>
git fetch 명령은 리모트 저장소의 데이터를 모두 로컬로 가져오지만, 자동으로 Merge 하지 않는다.<br>
그래서 당신이 로컬에서 하던 작업을 정리하고 나서 수동으로 Merge 해야 한다.

##### Pull

    $ git pull <remote>

리모트 저장소 브랜치에서 데이터를 가져올 뿐만 아니라 자동으로 로컬 브랜치와 Merge 시킬 수 있다.<br>
Clone 한 서버에서 데이터를 가져오고 그 데이터를 자동으로 현재 작업하는 코드와 Merge 시킨다.


##### Push
git push <리모트 저장소 이름> <브랜치 이름> (Clone 하면 보통 자동으로 origin 이름이 생성된다)

    $ git push origin master

이 명령은 Clone 한 리모트 저장소에 쓰기 권한이 있고, Clone 하고 난 이후 아무도 Upstream 저장소에 Push 하지 않았을 때만 사용할 수 있다.<br>
다시 말해서 Clone 한 사람이 여러 명 있을 때, 다른 사람이 Push 한 후에 Push 하려고 하면 Push 할 수 없다.<br>
먼저 다른 사람이 작업한 것을 가져와서 Merge 한 후에 Push 할 수 있다.


##### 리모트 저장소 살펴보기

    $ git remote show origin
    * remote origin
      Fetch URL: https://github.com/schacon/ticgit
      Push  URL: https://github.com/schacon/ticgit
      HEAD branch: master
      Remote branches:
        master                               tracked
        dev-branch                           tracked
      Local branch configured for 'git pull':
        master merges with remote master
      Local ref configured for 'git push':
        master pushes to master (up to date)

리모트 저장소의 URL과 추적하는 브랜치를 출력<br>
git pull 명령을 실행할 때 master 브랜치와 Merge 할 브랜치가 무엇인지 보여 준다.


##### 리모트 저장소 이름 변경
git remote rename 명령

**예제)** pb 를 paul 로 변경

    $ git remote rename pb paul
    $ git remote
    origin
    paul

**예제)** 리모트 저장소를 삭제

    $ git remote remove paul
    $ git remote
    origin

해당 리모트 저장소에 관련된 추적 브랜치 정보나 모든 설정 내용도 함께 삭제된다.



태그
----
사람들은 보통 릴리즈할 때 사용한다(v1.0, 등등)

    $ git tag

-l, --list 옵션
1.8.5 버전의 태그들만 검색

    $ git tag -l "v1.8.5*"

Git의 tag
  1. Lightweight tag
    브랜치와 비슷한데 브랜치처럼 가리키는 지점을 최신 commit으로 이동시키지 않는다.
    단순히 특정 commit에 대한 포인터일 뿐이다.
  2. Annotated tag
    Git 데이터베이스에 태그를 만든 사람의 이름, 이메일과 태그를 만든 날짜, 그리고 태그 메시지도 저장
    GPG(GNU Privacy Guard)로 서명할 수도 있다
일반적으로 Annotated 태그를 만들어 이 모든 정보를 사용할 수 있도록 하는 것이 좋다.
하지만 임시로 생성하는 태그거나 이러한 정보를 유지할 필요가 없는 경우에는 Lightweight 태그를 사용할 수도 있다.

Annotated 태그

    $ git tag -a v1.4 -m "my version 1.4"
    $ git tag
    v0.1
    v1.3
    v1.4

git show 명령으로 태그 정보와 commit 정보를 모두 확인할 수 있다.

    $ git show v1.4
    tag v1.4
    Tagger: Ben Straub <ben@straub.cc>
    Date:   Sat May 3 20:19:12 2014 -0700

    my version 1.4

    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

        changed the version number


Lightweight 태그
기본적으로 파일에 commit 체크섬을 저장하는 것뿐이다. 다른 정보는 저장하지 않는다.

    $ git tag v1.4-lw
    $ git tag
    v0.1
    v1.3
    v1.4
    v1.4-lw
    v1.5


나중에 태그하기???
예전 commit에 대해서도 태그할 수 있다.
먼저 체크섬을 확인한다.

    $ git log --pretty=oneline

v1.2로 태그하기

    $ git tag -a v1.2 9fceb02

(긴 체크섬을 전부 사용할 필요는 없다).
확인

    $ git tag
    $ git show v1.2


태그 공유하기???
git push 명령은 자동으로 리모트 서버에 태그를 전송하지 않는다.
태그를 만들었으면 서버에 별도로 Push 해야 한다.

    $ git push origin v1.5

한 번에 태그를 여러 개 Push 하고 싶으면 --tags 옵션을 추가하여 git push 명령을 실행한다.

    $ git push origin --tags


태그를 Checkout 하기



Git Alias
---------
    $ git config --global alias.unstage 'reset HEAD --'

아래 두 명령은 동일한 명령이다.

    $ git unstage fileA
    $ git reset HEAD -- fileA

    $ git config --global alias.last 'log -1 HEAD'
    $ git last
