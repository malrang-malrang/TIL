# Today I Learned.

## 학습내용

# CocoaPods 을 사용해보자!

## 코코아팟(CocoaPods)이란?
>코코아팟(CocoaPods)은 Swift 및 Objective-C 언어 환경 프로젝트의 의존성을 관리해주는 도구다.
>
>코코아 팟은 `Ruby` 로 개발되어있다.
>
>현재 가장많은 라이브러리를 보유하고있다.
>`Podfile` 에 사용할 라이브러리만 명시해주면 자유롭게 프로젝트에서 사용할수 있다.

## 코코아팟을 설치해보자!
>터미널을켜고 `sudo gem install cocoapods` 키워드를 입력하자!
>![](https://i.imgur.com/zAfpOAy.png)

## Podfile 을 만들어보자!
>터미널에서 `cd`  명령어를 사용해 의존성 관리도구를 적용할 프로젝트 경로로 이동한다!
>![](https://i.imgur.com/HmX6Lkp.png)
>
>터미널에 `pod init` 명령어를 사용하면 앞으로 의존성을 관리할 `Podfile`이 생성된다.
>![](https://i.imgur.com/SeJb3w8.png)

## Podfile 을 실행해 사용할 라이브러리를 추가하자!
>`Pod file`이 존재하는 프로젝트 경로에서(폴더내부) `vi Podfile` 명령어를 실행하자
>![](https://i.imgur.com/mGGuxbh.png)
> `Podfile` 내부를 수정할수 있게된다.
>![](https://i.imgur.com/RnOn2BC.png)
>이후 `i` 커맨드를 클릭해 원하는 라이브러리를 명시해주면된다!
>![](https://i.imgur.com/YgUk4Ku.png)
>그럼 이제 사용하고싶은 라이브러리를 명시해주었으니 `:wq` 명령어를 입력해서 저장하자!
>그럼 이제 적용할 프로젝트 경로로 이동해 `pod install`명령어를 실행해준다!
>![](https://i.imgur.com/a4xXSMP.png)
>그러면 이렇게 해당 폴더에 `workspace` 파일이 생겨났다.
>![](https://i.imgur.com/ac9fmKG.png)
>이제는 `Xcode` 파일이 아닌 `workspace` 파일 에서 프로젝트를 진행하면 된다!


### Podfile 에 라이브러리를 추가할때 주의할점!
>라이브 러리도 제작자가 업데이트를 할수도있다!!
>
>하지만 사용할 라이브러이의 버젼을 항상 최신버젼으로 사용한다면 따로 버젼을 명시하지 않아도된다!
>`pod 'Alamofire'` < 예시
>
>추가할 라이브러리 의 특정 버전만을 사용할수도 있다!
>`pod 'Alamofire', '1.0'` < 예시
>
>**버젼에 대한조건을 명시할수도 있다!**
>- `> 0.1`: 0.1 보다 높은 버젼
>- `>= 0.1`: 0.1 이고 보다 높은 버젼
>- `< 0.1`: 0.1 보다 낮은 버젼
>- `<= 0.1`: 0.1 이고 보다 낮은 버젼
>- `~> 0.1.2`: 0.1.2 이상이지만 0.2 미만의 버젼
>- `~> 0.1`: 0.1 이상이지만 1.0 미만의 버젼
>- `~> 0`: 실질적으로 포함시키지 않는다는 의미

## Podfile.lock
>프로젝트 경로에 `pod install`을 하고나면 `Podfile.lock`라는 파일이 생긴다.
>
>`Podfile.lock` 은 pod(추가한 라이브러리) 들의 버젼을 게속 추적하여 기록하고 유지시키는 역할을 한다.
>
>**CHECKSUM???** 
>
>![](https://i.imgur.com/erNOSeu.png)
>
>요녀석은 유일성을 보증해주는 해쉬값이다.
>`pod` 들중 하나라도 버젼에 변화가 생긴다면 `CHECKSUM` 요녀석도 변화가 생긴다.
>
>`CHECKSUM`이 변함없이 유지된다는것은 협업하는 환경에서 pod 버젼을 모두 동일하게 사용하고있음을 의미한다.
>`CHECKSUM`의 값이 변했다면 `pod`들중 무언가의 버젼이 변화가생겼다는것을 의미하며 `pod`버젼을 업데이트 했다면 `Podfile.lock`도 같이 커밋해주어야 한다.

## 코코아팟 라이브러리를 만들어보자!
>간단한 알럿을 띄우는 라이브러리를 제작하고 오픈소스로 배포 해보자!

### 1. 터미널에서 초기 설정!
>터미널에서 `pod lib create SimpleAlert` 명령어를 실행해보자! `SimpleAlert` 은 지금 만드려는 라이브러리의 이름이니까 다른 라이브러리를 만들때는 다른 이름을 사용하면된다!
>
>바탕화면에 비어있는 폴더를 만들었고 비어있는 폴더 경로로 들어가 위의 명령어를 실행했다.
>![](https://i.imgur.com/1NHQibS.png)
>그러면 아래의 사진처럼 코코아팟의 간단한 설정이 진행된다!
>5개의 질문으로 구성되어있다!
>![](https://i.imgur.com/X2an0Ad.png)
>질문들은 다음과 같다.
>1. What platform do you want to use?? [ iOS / macOS ]
>    - 어떤 플랫폼 쓸거임?
>2. What language do you want to use?? [ Swift / ObjC ]
>    - 무슨 언어 쓸거임??
>3. Would you like to include a demo application with your library? [ Yes / No ]
>    - 음.. 새로 만드려는 라이브러리에 대한 화면 스크린샷이 필요한지? 라고 이해하자 (알럿은 화면에 나타나는 기능이기때문에 Yes)
>    
>4. 1. Which testing frameworks will you use? [ Specta / Kiwi / None ]
>    - 2번 질문에서 Objc 를 고른 경우
>테스트를 위한 프레임 워크를 추가적으로 사용할 것인지 묻는 질문
>None 을 고르면 Apple 의 XCTest를 사용 하면된다.
>4. 2. Which testing frameworks will you use? [ Quick / None ]
>    - 2번 질문에서 swift 를 고른 경우 
>설명은 위와 같다.
>5. Would you like to do view based testing? [ Yes / No ]
>    - 뷰 기반의 테스트를 진행할것인지?
>    - yes 누르면 스냅샷 기반의 테스트 오픈소스 `FBSnapshotTestCase` 를 포함시켜준다
>    
>위의 질문들에 대한 값을 입력하면 폴더에 `Xcode` 파일이 생성되고 자동으로 `Xcode` 프로젝트가 실행된다.
>
>![](https://i.imgur.com/a8JSLa8.png)

### podspec??
>위처럼 라이브러리를 생성했다면 같은 경로에 (라이브러리명) 파일이 같이 생성된다.
>
>`.podspec` 은 팟 라이브러리에 대한 정보를 가진 파일이다.
>
>소스를 가져와야하는곳, 사용할 파일, 적용할 빌드 설정외에도 이름, 버젼, 라이브러리에 대한 설명 등 일반적인 메타데이터가 포함된다.
>
>`.podspec` 파일에 문제가 생기면 라이브러리를 코코아팟에 배포할때 오류가 생긴다.
>
>`.podspec` 을 실행해보면 다음과 같은 값들이 들어있다.
>![](https://i.imgur.com/EnSTFuF.png)
>주석 처리된부분을 제외하고 보면(앞에 # 되어있는거 주석 임) name, version 등등 작성되어있는데 이것들에대한 설명을 보자!
>
>#### `.podspec`항목에 대한 설명
>- name: 라이브러리 이름
>- version: 배포 버전
>- license: 오픈소스 라이선스 정보
>- homepage: 홈페이지 주소로 주로 Github 저장소의 메인 페이지를 사용
>- author: 라이브러리 만든이의 이름과 이메일
>- summary: 라이브러리에 대한 간단한 설명
>- source: 라이브러리 소스가 위치해있는 원격 저장소 주소
>- source_files: 소스 파일이 위치한 디렉토리 주소
>- frameworks: 사용한 프레임워크
>
>위의 항목중 s.source 를 보면 라이브러리 소스를 받아올 `Github` 저장소 `URL` 이 작성되어있다. 
>그렇다 ; 라이브러리 소스를 `Github` 저장소에 올려두어야 한다는 소리다.

>그럼 일단 비어있는 레포를 하나 만들어보자!
>![](https://i.imgur.com/O8v8ciO.png)
>그후 해당 폴더를 터미널 혹은 깃관리 프로그램을 이용해 방금 만들어준 비어있는 레포지토리에 연결, 푸쉬 해준다!
>![](https://i.imgur.com/iGyWogc.png)
>그후 `.podspec`을 열어보면 s.source 에 Github 레포지토리 주소가 기입된걸 볼수있다.
>![](https://i.imgur.com/tCl7KBm.png)

## 소스코드를 작성하자!
>이제 라이브러리의 핵심이 되는 코드를 작성해보자!
>![](https://i.imgur.com/EbPLC2d.png)
>디렉토리를 보면 Malrangmalrang 프로젝트와 Pods 프로젝트로 나뉜것을 볼수있다. 
>Malrangmalrang 하위의 Example for Malrangmalrang 은 데모 어플리케이션을 구현할수 있는 영역이다.
>쉽게 말하면 Malrangmalrang 라이브러리를 개발한후 실제 라이브러리를 사용하는 입장에서 테스트할수 있는곳이다.
>
>Pods 모듈 내부의 ReplaceMe.swift 파일 내에 외부모듈에서 사용될 코드를 임시로 만들어 보았다.
>이때 클래스의 접근제어는 open, public 이어야한다.
>![](https://i.imgur.com/cXvPS8y.png)
>open, public 둘다 외부 모듈에서 접근은 가능하지만 차이점은 open 은 상속이 가능 하다는점이다.
>
>이후 테스트하기위한 Example for Malrangmalrang 내부의 파일에서 상위에 import Malrangmalrang 키워드를 작성하면 방금 만들어둔 타입을 구현하지않아도 사용할수 있게 된다.
>![](https://i.imgur.com/bWTCHKW.png)
