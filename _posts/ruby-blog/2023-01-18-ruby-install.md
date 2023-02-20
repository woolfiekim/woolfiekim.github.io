---
layout: single
title:  "[Ruby] MacOS m1 jekyll minimal-mistakes theme ruby versions local"
categories: 
  - ruby  #카테고리
tag: [ruby, macOS, m1, jekyll, rbenv] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

와 이게 되네?

진짜 ㅋㅋㅋㅋ 어이가 없다...ㅋㅋ

요즘 깃허브 블로그를 만들고 있다. 물론, 이 글도 깃허브 블로그에 올라가고 있다.

지금까지 블로그를 만든 방법은 깃허브에 push를 하고 5~10분 후 확인을 하는 것이였다.

귀찮았지만 내용이 잘 들어가는지 확인만 했기 때문에 큰 어려움은 없었다.

하지만, 이번에 카테고리 기능과 자질구레한 기능을 더 넣고 싶었는데 반영이 되기까지 몇 분의 시간을 기다리는 것이 꽤나 비효율적이라는 것을 깨달았다.

그래서 local로 코드를 저장하면 변경사항이 바로 보일 수 있는 방법을 쓰려고 했다.

이 테마는 Jekyll로 만든 것이기 때문에 ruby를 설치할 수 밖에 없었다. 

몇 일을 소모해서 여러 시도를 해보았다.

그러다가 오늘 생각지도 못한 조력자로 해결을 했다.

Jekyll로 만든 깃헙 블로그를 로컬에서 실행하는 방법을 적은 블로그가 많았다.

꽤나 많은 블로그를 뒤져보았지만 해결하기 힘들었다. 그래서 지금까지 어떤 방법을 써왔고 어떤 오류가 떴고, 어떻게 해결을 했는 지 정리하겠다.

## 현재 내 장비의 스펙

macOS M1 이다.

## 내가 시도해본 방법

1. Ruby설치 후 bundler 패키지 설치 (rbenv 사용) - 실패
   ```shell
    $ brew install libyaml
    $ brew update && brew upgrade ruby-build
   ```
2. 도커를 이용해 Ruby 이미지 사용 - 성공한 듯 싶었으나 - 실패
3. reinstall Xcode - 실패
4. update ventura - 실패
5. uninstall ruby - 실패
6. brew로 ruby, bundler 설치 - 성공

이렇게 적으니까 뭔가 많이 시도를 안해본 거 같지만 아니다 ㅠㅠㅠ 진짜 아니다 ㅠㅠㅠㅠㅠ

온갖 글들을 구글링해서 다 시도를 해보았다.

일단 2번은 왜 안되는지 설명이 안되기 때문에 넘어간다.

### 1. Ruby설치 후 bundler 패키지 설치 (rbenv 사용) - 실패

1번은 [이 블로그](https://unluckyjung.github.io/develop-setting/2021/01/20/Mac-Jekyll-Setting/)를 보고 따라했다.

내 사수인 대리님의 맥에서는 바로 성공했지만 이상하게 내 맥은 되지 않앗다...

```shell
rbenv install {원하는 버전} 
```

이 부분을 하려고 하면 자꾸 이 에러가 떴다.

```shell
❯ rbenv install 2.7.7
To follow progress, use 'tail -f /var/folders/_q/0v9yhl5j1wz64t17fwvzljsw0000gn/T/ruby-build.20230116161632.64650.log' or pass --verbose
Downloading openssl-1.1.1s.tar.gz...
-> https://dqw8nmjcqpjn7.cloudfront.net/c5ac01e760ee6ff0dab61d6b2bbd30146724d063eb322180c6f18a6f74e4b6aa
Installing openssl-1.1.1s...
Installed openssl-1.1.1s to /Users/kim_joohee/.rbenv/versions/2.7.7

Downloading ruby-2.7.7.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.7.tar.bz2
Installing ruby-2.7.7...

WARNING: ruby-2.7.7 is nearing its end of life.
It only receives critical security updates, no bug fixes.

ruby-build: using readline from homebrew
ruby-build: using gmp from homebrew

BUILD FAILED (macOS 13.1 using ruby-build 20221225)

Inspect or clean up the working tree at /var/folders/_q/0v9yhl5j1wz64t17fwvzljsw0000gn/T/ruby-build.20230116161632.64650.w87tlo
Results logged to /var/folders/_q/0v9yhl5j1wz64t17fwvzljsw0000gn/T/ruby-build.20230116161632.64650.log

Last 10 log lines:
compiling objspace_dump.c
compiling date_strftime.c
compiling date_strptime.c
installing default date_core libraries
installing default console libraries
linking shared-object objspace.bundle
linking shared-object io/console.bundle
linking shared-object nkf.bundle
linking shared-object date_core.bundle
make: *** [build-ext] Error 2
```

2.7.7, 3.0.5, 3.1.3, 3.2.0 모든 버전이 다 설치가 안되고 저렇게 떴다.


### 3. reinstall Xcode & 4. update ventura - 실패

이것은 [ruby build discussion 깃허브](https://github.com/rbenv/ruby-build/discussions)페이지에서 보고 시도했는데 실패 

(이 곳에 찾다찾다, 시도하다하다 안되서 설치 도와달라고 썼었다.)

### 5. uninstall ruby - 실패

```shell
❯ ruby --version
ruby 2.6.10p210 (2022-04-12 revision 67958) [universal.arm64e-darwin22]

❯ rbenv uninstall 2.6.10p210
rbenv: version `2.6.10p210' not installed
❯ rbenv uninstall 2.6.1
rbenv: version `2.6.1' not installed
❯ rbenv uninstall 2.6.10
rbenv: version `2.6.10' not installed
```

이상하게 루비를 지웠는데도 계속 2.6.10p210이 남아 있는 것이 확인 되었다. 알고보니 맥 자체에서 루비를 쓰는 거 같았다. 직접 그 폴더까지 가서 지우려고 했지만 강제 삭제조차도 되지 않았다.

### 6. brew로 ruby, bundler 설치 - 성공

이게 바로 선임 말고 다른 대리님이 한 방에 해결한 방법이다!!!

우선 rbenv는 버렸다.

#### 1. 
```shell
history | grep brew
```
- history > 지금까지 친 터미널 히스토리가 나온다.
- grep brew > brew를 검색

#### 2. brew search ruby >  루비를 검색
#### 3. brew install ruby@2.7 > 설치
```shell
You may want to add this to your PATH.

ruby@2.7 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have ruby@2.7 first in your PATH, run:
  echo 'export PATH="/usr/local/opt/ruby@2.7/bin:$PATH"' >> ~/.zshrc
```

#### 4. 이 걸 터미널에 실행
```shell
echo 'export PATH="/usr/local/opt/ruby@2.7/bin:$PATH"'
```

#### 5. 그리고 그 뒤는 
[이 포스트의 2번](https://docs.github.com/ko/enterprise-server@3.6/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
부터 따라했다. 

와... 이게 되네..? 

#### 6. 다음부터 확인시 
이 프로젝트의 경로로 터미널을 열어서 
```shell
bundle exec jekyll serve
```
이것만 하면 local(localhost:4000)로 실시간 확인이 가능하게 되었다. 

만세!!
