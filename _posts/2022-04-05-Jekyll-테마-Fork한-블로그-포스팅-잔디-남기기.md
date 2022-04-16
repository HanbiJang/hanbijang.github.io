---
layout: post
title: Jekyll 테마 Fork한 블로그 잔디 심기
author: HanbiJang

categories: [Jekyll]
tags: [Tutorial, Jekyll]
render_with_liquid: false
---


# Jekyll 테마 fork한 레포지토리 잔디 심기 + 오류 해결

나처럼 깃헙 블로그 테마를 원본 레포에서 Fork 하여 사용한 사용자라면 이상할 것이다
커밋 푸시를 해도... 깃허브 메인에 잔디가 표시가 안 된다 😅 (저는 Chirpy테마 fork하여 사용 중)

## 이 방법은 왜 되나요

`bare clone` 과 `mirror push` 로 인해 자신이 (fork하여) 작업하던 레포지토리의 모든 푸시 내역 등의 작업 내역들까지 앞으로 만들 새 레포지토리에 복사가 되는 것이다.

# 방법

**새 레포지토리 만들기 = A**

**내가 Fork 해서 작업했는데 잔디가 안 심어지는 기존의 레포지토리 = B**

(주의)

>A 레포와 B 레포 둘을 헷갈려서 진행하면 끔찍한 결과가 기다린다. 주의하자....
{: .prompt-danger }

cd 명령어로 A,B를 제외한 아무 폴더 경로에 가서

```cpp
git clone --bare https://github.com/HanbiJang/hanbijang.github.io.git
```

와 같이

```cpp
git clone --bare B레포지토리주소
```

입력해주고

🔥 **cd 명령어로 B 레포지토리 폴더로 가서**  (없으면 클론해서 만들어야 한다)

```cpp
git push --mirror A레포지토리주소
```

위 방법으로 하면 됨 (정말 간단함!)

그리고 나서

🔥 **A레포지토리**의 **Settings** 으로 가서 Branch를 **내가 작업했던 브랜치로** 설정한다! (중요)

- Chirpy 테마의 경우, 브랜치를 하나 더 만드는 등 다른 짓을 하지 않았다면 **master** 일 것이다

---

## Push가 안 되는 오류

그러나!! push 단계에서 뜬금 없이 `workflow scope` 에러 발생하는 경우가 있음

- 최근에 깃허브 토큰을 재발급 받았거나 하는 경우가 원인인듯하지만 누구에게나 발생 가능...
- 내 경우는 master등을 제외한 gh pages 브랜치만 복사되는 문제가 있었다

```cpp
$ git push --mirror https://github.com/HanbiJang/new_repo.git
Enumerating objects: 6437, done.
Counting objects: 100% (6437/6437), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2773/2773), done.
Writing objects: 100% (6350/6350), 5.22 MiB | 4.35 MiB/s, done.
Total 6350 (delta 3680), reused 6078 (delta 3467), pack-reused 0
remote: Resolving deltas: 100% (3680/3680), completed with 54 local objects.
To https://github.com/HanbiJang/new_repo.git
 ! [remote rejected] main (refusing to delete the current branch: refs/heads/main)
 ! [remote rejected] master -> master (refusing to allow an OAuth App to create or update workflow `.github/workflows/pages-deploy.yml` without `workflow` scope)
 ! [remote rejected] origin/HEAD -> origin/HEAD (refusing to allow an OAuth App to create or update workflow `.github/workflows/pages-deploy.yml` without `workflow` scope)
 ! [remote rejected] origin/code-style-review -> origin/code-style-review (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] origin/docs -> origin/docs (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] origin/master -> origin/master (refusing to allow an OAuth App to create or update workflow `.github/workflows/pages-deploy.yml` without `workflow` scope)
 ! [remote rejected] origin/production -> origin/production (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.0 -> v2.0 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.1 -> v2.1 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.2 -> v2.2 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.2.1 -> v2.2.1 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.3 -> v2.3 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.4 -> v2.4 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.4.1 -> v2.4.1 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.4.2 -> v2.4.2 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.5 -> v2.5 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.5.1 -> v2.5.1 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.6.0 -> v2.6.0 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.6.1 -> v2.6.1 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.6.2 -> v2.6.2 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
 ! [remote rejected] v2.7.0 -> v2.7.0 (refusing to allow an OAuth App to create or update workflow `.github/workflows/ci.yml` without `workflow` scope)
error: failed to push some refs to 'https://github.com/HanbiJang/new_repo.git'
```

이런 끔찍한 오류가 좌르륵 나온다면


자격 증명 관리자 - window 자격증명에서

`git:[https://github.com](https://github.com/)`

이 녀석을 지우고 다시 시도하면

깃허브 로그인 창이 뜨면서 아래와 같이 해결된다

