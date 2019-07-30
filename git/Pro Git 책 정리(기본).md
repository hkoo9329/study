# Pro Git 책 정리 (기본)



# 시작하기

이 장에서 설명하는 것은 Git을 처음 접하는 사람에게 필요한 내용이다. 먼저 버전 관리 도구에 대한 이해와 Git을 설치하는 방법을 설명하고 마지막으로 Git 서버를 설정하고 사용하는 방법을 설명한다.



## 버전 관리란?

버전 관리는 무엇이고 우리는 왜 이것을 알아야 할까? 버전 관리 시스템은 파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시  꺼내올 수 있는 시스템이다. 이 책에서는 버전 관리하는 에제로 소프트웨어 소스 코드만 보여주지만, 실제로 거의 모든 컴퓨터 파일의 버전을 관리할 수 있다.

버전 관리 시스템 (VCS -  Version Control System) 을 사용하면 

- 각 파일을 이전 상태로 되돌릴 수 있다. 
- 프로젝트를 통째로 이전 상태로 되돌릴 수 있다.
- 시간에 따라 수정 내용을 비교해 볼 수 있다.
- 누가 문제를 일으켰는지도 추적할 수 있다.
- 누가 언제 만들어낸 이슈인지도 알 수 있다.
- 파일을 잃어버리거나 잘못 고쳤을 때도 쉽게 복구할 수 있다.





## 로컬 버전 관리

많은 사람은 버전을 관리하기 위해 디렉토리로 파일을 복사하는 방법을 쓴다. 이 방법은 간단하므로 자주 사용한다. 그렇지만 뭔가 잘못되기 쉽다. 작업하던 디렉토리를 지워버리거나, 실수로 파일을 잘못 고칠 수도 있고, 잘못 복사할 수도 있다.

이런 이유로 프로그래머들은 오래전에 로컬 VCS라는 걸 만들었다. 이 VCS는 아주 간단한 데이터베이스를 사용해서 파일의 변경 정보를 관리했다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FSlVQs%2FbtqwTx1DXOs%2FKnBwrwokPGh6VErkdWIb61%2Fimg.jpg)

>Figure 1. 로컬 버전 관리
>
>많이 스는 VCS 도구 중에 RCS (Revision Control System) 라고 부르는 것이 있는데 오늘날까지도 아직 많은 회사가 사용하고 있다. Mac OS X 운영체제에서도 개발 도구를 설치하면 RCS가 함께 설치된다. RCS는 기본적으로 Patch Set (파일에서 변경되는 부분)을 관리한다. 이 Patch Set은 특별한 형식의 파일로 저장한다. 그리고 일련의 Patch Set을 적용해서 모든 파일을 특정 시점으로 되돌릴 수 있다.



## 중앙 집중식 버전 관리 (CVCS)

프로젝트를 진행하다 보면 다른 개발자와 함께 작업해야 하는 경우가 많다. 이럴 때 생기는 문제를 해결하기 위해 CVCS (중앙집중식 VCS)가 개발됐다. CVS, Subversion, Perforce 같은 시스템은 파일을 관리하는 서버가 별도로 있고 클라이언트가 중앙 서버에서 파일을 받아서 사용(checkout)한다. 수년 동안 이러한 시스템들이 사랑 받았다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FGMRGm%2FbtqwVTvIKIm%2FbC5dMbwYvM5dk6kIJxXBk0%2Fimg.jpg)

>CVCS 환경은 로컬 VCS에 비해 장점이 많다. 모두 누가 무엇을 하고 있는지 알 수 있다. 관리자는 누가 무엇을 할지 꼼꼼하게 관리할 수 있다. 모든 클라이언트의 로컬 데이터베이스를 관리하는 것보다 VCS 하나를 관리하기가 훨씬 쉽다.
>
>그러나 이 CVCS 환경은 몇 가지 치명적인 결점이 있다. 가장 대표적인 것이 중앙 서버에 발생한 문제다. 만약 서버가 한 시간 동안 다운되면 그동안 아무도 다른 사람과 협업할 수 없고, 사람들이 하는 일을 백업할 방법도 없다. 그리고 중앙 데이터베이스가 있는 하드디스크에 문제가 생기면 프로젝트의 모든 히스토리를 잃는다. 물론 사람마다 하나씩 가진 스냅샷은 괜찮다. 로컬 VCS 시스템도 이와 비슷한 결점이 있고 이런 문제가 발생하면 모든 것을 잃는다.



## 분산 버전 관리 시스템

