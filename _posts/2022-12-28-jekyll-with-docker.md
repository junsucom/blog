---
layout: post
title: Docker 로 Jekyll 환경 구축하기
author: junsu
date: 2022-12-28T10:51:00.000Z
---
jekyll 블로그를 시작 하려는데.. 환경 구축이 어렵다.

- 루비고 뭐고 컴퓨터에 설치하면 왠지 컴퓨터 느려 지는것 같고..
- 나중에 버전업 하려면 관리 안되고..
- 다른 프로그램이랑 루비 버전 충돌나면, [RVM](https://rvm.io/) 설치 해서 왔다갔다 해야하고..

피곤하다.

그래서 Docker 이미지를 가져와서 시작 해 보려 한다.

물론 Docker 도 많이 어렵지만.. Docker를 알아두면 편하게 쓸곳이 많다.

준비물

- [Docker](https://www.docker.com/)

Start

1. Docker 에서 Jekyll 이미지 내려 받기

```bash
# 최신버전으로!!
docker pull jekyll/jekyll

# or 버전 설정이 필요 하다면
# docker pull jekyll/jekyll:4.2.2
```

1. 새로운 사이트 생성방법 (기존에 만든 사이트를 돌릴려면 3번으로 넘어 가자.)

```bash
export site_name="my-blog" && export MSYS_NO_PATHCONV=1
docker run --rm \
  --volume="$PWD:/srv/jekyll" \
  -it jekyll/jekyll \
  sh -c "chown -R jekyll /usr/gem/ && jekyll new $site_name" \
  && cd $site_name
```

참고 : [https://github.com/envygeeks/jekyll-docker/blob/master/README.md#quick-start-under-linux--git-bash](https://github.com/envygeeks/jekyll-docker/blob/master/README.md#quick-start-under-linux--git-bash)

위의 커멘드 실행시 local 에 “my-blog” 라는 디랙토리와 jekyll 기본 파일 들이 생기고, 해당 폴더로 들어가 있다.

Docker 는 잠깐 생겼다가 사라진다.

1. Server 로 돌려 보자!

```bash
docker run --rm \
  --name my-blog \
  --volume="$PWD:/srv/jekyll:Z" \
  -p 4000:4000 \
  -it jekyll/jekyll \
  jekyll serve
```

뭔가 우당탕탕 거리다가 잘 안되는 듯한 느낌..

```
Auto-regeneration: enabled for '/srv/jekyll'
                    ------------------------------------------------
      Jekyll 4.2.2   Please append `--trace` to the `serve` command 
                     for any additional information or backtrace. 
                    ------------------------------------------------
/usr/gem/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```

webrick LoadError 가 발생!!

[찾아보면](https://github.com/jekyll/jekyll/issues/9066) Ruby 버전 3.0.0 이상에는 webrick 이 기본으로 포함 되어 있지 않아서 따로 설치 해 줘야 한다.

Gemfile 제일 아래에 `gem "webrick"` 를 추가해 준다.

shell 에서 `echo '\ngem "webrick"' >> Gemfile`

그럼 다시 실행

```bash
docker run --rm \
  --name my-blog \
  --volume="$PWD:/srv/jekyll:Z" \
  -p 4000:4000 \
  -it jekyll/jekyll \
  jekyll serve
```

그럼 다시 뭔가 설치가 되면서 서버가 돌아가고 있다는 메시지를 볼 수 있다!!

```
Bundle complete! 8 Gemfile dependencies, 32 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
ruby 3.1.1p18 (2022-02-18 revision 53f5fc4236) [x86_64-linux-musl]
Configuration file: /srv/jekyll/_config.yml
            Source: /srv/jekyll
       Destination: /srv/jekyll/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts
                    done in 2.011 seconds.
 Auto-regeneration: enabled for '/srv/jekyll'
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
[2022-12-28 18:32:16] ERROR `/favicon.ico' not found.
```

잘 실행되면 docker run —rm 대신에 docker run -d 로 실행해서 detach 시킨 다음에 사용하면된다.

```bash
# detach 모드로 실행
docker run -d \
  --name my-blog \
  --volume="$PWD:/srv/jekyll:Z" \
  -p 4000:4000 \
  -it jekyll/jekyll \
  jekyll serve

# 실행
docker start my-blog
# 정지
docker stop my-blog
```