```cpp
$ git push --mirror https://github.com/HanbiJang/new_repo.git
Enumerating objects: 6437, done.
Counting objects: 100% (6437/6437), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2773/2773), done.
Writing objects: 100% (6350/6350), 5.22 MiB | 4.21 MiB/s, done.
Total 6350 (delta 3679), reused 6079 (delta 3467), pack-reused 0
remote: Resolving deltas: 100% (3679/3679), completed with 55 local objects.
To https://github.com/HanbiJang/new_repo.git
 * [new branch]      master -> master
 * [new reference]   origin/HEAD -> origin/HEAD
 * [new reference]   origin/code-style-review -> origin/code-style-review
 * [new reference]   origin/docs -> origin/docs
 * [new reference]   origin/master -> origin/master
 * [new reference]   origin/production -> origin/production
 * [new tag]         v2.0 -> v2.0
 * [new tag]         v2.1 -> v2.1
 * [new tag]         v2.2 -> v2.2
 * [new tag]         v2.2.1 -> v2.2.1
 * [new tag]         v2.3 -> v2.3
 * [new tag]         v2.4 -> v2.4
 * [new tag]         v2.4.1 -> v2.4.1
 * [new tag]         v2.4.2 -> v2.4.2
 * [new tag]         v2.5 -> v2.5
 * [new tag]         v2.5.1 -> v2.5.1
 * [new tag]         v2.6.0 -> v2.6.0
 * [new tag]         v2.6.1 -> v2.6.1
 * [new tag]         v2.6.2 -> v2.6.2
 * [new tag]         v2.7.0 -> v2.7.0
 * [new tag]         v2.7.1 -> v2.7.1
 * [new tag]         v2.7.2 -> v2.7.2
 * [new tag]         v3.0.0 -> v3.0.0
 * [new tag]         v3.0.1 -> v3.0.1
 * [new tag]         v3.0.2 -> v3.0.2
 * [new tag]         v3.0.3 -> v3.0.3
 * [new tag]         v3.1.0 -> v3.1.0
 * [new tag]         v3.1.1 -> v3.1.1
 * [new tag]         v3.2.0 -> v3.2.0
 * [new tag]         v3.2.1 -> v3.2.1
 * [new tag]         v3.2.2 -> v3.2.2
 * [new tag]         v3.3.0 -> v3.3.0
 * [new tag]         v3.3.1 -> v3.3.1
 * [new tag]         v3.3.2 -> v3.3.2
 * [new tag]         v4.0.0 -> v4.0.0
 * [new tag]         v4.0.1 -> v4.0.1
 * [new tag]         v4.0.2 -> v4.0.2
 * [new tag]         v4.1.0 -> v4.1.0
 * [new tag]         v4.1.1 -> v4.1.1
 * [new tag]         v4.2.0 -> v4.2.0
 * [new tag]         v4.2.1 -> v4.2.1
 * [new tag]         v4.3.0 -> v4.3.0
 * [new tag]         v4.3.1 -> v4.3.1
 * [new tag]         v4.3.2 -> v4.3.2
 * [new tag]         v4.3.3 -> v4.3.3
 * [new tag]         v4.3.4 -> v4.3.4
 * [new tag]         v5.0.0 -> v5.0.0
 * [new tag]         v5.0.1 -> v5.0.1
 * [new tag]         v5.0.2 -> v5.0.2
 * [new tag]         v5.1.0 -> v5.1.0
 ! [remote rejected] main (refusing to delete the current branch: refs/heads/main)
error: failed to push some refs to 'https://github.com/HanbiJang/new_repo.git'
```

아직도 한 줄 오류가 뜨긴 하지만 말끔히 master브랜치의 작업내역, 커밋 내역이 완벽히 복사가 되어 새브랜치 A에 만들어졌고, 잔디도 추가되었다!

~~나는 귀찮아서 그냥 원래 작업하던 깃허브 레포지토리에서 작업할 것이므로, 여전히 잔디 표시가 안 될 것이다.
이렇게 가끔 가다 `bare clone` , `mirror push` 를 해주면 그만이니 상관 없다.~~

⇒ 수정! 전에 만들어둔 Fork된 레포지토리 명을 `Username.github.io` 에서 다른 것으로 바꾸고,
해당 잔디 심기용 레포의 이름을 `Username.github.io`로 만든다!
몇 분 후에 다시 깃허브 홈페이지가 만들어집니다.

만약 Chirpy테마 블로그에서 문제가 있다면 다음 포스트에서 문제를 해결하면 됩니다 :)
[깃허브 블로그 오류 해결](https://hanbijang.github.io/posts/Jekyll-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)

---

### 참고한 사이트들

[잔디심는 법 참조](https://velog.io/@whoyoung90/fork-%ED%95%B4%EC%98%A8-repository-%EC%9E%94%EB%94%94-%EC%8B%AC%EB%8A%94-%EB%B0%A9%EB%B2%95)

[workflow scope 오류 해결 참조](https://director-joe.kr/91)