DVCS (분산 버전 관리 시스템) 인 Git, Mecurial, Bazaar, Darcs 같은 DVCS에서의 클라이언트는 단순히 파일의 마지막 스탭샷을 Checkout 하지 않는다. 그냥 저장소를 전부 복제한다. 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. 모든 Checkout은 모든 데이터를 가진 진정한 백업이다.



게다가 대부분의 DVCS 환경에서는 리모트 저장소가 존재한다. 리모트 저장소가 많을 수도 있다. 그래서 사람들은 동시에 다양한 그룹과 다양한 방법으로 협업할 수 있다. 계층 모델 같은 중앙집중식 시스템으로는 할 수 없는 워크플로를 다양하게 사용할 수 있다.



## Git 기초

Git을 배우려면 Subversion이나 Perforce 같은 다른 VCS를 사용하던 경험을 버려야한다. Git은 미묘하게 달라서 다른 VCS에서 쓰던 개념으로는 헷갈린다. 사용자 인터페이스는 매우 비슷하지만, 정보를 취급하는 방식이 다르다.



## 차이가 아니라 스냅샷

Subversion과 Subversion 비슷한 놈들과 Git의 가장 큰 차이점은 데이터를 다루는 방법에 있다. 큰 틀에서 봤을 때 VCS 시스템 대부분은 관리하는 정보가 파일들의 목록이다. CVS, Subversion, Perforce, Bazaar 등의 시스템은 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리한다.



![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FHYXP1%2FbtqwWykKvr1%2FK0kPMazCRqpkxJ9GRhsy0K%2Fimg.jpg)



Git은 이런 식으로 데이터를 저장하지도 취급하지도 않는다. 대신 Git은 데이터를 파일 시스템 스냅샷으로 취급하고 크기가 아주 작다. Git은 커밋하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 중요하게 여긴다. 파일이 달라지지 않았으면 Git은 성능을 위해서 파일을 새로 저장하지 않는다. 단지 이전 상태의 파일에 대한 링크만 저장한다. Git은 데이터를 ==스냅샷의 스트림==처럼 취급한다.



![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FA81CZ%2FbtqwWfTWAmU%2FELemaGNgtDawHZa8PofSu0%2Fimg.jpg)



이것이Git이 다른 VCS와 구분되는 점이다. 이점 때문에 Git은 다른 시스템들이 과거로부터 답습해왔던 버전 컨트롤의 개념과 다르다는 것이고 많은 부분을 새로운 관점에서 바라본다. Git은 강력한 도구를 지원하는 작은 파일 시스템이다. 

Git은 단순한 VCS가 아니다. Git 브랜치에서 설명할 Git 브랜치를 사용하면 얻게 되는 이득이 무엇인지 설명한다.



## 거의 모든 명령을 로컬에서 실행

거의 모든 명령이 로컬 파일과 데이터만 사용하기 때문에 네트워크에 있는 다른 컴퓨터는 필요 없다. 대분의 명령어가 네트워크의 속도에 영향을 받는 CVCS에 익숙하다면 Git이 매우놀라울 것이다. Git의 이런 특징에서 나오는 미칠듯한 속도는 오직 Git느님만이 구사할 수 있는 전능이다. 프로젝트의 모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령이 순식간에 실행된다.

에를 들어 Git은 프로젝트의 히스토리를 조회할 때 서버 없이 조회한다. 그냥 로컬 데이터베이스에서 히스토리를 읽어서 보여 준다. 그래서 눈 깜짝할 사이에 히스토리를 조회할 수 있다. 어떤 파일의 현재 버전과 한 달 전의 상태를 비교해보고 싶을 때도 Git은 그냥 한 달 전의 파일과 지금의 파일을 로컬에서 찾는다. 파일을 비교하기 위해 리모트에 있는 서버에 접근하고 나서 예전 버전을 가져올 필요가 없다.

즉 오프라인 상태이거나 VPN에 연결하지 못해도 막힘 없이 일 할 수 있다. 비행기 등에서 작업하고 네트워크에 접속하고 있지 않아도 커밋할 수 있다.



## Git의 무결성

Git은 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다. 그래서 체크섬을 이해하는 Git 없이는 어떠한 파일이나 디렉토리도 변경할 수 없다. 체크섬은 Git에서 사용하는 가장 기본적인(Atomic) 데이터 단위이자 Git의 기본 철학이다. Git 없이는 체크섬을 다룰 수 없어서 파일의 상태도 알 수 없고 심지어 데이터를 잃어버릴 수도 있다.

Git은 SHA-1 해시를 사용하여 체크섬을 만든다. 만든 체크섬은 40자 길이의 16진수 문자열이다. 파일의 내용이나 디렉토리 구조를 이용해서 체크섬을 구한다. SHA-1은 아래처럼 생겼다.

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

