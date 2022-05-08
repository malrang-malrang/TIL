# Today I Learned.

## 학습내용

# Swift Lint 란?
>커뮤니티 혹은 팀에서 정한 스타일 규칙을 따르지 않는 코드부분을 식별하고 표시하는것을 도와준다.
>
>개발자 마다 코딩 스타일이 다르기때문에, 코드를 작성할 때 추천하는 코딩 스타일, 괜찮은 사례등을 모아 놓은 가이드 라인이다.
>
>이러한 코딩 컨벤션들을 Rule로 정함으로써 모든 Swift 코드가 일관성을 유지하도록 해준다.
>이렇게 하면 새로운 개발자가 합류하거나 예전 코드를 읽을 때 도움이 된다.

## Rule
>[Swift Lint Rule](https://realm.github.io/SwiftLint/rule-directory.html)
>`Swift Lint` 에는 75개가 넘는 룰들이 있다.
>`Rule`에는 `Default`로 적용되는 `Rule`도 있지만 사용자에 의해 적용되는 `Rule`들도 존재한다.
>`Rule`들을 세부적으로 적용 시키기 위해서는  `.swiftlint.yml`파일을 추가해주어야 한다.

### Rule 적용 여부를 설정하는 파라미터들
>**크게 3가지가 존재한다.**
>1. disabled_rules : 기본 활성화된 룰 중에 비활성화할 룰들을 지정한다.
>2. opt_in_rules : 기본 룰이 아닌 룰들을 활성화한다.
>3. whitelist_rules : 지정한 룰들만 활성화되도록 화이트리스트로 지정한다. disabled_rules 및 opt_in_rules 는 같이 사용할 수 없다.

### 공식문서 예시
```swift
disabled_rules: # 실행에서 제외할 룰 식별자들
  - colon
  - comma
  - control_statement
opt_in_rules: # 일부 룰은 옵트 인 형태로 제공
  - empty_count
  - missing_docs
  # 사용 가능한 모든 룰은 swiftlint rules 명령으로 확인 가능
included: # 린트 과정에 포함할 파일 경로. 이 항목이 존재하면 `--path`는 무시됨
  - Source
excluded: # 린트 과정에서 무시할 파일 경로. `included`보다 우선순위 높음
  - Carthage
  - Pods
  - Source/ExcludedFolder
  - Source/ExcludedFile.swift

# 설정 가능한 룰은 이 설정 파일에서 커스터마이징 가능
# 경고나 에러 중 하나를 발생시키는 룰은 위반 수준을 설정 가능
force_cast: warning # 암시적으로 지정
force_try:
  severity: warning # 명시적으로 지정
# 경고 및 에러 둘 다 존재하는 룰의 경우 값을 하나만 지정하면 암시적으로 경고 수준에 설정됨
line_length: 110
# 값을 나열해서 암시적으로 양쪽 다 지정할 수 있음
type_body_length:
  - 300 # 경고
  - 400 # 에러
# 둘 다 명시적으로 지정할 수도 있음
file_length:
  warning: 500
  error: 1200
# 네이밍 룰은 경고/에러에 min_length와 max_length를 각각 설정 가능
# 제외할 이름을 설정할 수 있음
type_name:
  min_length: 4 # 경고에만 적용됨
  max_length: # 경고와 에러 둘 다 적용
    warning: 40
    error: 50
  excluded: iPhone # 제외할 문자열 값 사용
identifier_name:
  min_length: # min_length에서
    error: 4 # 에러만 적용
  excluded: # 제외할 문자열 목록 사용
    - id
    - URL
    - GlobalAPIKey
reporter: "xcode" # 보고 유형 (xcode, json, csv, checkstyle, junit, html, emoji, markdown)
```
### 커스텀 룰 적용 예시
```swift
custom_rules:
  pirates_beat_ninjas: # 룰 식별자
    included: ".*.swift" # 린트 실행시 포함할 경로를 정의하는 정규표현식. 선택 가능.
    name: "Pirates Beat Ninjas" # 룰 이름. 선택 가능.
    regex: "([n,N]inja)" # 패턴 매칭
    match_kinds: # 매칭할 SyntaxKinds. 선택 가능.
      - comment
      - identifier
    message: "Pirates are better than ninjas." # 위반 메시지. 선택 가능.
    severity: error # 위반 수준. 선택 가능.
  no_hiding_in_strings:
    regex: "([n,N]inja)"
    match_kinds: string
```

## SwiftLInt 적용방법!
1. pod file  에 swiftLint 를 저장한다!
>터미널 명령어로 `SwiftLint` 를 적용하고싶은 경로로 이동한다!
>![](https://i.imgur.com/LkzN9yn.png)
>경로로 이동한후 `vi Podfile` 키워드를 입력한다!
>![](https://i.imgur.com/pIrfPQE.png)
>`i` 를 누른후 저기 개행되어있는 곳에 `pod 'SwiftLint'`키워드를 입력한다!
>`esc`를 누른후 `:wq!` 를 누른후 `enter` 를 누른다!
>![](https://i.imgur.com/YVzjqhD.png)
>그후 터미널 명령어로 `pod install` 키워드를 누른후 `enter`!
>![](https://i.imgur.com/YSC32UV.png)
> 위와같이 작업했다면 해당 폴더에 `Pods` 폴더와 `workspace`파일들이 생겨난다.

2. script 작성!
>생성된 `walkspace`파일로 들어가 모듈을 클릭한후 타겟에서 어떤 놈을 적용시킬건지 선택한다.
>그후 빌드 `phase` 를 클릭! 
>![](https://i.imgur.com/w1UUOPY.png)
>`+`버튼을 누른후 `New Run Script Phase` 클릭!
>![](https://i.imgur.com/soYySBT.png)
>새로 생긴 `Run Script` 내부를 수정해야한다!
>![](https://i.imgur.com/NAQ3AZY.png)
>`"{PODS_ROOT}/SwiftLint/swiftlint"` 키워드를 입력!
>![](https://i.imgur.com/k7OfzEy.png)
>키워드를 입력후 빌드를 하게되면 SwiftLint 컨벤션에 맞지않는 코드들이 `warning` 표기로 알려주게된다!
>사용하고 싶지않은 컨벤션 옵션이나 사용하고 싶은 컨벤션 옵션들만 표기해주도록 변경해보자!

3. yml
> 모듈내 최상위에 `New File` 을 누른뒤 `empty` 파일을 만들어주고 파일 네임 끝에`.yml`키워드를 작성해 `.yml` 파일을 만들어준다.
>![](https://i.imgur.com/Ksbq1ds.png)
>`yml` 파일 내부에 위에 설명했던 옵션들을 적용해보자!
>![](https://i.imgur.com/dhHEz7F.png)
>위의 예시는 Pods 모듈 내부의 파일들은 SwiftLint 컨벤션을 적용시키지 않을것이고.
>기본 컨벤션 옵션들중에 `void_return`, `tralling_whitespace` 등의 옵션은 적용하지 않겠다고 설정한것이다.

