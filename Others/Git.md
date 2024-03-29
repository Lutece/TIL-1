# Git

Git은 버전 관리 시스템(VCS, Version Control System)의 한 종류이다. 

다른 버전관리 시스템으로는 SVN이 있다. Git과 SVN의 차이가 궁금하다면, Quora의 이 질문([What is difference between SVN and Git?](https://www.quora.com/What-is-the-difference-between-SVN-and-Git))에 대한 답변들을 살펴보길 권한다.

Git은 버전/브랜치 별로 프로젝트의 형상을 관리할 수 있기 때문에 1인개발에도 유용하며, 협업시엔 Merge 기능을 이용해 생산성을 올릴 수도 있다. 또한 코드 리뷰에 이용하기도 한다.

개발자라면 형상관리는 필수다.

<br>

- [Staging과 Commit](#git-staging-commit)
- [파일단위 아닌 변경사항 단위로 Staging하기](#git-add-p)
- [Unstaging](#git-restore)
- [브랜치 생성하기](#create-branch)
- [브랜치 이동하기](#move-branch)
- [브랜치 삭제하기](#delete-branch)
- [원격 저장소 브랜치 삭제하기](#delete-origin-branch)
- [브랜치 이름 변경하기](#change-branch-name)
- [Fork](#git-fork)
- [PR](#git-pr)
- [.gitignore](#git-gitignore)
- [credential](#git-credential)

<br>

## <a name="git-staging-commit"></a>Staging과 Commit

형상관리의 핵심인 스테이징과 커밋이다. 

어떤 기능을 구현했는지, 어떤 버그를 잡았는지를 커밋 메세지와 함께 커밋하면, 나중에 해당 커밋으로 복귀할 수도 있다. 타임 스탬라고 생각하면 좋다. 지금까지 한 작업을 스탬프로 찍으면 언제든 지금 찍은 스탬프로 돌아올 수 있다는 이야기이다.

<img src="https://d1lss44hh2trtw.cloudfront.net/assets/editorial/2018/02/kingdom-come-deliverance-how-to-save-your-game-re.jpg" style="zoom:80%;" />

RPG 게임을 하다가 어려운 스테이지를 앞두고 미리 게임을 저장하는 일과도 같다.



Git의 작업 흐름도와 각각 작업에 따른 명령어는 다음과 같다.

![](http://www.mediafire.com/convkey/4508/4t5cxewkb7l4z48zg.jpg)

Local과 Remote는 각각 개발자가 개발과 형상관리를 하고있는 곳을 Local, 형상관리한 파일이 저장된 원격저장소가 Remote에 해당한다. 즉 컴퓨터로 개발을 하고 Github에 형상관리를 저장해둔다면, 컴퓨터가 Local에 해당하고, Github이 Remote에 해당한다는 이야기이다.

Local은 다시 3가지로 구분될 수 있는데, working directory와 staging area, local repo가 있다. 각각의 장소의 역할은 다음과 같다.

| where                 | role                                                         |
| --------------------- | ------------------------------------------------------------ |
| **working directory** | 개발자가 작업을 하고 있는 프로젝트 디렉토리이다.<br /> `git init` 으로 버전관리되고 있는 로컬 작업환경이다. |
| **staging area**      | 형상관리 될 파일들, 즉 커밋**할** 파일들이 머무르는 장소이다. <br />커밋을 위해 커밋할 파일들을 staging area에 올려두고 커밋 메세지와 함께 커밋을 하면, local repo로 이동한다. |
| **local repo**        | 최종적으로 커밋이 보관되는 장소이다. <br />Github, Gitlab같은 원격 저장소(Remote)에 push할 수 있다. |
| **remote repo**       | Github, Gitlab과 같은 Git 호스팅 서비스에서 제공하는 원격 저장소에 해당한다. |

<br>

그럼 직접 파일을 생성하고, 커밋을 해보자.

일단 파일을 생성하고 명령어 `git init` 으로 디렉토리에 Git을 셋팅하였다.

![](http://www.mediafire.com/convkey/b76d/r5n1ssmh8kx0xg2zg.jpg)

<br>

여기서 file_01.java 파일을 커밋하기 위해서 스테이지에 올려보겠다.

![](http://www.mediafire.com/convkey/59ac/0h0th7tcljt5i4hzg.jpg)

`git status` 명령어로 보니 파일들이 서로 다르게 표시된걸 확인할 수 있다. 녹색으로 표시되는 파일이 스테이지에 올라간 파일이며, 이 파일은 커밋 대상이 된다.

만약 커밋하고 싶지 않은 파일이 staging area에 올라갔다면, `git restore --staged <file>` 로 working directory로 복귀시킬 수 있다. 아래 [내용](#git-restore)을 참고바란다.

이제 staging area에 있는 파일을 커밋하고 확인해보겠다.

![](http://www.mediafire.com/convkey/e69e/gusd02c4xmmcvelzg.jpg)

file_01.java 파일을 커밋하고 `git status` 를 확인해보니 file_01.java 파일이 사라진걸 확인할 수 있다. 커밋에 file_01.java 파일의 최신 변경사항이 반영되었기 때문에 Untracked files 목록에서 사라진 것이다. 

Untracked files들은 아직 커밋을 만들지않아서 git에서 추적(tracking)하지 못하는 파일들이다.

만약 다시 file_01.java 파일을 수정한다면 `modified` 목록에 수정된 파일이 보여질 것이다. 

![](http://www.mediafire.com/convkey/f37c/baki1lcse85ue0fzg.jpg)

Tracking 되고 있는 파일중에 변경사항이 발생했으니 새로 staging area에 올려서 커밋하라는 메세지이다.

다시 커밋을 하고, 커밋 목록만 따로 조회해보겠다. 명령어는 `git log` 를 사용한다.

![](http://www.mediafire.com/convkey/6443/7fa1yu4gaofg6grzg.jpg)

작성자와 날짜, 시간, 커밋에 포함된 파일목록 그리고 커밋 ID가 출력된다.

<br>

## <a name="git-add-p"></a>파일단위 아닌 변경사항 단위로 Staging하기

위에서 작성된 스테이징 방식(`git add file`)이 파일 단위라면, 여기서 언급되는 스테이징 방식은 작업의 변경사항 단위로 스테이징하는 방법이다.

하나의 파일 안에서도 변경한 부분이 몇 가지가 될 수 있는데, 이 때 `git add file` 을 사용하면 파일이 통째로 스테이징된다. 그러나 `git add -p` 를 사용하면 **<u>변경사항 단위로만 스테이징</u>** 할 수 있다. 더 편하고 더 분명하게 버전관리 할 수 있다고 생각된다.

이 때의 변경사항 단위를 **Hunk**라고 한다.

test 폴더를 만들어서 실습을 해보았다.

생성한 파일(test.html)에 세 줄의 코드를 추가하고, 명령어 `git add -p` 를 입력하였다.

![](https://lh3.googleusercontent.com/nOsWLkb4d1cDxvd62YTU_k71_OJRwiiXYgItZ_xQJPXHMPGHUMtXT1TaSMN0DGK_YfnzfPuqLk9n8hCdH9UBY4knvRbldaOh2KiPcg8vCJQH7KzuODPwfKojcR3OABe83KdW9ralyr0EWLmapKfAxib8Y-qRTQxz--VqdNDuCgJpMPf-hD2kCBwzK5YITWEp80LEijQpbUhPqpWZmploBXtcA1tGqebeoFSQwuqkOnyHIh4xvn47BPJWJqIMS0BcOxFFhuP1OrQyvOkqL2FHXRS0gFKMs0of7D7aU00mKPVW8ABPb46TOMzZuHVfNRG2hmndTWzIsCZStEc3z3UTAIAdh2QzzZ4TbEmzOGXRZWKHKwHfkg6Qzsjktn-M7_RGtonRDKAuey_hQi0pW55Pxjq8gv10vFws-oKzEJrC2aA4mebdx3Rdoe6JzZ5sWJSSY50Jh0J2-mYuB4c7qw9ruGL1gRuO-vwrpG3a4QHn7-KqsP9nUaFCYYf1Ejcx_GrJYpuW5smq4vUYssQxsP9zb9pqAteUf5_ob4oCsQgGFhLulzVfFlpmsb1K3AaGKTI5ErZomdsSfhnT3Yqju8mGgn6bVItQT59ACegt-CHeB8Pa6uS0EgdsZXwe-72IgnZYHfQMplcAZ1Kdj-lwckGYItDMaPRRmdBk9iuxRR0PNalivmv0SzYYvfSTJJEolEMAb8jCfbstMaUjGL4uI2dC9KofzqvOpB3S6CFt4rx6Fl9Hkh3P5A=w720-h489-no)

여기서 `s` 는 split을 의미한다. 세 줄을 한 꺼번에 스테이징하기 보다 더 세분화해서 스테이징하기 위해 split을 하는거다. split을 한 결과는 다음과 같다.

![](https://lh3.googleusercontent.com/KQzWbcfyFdb7qvH4rUYjmL8s2NNSrph-oPVYIOUiVmkBP1ERRqMPy53w0X1AXzbtcIt79H-uiNHftCrcf6z79Zhi9RqN0jb0Qa8w1rR2wQ76Is9HFMrl0mELyBz-AV2wVPk0yFSvOLuPV5HUyG4-HJmKqLq73yMW6s3hY959DrpkgZCC-BAaW5rU0fVvVWZZcgxBSK-ItoUYTQ_-m8Y1lEKE3q7XfFufwik6sqz2qeGRwg6cwXUVhcmHh6HpqlLwJEqsq4__KcuXgJrCNiev6cm6b45Dz2BBCQFx9sxl4es1vyoerosQEqbn2HDqop03OU_umWdFiU4p_H-hYqKbggr8i_QnMLIgLeKZyvoSf3SN0_fiwMRm79YbYzoimXbc8IXZadfBvRrH_8azJxyQ0UVo9aiVy6dKlOe8YOLUR66Ks7arDS-NrHur-65kbxf7FnG3pGGAa_f8YqYJahGkGjqL88aLHOKWo_OPYUCCJ_H7aAU3P_xYE27AmfTYyhp7Ef5QlGyWxtUr151Uz5paLhRgFokveUFrxIBqbcVW9U5DQsdZwVLlhh7JspUXfsJddcfJUh2fVH25BNizW1D4nogEvd7GU6ayUWn8nE7dTXKYzLodJTdB8glSOIxlKE3kU5yml0UOpolWw-zLKKnwT0z6PX6y2hJwpLtcQTwsB0odO5QDov9fnSiI9PxDo4oFou3rNBme1nkLnrxXQ89rquVp1iiX49M8MO-lVLbnieGVcBRDog=w720-h572-no)

하나의 hunk가 3개의 hunk로 쪼개져서 각각 스테이징 할 수 있었다. 세 줄의 코드중 마지막 줄 코드만 스테이징 하지 않기로 했다.

![](https://lh3.googleusercontent.com/ITFznygeA3KQNMX61PfMxOAAUu5pNO2vnwE_J83_oWu2nxoS80Aqm3JmQbaDCMNhoty_NFfGt88z2R0R05CFBbRsxfG9yLZP-GtvfltkE41BtIaMSOEsBLmrYIrW5Z2HGGcdHyqq2HtL-j53X7Kv7jeTiClJ3-L88oN4Hg17A6iXRSbGhjtm8Dv3CrZg_qOQL1iqadjribOmDlrogB4Tg_UPvQwTciNK29ITjfh5sltU1IkO-GFVmSV8PwuDLpdRmGwJQ4BGQzjGMPhDxnMNOoJLoNbNOqQd0L1QvBGP6xg83tX-PgtADpa0T3T-hzv0HZwEp6oCuyxGjs0QQcqR-fTbCo2wnlhWe1Lyg6JydTMzIi5DJvyrvkuqQ5qERSPzNKhULlLaHMxwUMCsXqPDOZhDm4PS7WSb8sM6tZsY3s_dc8Hvyro884QasSWEdO9HUj1-saiENzDuoiDsKL3npc9kGP2j8-ntzL9DGm4BuWNPAtumN7VPFqrxPpVSnaD_vqD8X9vcs-mAee9zESeDBKmAQ_vKdIApQOGzxTYF_YIti31AkuC9YQH0v74RAoDcU0-J86D4rYyt5DXu54LNA42lTfqUAXLsN1DnBCsqEsTX2wNGlihPELVDexwHckf-qk1iTfhWLAoAmQ_9NrNAmbyjwRDrzJKn4gEE5HfKABQ6TdsU8jRn-fvZrk08hFfGxm7UEGcMCRVorVVndo2X5VkWkv2ovtHrrqxKEIGN3Ki69RYnOg=w720-h337-no)

다시 `git add -p` 을 입력해보니 아까 스테이징 하지 않은 한 줄의 코드만 스테이지 여부를 묻고, 나머지 코드는 스테이징 되었음을 확인할 수 있었다. 

이처럼 `git add -p` 를 사용하면, <mark><b>변경사항 단위</b>로만 스테이징이 가능하다.</mark>

`git add -p` 에서 응답할 수 있는 명령어는 `?` 을 입력하면 볼 수 있는데, 다음과 같다.

![](https://lh3.googleusercontent.com/EZaWDiERwWHC0b9Y26t-iuMtBSjLtEmADt0p_a1950PDQwH17gLUsKtwQ86NrBgg7MZdhcLzMPn62pgebHj1c6zCGpJ2-t2Xn4Oyn8z_7HLCu_COp-bgiMWljOA1MfEetjide4LHMV8TO7f-NSzyT1GVvedWRjbCzSP2rgB80U99QUgcWvKThEYQsRvnR-OgnwWm0mKrdDfoaKSoEGIAPKfKjO0JSnoZshpz9-XWneVad0QzIUJIuvxWp71El8mweBQpk8jaf5YZVtYaVNCm-JmIUehi7V7Oaxpe0qAtHB55TSpkQ_UuQ9D5aqsWSTLhHrwtR42EptGdQ5j7jdf7HtYqAKK5bGB4bBlFhOcMiWhi6wAVkEj16-PZP71szp7bKDLxZgGBTDB75KGGQuK0T7L7sUGXqLz8KRzI5z5MtlVFNx_2w-JuDMCMcKBwc6ko_17Aj_bqjLWesfODFUBwyVX9g7PLfLngt4HUYhjYk67mw0o5GvP9o0_FFFX41Ov1yg3g3JEU6uxGGYcGkb6J0ZYQIzsSQ6VY6Ozw4AT2ylVYpcN9vB9b_mCEJGdIP-h960akfLQb1LIDfh3WvamC3HESsxB3uB8IDTw-XgzaIyiM_IV1m2YcfEjHgareU9GLdDqFLEJ5poysw9EmRZFNbL61vfyJsx_idxBfY8llBhecQdk8lmvppfNLEbKLTjKyD3nHJP58B3qZD4Ub6Nvf0N2ZlkXyxo0mOz2l0KWfx1CUdIkaBg=w1440-h366-no)

주로 쓰이는 명령어는 `y`, `n`, `q` 이다.

`y` : 해당 hunk를 스테이징한다.

`n` : 스테이징하지않고 건너뛴다.

`q` : add 과정을 종료한다.

<br>

## <a name="git-restore"></a>Unstaging(staging area➡️working directory)

staging area에서 working directory로 돌리는 방법에 관한 방법이다.

`git add .` 명령어로 작업한 파일 전체를 staging area에 올려두었는데, 이 중 커밋되면 안되는 파일을 발견했다. 

이 땐 `git restore --staged <file>` 명령어를 통해 staging된 파일을 working directory로 복귀시킬 수 있다.

<br>

file_02.java 파일을 staging area에 올려두었다가 다시 untracking file로 바꾼 상태이다.

![](http://www.mediafire.com/convkey/54a7/pln9p0sn3f2bed1zg.jpg)

<br>

## <a name="create-branch"></a>브랜치 생성하기

```git
git branch unit-Test
```

`unit-Test` 라는 이름의 브랜치를 생성할 수 있다.

<br>

## <a name="show-branches"></a>브랜치 확인하기

로컬 저장소의 모든 브랜치를 확인하고 싶다면,

```git
git branch
```

![](http://www.mediafire.com/convkey/113b/6naabezpnga2arlzg.jpg)

원격 저장소 브랜치까지 모두 확인하고 싶다면,

```git
git branch -a
```

![](http://www.mediafire.com/convkey/d579/to1uf1203n1i2d4zg.jpg)

<br>

## <a name="move-branch"></a>브랜치 이동하기

```git
git checkout master
```

![](http://www.mediafire.com/convkey/53ce/pgdo7ue0e03jbl4zg.jpg)

<br>

## <a name="delete-branch"></a>브랜치 삭제하기

```git
git branch -D unit-Test
```

<br>

## <a name="delete-origin-branch"></a>원격 저장소 브랜치 삭제하기

```git
git push origin --delete unit-Test
```

원격 저장소에서 `unit-Test` 브랜치가 삭제된다.

출처 : [정광섭 - git 원격지 브랜치 삭제](https://www.lesstif.com/pages/viewpage.action?pageId=20776547)

<br>

## <a name="change-branch-name"></a>브랜치 이름 변경하기

만약 이름을 변경하길 원하는 브랜치가 선택되어 있다면,

```git
git branch -m new-name
```

현재 선택된 브랜치가 아닌 다른 브랜치의 이름을 변경하고자 한다면,

```git
git branch -m old-name new-name
```

![](http://www.mediafire.com/convkey/e9d9/to6tbiwhaa0go37zg.jpg)

원격 저장소에서 적용하기

```git
git push origin :old-name
```

변경된 새 브랜치 원격 저장소에 적용하기

```git
git push --set-upstream origin new-name
```

![](http://www.mediafire.com/convkey/5d11/rpk9z3iwnxxto6czg.jpg)

출처 : [W3docs - How to Rename Git local and remote branches](https://www.w3docs.com/snippets/git/how-to-rename-git-local-and-remote-branches.html)

<br>

## <a name="git-fork"></a>Fork

Fork는 다른 사람의 저장소를 내 저장소를 가져오는 기능이다. 정확히는 내가 Fork한 시점까지의 버전/커밋만 가져올 수 있다. 이후에 원래의 저장소, Upstream 저장소에 변경사항이 발생해도 내 저장소에 자동으로 반영되지는 않는다. 이 때엔 수동으로 Upstream 저장소의 변경사항을 동기화(`fetch`)하고 변경사항을 가져와야 한다. 자세한 방법은 [여기](https://youngjinmo.github.io/2019/09/git-sync-forked-repo/)를 참고바란다.

### 1. Fork할 저장소에서 Fork버튼을 클릭해서 가져온다.

![](http://www.mediafire.com/convkey/a78d/ozcxdz0mxotz4eozg.jpg)

### 2. Fork 저장소 확인

![](http://www.mediafire.com/convkey/bfca/349f4f3e0xz9iu3zg.jpg)

![](http://www.mediafire.com/convkey/68dc/3wsmvml6eq37oqezg.jpg)

### Clone과 Fork의 차이

Git 호스팅 서비스(Github, Gitlab)는 원격 저장소이다. 이런 원격 저장소에서 내 컴퓨터로 즉 로컬로 가져오는게 Clone이고, 원격저장소 내에서 내 이름의 저장소로 가져오는 행위는 Fork이다. 이 부분을 헷갈리지 않을 필요가 있다.

<br>

## <a name="git-pr"></a>PR

깃헙에서 PR이라는 단어를 자주봤을 것이다. PR은 Pull Request의 약어이다. 말 그대로 내가 작성한 커밋을 Pull 해달라고 요청하는 것이다. PR이 사용되는 경우는 아래의 경우에 해당된다.

프로젝트에 사용하는 공용 저장소를 [Fork](#git-fork)해서 자신의 저장소로 가져왔다. 여기에서 기능을 추가해서 커밋을 했는데 이게 프로젝트에 반영되려면 원격 저장소(Upstream)에 PR를 하는 것이다.

오픈소스에 기여(Contribution)하는 방법도 이와 같다. 오픈소스를 사용하다가 버그가 발견되었을 때, 해당 버그를 해결하기 위해 Fork한 저장소에서 버그를 해결한 코드를 작성후, PR을 하고, 원작자가 이를 받아들여서 오픈소스에 반영되면 내 이름이 컨트리뷰터에 오르게 된다.

PR하는 방법을 살펴보자.

### 1. 내 저장소에서 PR 요청

Github에 접속하면 Pull Request가 활성화되어있는 걸 확인할 수 있다. 

그 왼쪽의 메세지를 읽어보면, *'이 브랜치에 10개의 커밋이 [Upstream Owner:branch]를 바라보고 있다'*고 알려주고 있다. 이는 Upstream에 PR을 보낼 수 있는 10개의 커밋이 있다는 이야기이다.

![](http://www.mediafire.com/convkey/6ce7/3swf98prpdu4fu7zg.jpg)

### 2. Upstream 저장소의 브랜치와 내 저장소의 브랜치를 맞추고 PR

Upstream 저장소의 어떤 브랜치에 내 커밋을 병합(merge)할 것인지, 또 내가 보낼 커밋은 어떤 브랜치의 커밋인지를 맞춘뒤 Create pull request 버튼을 클릭하면 Merge 커밋메세지를 작성할 수 있다. Upstream의 오너가 이를 수용해서 merge하면 내가 작성한 커밋이 한꺼번에 Upstream의 브랜치에 반영된다.

![](http://www.mediafire.com/convkey/6bc0/1vj19nj4yncrlq3zg.jpg)

<br>

## <a name="git-gitignore"></a>.gitignore

git으로 버전관리를 하다보면, 버전관리할 필요가 없는 불필요한 파일이 발생하기도 한다. MacOS에서는  ".DS_Store"라는 운영체제 관련된 파일이 생성되는데 이게 저장소로 push되면, 다른 운영체제의 contributor가 작업시에 conflict가 발생될 수 있다. 

따라서 이런 파일들은 `.gitignore` 라는 파일로 버전관리목록에서 제외시켜야 한다.

### 1. `vi .gitignore`

vi에디터로 `.gitignore` 파일을 생성한다. 그리고 버전관리 하지않을 파일의 목록 또는 파일의 디렉토리명을 입력해두면, `git status`에서 확인되지 않는다.

![](http://www.mediafire.com/convkey/5bf7/95y9yh7c0yhenfqzg.jpg)

<br>

### 2. 버전관리 제외할 파일 형식 추가

![](http://www.mediafire.com/convkey/393d/uwzge9q8xfrx84izg.jpg)

`.gitignore` 에서 `.DS_Store` 파일들을 버전관리 목록에서 제외하라고 작성해두었다.

그리고 git 명령어 `git status`로 확인해보자.

![](http://www.mediafire.com/convkey/1712/ut4pr6bajecoodtzg.jpg)

더 이상 `.DS_Store` 파일이 보이지 않는 것을 확인할 수 있다.

<br>

### 3. 이미 원격저장소에 들어간 파일을 ignore하고 싶을 때

이미 원격 저장소에 들어간 파일을 `ignore`해봐야 반영되지 않을 것이다. 이땐 다음의 절차대로 하면 원격 저장소로 넘어간 파일도 ignore할 수 있게된다.

1. `ignore`하고자 하는 파일을 삭제.
2. 새로운 커밋 생성
3. `.gitignore`에 파일명 또는 포맷을 작성해서 ignore.
4. 명령어 `git status` 통해 확인

<br>

## <a name="git-cedential"></a>Github Credential 저장

Github Credential이란 Github의 계정정보를 말한다. 

저장소에 push/pull 하거나 private 저장소를 clone하기 위해서는 해당 저장소를 이용할 수 있는 권한이 필요한데 이 때 credential에 계정 정보를 저장해두면 저장소 이용시마다 로그인할 필요가 없다.

터미널 명령어는 다음과 같다.

`git config credential.helper store`

`git push https://github.com/repo.git `

`Username for 'https://github.com' : your github email`

`Password for 'https://your github email' : your github password`

출처 : https://git-scm.com/docs/git-credential-store

<br>

<br>