Git은 모든 것을 해시로 식별하기 때문에 이런 값은 여기저기서 보인다. 실제로 Git은 파일을 이름으로 저장하지 않고 해당 파일의 해시로 저장한다.



## Git은 데이터를 추가할 뿐

Git으로 무얼 하든 Git 데이터베이스에 데이터가 추가된다. 되돌리거나 데이터를 삭제할 방법이 없다. 다른 VCS처럼 Git도 커밋하지 않으면 변경사항을 잃어버릴 수 있다. 하지만, 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다.

Git을 사용하면 프로젝트가 심각하게 망가질 걱정 없이 매우 즐겁게 여러 가지 실혐을 해볼 수 있다. 되돌리기을 보면 Git에서 데이터를 어떻게 저장하고 손실을 어떻게 복구하는지 알 수 있다.



## 세가지 상태

이 부분은 중요하기에 집중해서 읽어야 한다. Git을 공부하기 위해 반드시 짚고 넘어가야 할 부분이다. Git은 파일을 Committed, Modified, Staged 이렇게 세 가지 상태로 관리한다. Committed란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다. Modified는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다. Staged란 현재 수정한 파일을 곧 커밋할 것이라는 표시한 상태를 의미한다.

이 세 가지  상태는 Git 프로젝트의 세 가지 단계와 연결돼 있다. Git 디렉토리, 워킹 트리, Staging Area 이렇게 세 가지 단게를 이해하고 넘어가자.



![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Frmoet%2FbtqwWnEbnop%2FuhkBfjV2lKx5TVp7ayV840%2Fimg.jpg)



Git 디렉토리는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 말한다. 이 Git 디렉토리가 Git의 핵심이다. 다른 컴퓨터에 있는 저장소를 Clone 할 때 Git 디렉토리가 만들어진다.

워킹 트리는 프로젝트의 특정 버전을 Checkout 한 것이다. Git 디렉토리는 지금 작업하는 디스크에 있고 그 디렉토리안에 압축된 데이터베이스에서 파일을 가져와서 워킹 트리를 만든다.

Staging Area는 Git 디렉토리에 있다. 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장한다. 종종 "Index" 라고 불리기도 하지만, Staging Area라는 명칭이 표준이 되어가고 있다.

Git으로 하는 일은 기본적으로 아래와 같다.

1. 워킹 트리에서 파일을 수정한다.
2. Staging Area에 파일을 Stage 해서 커밋할 스냅샷을 만든다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉토리에 영구적인 스냅샷으로 저장한다.



Git 디렉토리에 있는 파일들은 Committed 상태이다. 파일을 수정하고 Staging Area에 추가했다면 Staged이다. 그리고 Checkout 하고 나서 수정했지만, 아직 Staging Area에 추가하지 않았으면 Modified이다. Git의 기초에서 이 상태에 대해 좀 더 자세히 배운다. 특히 Staging Area를 이용하는 방법부터 아예 생략하는 방법까지도 설명한다.









# Git의 기초

## Git 저장소 만들기

Git 저장소를 만드는 방법은 두 가지다. 기존 프로젝트나 디렉토리를 Git 저장소로 만드는 방법이 있고, 다른 서버에 있는 저장소를 Clone 하는 방법이 있다.



### 기존 디렉토리를 Git 저장소로 만들기

기존 프로젝트를 Git으로 관리하고 싶을 때, 프로젝트의 디렉토리로 이동한다. 그리고 아래와 같은 명령을 실행한다.

```
$ git init
```

이 명령은 .git 이라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일 (Skeleton)이 들어 있다. 이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다. 

Git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야 한다. git add 명령으로 파일을 추가하고 git commit 명령으로 커밋한다.

```
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

명령어 몇 개로 순식간에 Git 저장소로 만들고 파일 버전 관리를 시작했다.



## 기존 저장소를 Clone 하기

다른 프로젝트에 참여하려거나 (Contribute) Git 저장소를 복사하고 싶을 때 git clone 명령을 사용한다. Git이 Subversion과 다른 가장 큰 차이점은 서버에 있는 거의 모든 데이터를 복사한다는 것이다.

git clone을 실행하면 프로젝트 히스토리를 전부 받아온다. 실제로 서버의 디스크가 망가져도 클라이언트 저장소 중에서 아무거나 하나 가져다가 복구하면 된다. 

**<span style ="color:#B40404">git clone [url]</span>** 명령으로 저장소를 Clone 한다. libgit2 라이브러리 소스코드를 Clone  하려면 아래와 같이 실행한다.

```
$ git clone https://github.com/libgit2/libgit2
```

>이 명령은 'libgit2' 이라는 디렉토리를 만들고 그 안에 <span style ="color:#B40404">.git</span> 디렉토리를 만든다. 그리고 저장소의 데이터를 모드 가져와서 자동으로 가장 최신 버전을 Checkout 해 놓는다. <span style ="color:#B40404">libgit2</span> 디렉토리로 이동하면 Checkout으로 생성한 파일을 볼 수 있고 당장 하고자 하는 일을 시작할 수 있다. 아래와 같은 명령을 사용하여 저장소를 Clone 하면 'libgit2' 가 아니라 다른 디렉토리 이름으로 Clone 할 수 있다.

```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

디렉토리 이름이 <span style ="color:#B40404">mylibgit</span> 이라는 것만 빼면 이 명령의 결과와 앞선 명령의 결과는 같다.

Git은 다양한 프로토콜을 지원한다. 이제까지는 <span style ="color:#B40404">https://</span> 프로토콜을 사용했지만 <span style ="color:#B40404">git://</span>를 사용할 수도 있고 <span style ="color:#B40404">user@server:path/to/repo.git</span> 처럼 SSH 프로토콜을 사용할 수도 있다. 



## 수정하고 저장소에 저장하기

만질 수 있는 Git 저장소를 하나 만들었고 워킹 디렉토리에 Checkout도 했다.  이제는 파일을 수정하고 파일의 스냅샷을 커밋해 보자. 파일을 수정하다가 저장하고 싶으면 스냅샷을 커밋한다.

워킹 디렉토리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked (관리대상이 아님)로 나눈다.  Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이다. 

Tracked 파일은 또 Unmodified (수정하지 않음)와 Modified (수정함) 그리고 Staged (커밋으로 저장소에 기록할) 상태 중 하나이다. 그리고 나머지 파일은 모두 Untracked 파일이다.

Untracked 파일은 워킹 디렉토리에 있는 파일 중 스냅샷에도 Staging Area 에도 포함되지 않은 파일이다. 처음 저장소를 Clone 하면 모든 파일은 Tracked 이면서 Unmodified 상태이다. 파일을 Checkout 하고 나서 아무것도 수정하지 않았기 때문에 그렇다.





마지막 커밋 이후 아직 아무것도 수정하지 않은 상태에서 어떤 파일을  수정하면 Git은 그 파일을 Modified 상태로 인식한다. 실제로 커밋을 하기 위해서는 이 수정한 파일을 Staged 상태로 만들고, Staged 상태의 파일을 커밋한다. 이런 라이프사이클을 계속 반복한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FIfe8M%2Fbtqw0aYes7G%2FjKy3ZP0KDIN2B1MvUihVMk%2Fimg.jpg)





## 파일의 상태 확인하기

파일의 상태를 확인하려면 보통 <span style ="color:#B40404;font-weight:bold; font-size:20px">git status</span> 명령을 사용한다.

Untracked files 부분에 속해 있는 파일은 Untracked 상태라는 것을 말한다.



## 파일을 새로 추적

<span style ="color:#B40404;font-weight:bold; font-size:20px">git add </span> 명령으로 파일을 새로 추적할 수 있다.



## 파일 상태를 짤막하게 확인하기

<span style ="color:#B40404;font-weight:bold; font-size:20px">git status</span> 명령으로 확인할 수 있는 내용이 좀 많아 보일 수 있다. 그래서 좀 더 간단하게 변경 내용을 보여주는 옵션이 있다. 

<span style ="color:#B40404;font-weight:bold; font-size:20px">git status -s </span>또는<span style ="color:#B40404;font-weight:bold; font-size:20px"> git status --short</span> 처럼 옵션을 주면 현재 변경한 상태를 짤막하게 보여준다.

```
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

- 아직 추적하지 않은 새 파일 앞에는 ?? 표시가 붙는다.
- 새로 생성한 파일 앞에는 A 표시가 붙는다.
- 수정한 파일 앞에는 M 표시가 붙는다.

그리고 왼쪽에는 Staging Area에서의 상태를, 오른쪽에는 Working Tree에서의 상태를 표시한다.



## 파일 무시하기

어떤 파일은 Git이 관리할 필요가 없다. 보통 로그 파일이나 빌드 시스템이 자동으로 생성한 파일이 그렇다. 그런 파일을 무시하려면 <span style ="color:#B40404;font-weight:bold; font-size:20px">.gitignore</span> 파일을 만들고 그 안에  무시할 파일 패턴을 적는다.

```
$ cat .gitignore
*.[oa]
*~
```

첫 번째 라인은 확장자가 ".o" 나 ".a" 인 파일을 Git이 무시하라는 것이고, 둘째 라인은 ~로 끝나는 모든 파일을 무시하라는 것이다.

### .gitignore 파일에 입력하는 패턴의 규칙

- 아무것도 없는 라인이나, '#' 로 시작하는 라인은 무시한다.
- 표준 Glob 패턴을 사용한다.
- 슬래시(/)로 시작하면 하위 디렉토리에 적용되지 (Recursivity) 않는다.
- 디렉토리는 슬래시 (/)를 끝에 사용하는 것으로 표현한다.
- 느낌표 (!)로 시작하는 패턴은 파일을 무시하지 않는다.

```
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉토리에 있는 TODO 파일은 무시하고 subdir/TODO 처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉토리에 있는 모든 파일은 무시
build/ 

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉토리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf

```







## Staged와 Unstaged 상태의 변경 내용을 보기

단순히 파일이 변경됐다는 사실이 아니라 어떤 내용이 변경됐는지 살펴보려면 <span style ="color:#B40404;font-weight:bold; font-size:20px">git diff</span>  명령을 사용해야 한다.

만약 커밋하려고 Staging Area에 넣은 파일의 변경 부분을 보고 싶으면 <span style ="color:#B40404;font-weight:bold; font-size:20px">git diff --staged</span> 옵션을 사용한다. 이 명령은 저장소에 커밋한 것과 Staging Area에 있는 것을 비교한다.

꼭 잊지 말아야 할 것이 있는데 <span style ="color:#B40404;font-weight:bold; font-size:20px"> git diff </span> 명령은 마지막으로 커밋한 후에 수정한 것들 전부를 보여주지 않는다. <span style ="color:#B40404;font-weight:bold; font-size:20px">git diff </span>는 Unstaged 상태인 것들만 보여준다. 수정한 파일을 모두 Staging Area에 넣었다면 <span style ="color:#B40404;font-weight:bold; font-size:20px">git diff</span> 명령은 아무것도 출력하지 않는다.

Staged 상태인 파일은 <span style ="color:#B40404;font-weight:bold; font-size:20px">git diff --cached</span> 옵션으로 확인한다. <span style ="color:#B40404;font-weight:bold; font-size:20px"> -- staged</span> 와 <span style ="color:#B40404;font-weight:bold; font-size:20px"> --cached</span>는 같은 옵션이다.



## 변경사항 커밋하기

Git은 생성하거나 수정하고 나서 <span style ="color:#B40404;font-weight:bold; font-size:20px"> git add</span> 명령으로 추가하지 않은 파일은 커밋하지 않는다. 그 파일은 여전히 Modified 상태로 남아 있다. 커밋하기 전에 <span style ="color:#B40404;font-weight:bold; font-size:20px"> git status </span> 명령으로 모든 것이 Staged 상태인지 확인할 수 있다.

그 후에 <span style ="color:#B40404;font-weight:bold; font-size:20px"> git commit</span> 을 실행하여 커밋한다.

```java
$ git commit
$ git commit -m "메시지" // 메시지를 인라인으로 첨부하는 옵션 -m
$ git commit -am "메시지" // add와 메시지 추가를 동시에 하는 옵션
```



## 파일 삭제 하기

Git에서 파일을 제거하려면 <span style ="color:#B40404;font-weight:bold; font-size:20px"> git rm </span>명령으로 Tracked 상태의 파일을 삭제한 후에 (정확하게는 Staging Area에서 삭제하는 것) 커밋해야 한다.

이 명령은 워킹 디렉토리에 있는 파일도 삭제하기 때문에 실제로 파일도 지워진다.



## 파일 이름 변경하기

Git은 다른 VCS 시스템과는 달리 파일 이름의 변경이나 파일의 이동을 명시적으로 관리하지 않는다. 다시 말해서 파일 이름이 변경됐다는 별도의 정보를 저장하지 않는다. 다시 말해서 파일 이름이 변경됐다는 별도의 정보를 저장하지 않는다. Git은 똑똑해서 굳이 파일 이름이 변경되었다는 것을 추적하지 않아도 아는 방법이 있다. 파일의 이름이 변경된 것을 Git이 어떻게 알아내는지  알아보자

이렇게 말하고 Git에 <span style ="color:#B40404;font-weight:bold; font-size:20px"> mv</span> 명령이 있는게 좀 이상하지만, 아래와 같이 파일 이름을 변경할 수 있다.

```java
$ git mv file_from file_to
```

