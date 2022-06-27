# Today I Learned.

## 학습내용
# Localization
![](https://i.imgur.com/r5gqyNM.png)

여러 언어와 지역을 지원하여 앱 시장을 확장하십시오.

## Overview
현지화는 당신의 앱을 여러 언어와 지역으로 번역하고 적응시키는 과정입니다. 다양한 언어를 사용하고 다른 App Store 영역에서 다운로드하는 사용자에게 액세스 권한을 제공하기 위해 앱을 현지화합니다.

먼저 언어와 지역에 맞게 문자열을 자동으로 포맷하고 번역하는 API로 코드를 국제화합니다. 그런 다음 번역의 정확도를 높이기 위해 언어 복수 규칙에 따라 복수 명사와 동사를 포함하는 내용에 대한 지원을 추가합니다.

## Translate and Adapt Your App
Xcode에서 현지화는 사용자가 지원하는 특정 언어 및 지역에 대한 리소스 집합을 의미합니다.

프로젝트에 현지화를 추가하고 해당 언어와 지역에 포함할 리소스를 선택합니다. 현지화를 내보내고 파일을 로컬라이저로 보냅니다. 로컬라이저는 사용자 대면 텍스트를 번역하고 특정 문화 및 지역에 맞게 리소스를 조정합니다. 마지막으로 현지화된 파일을 가져오고 해당 언어와 지역에서 Xcode로 직접 앱을 테스트합니다.

현지화된 버전의 앱을 출시할 때 앱을 제공하는 특정 지역에 대한 App Store Connect에서 App Store 정보를 현지화할 수도 있습니다.

기타 현지화 팁, 도구 및 리소스는 앱을 새 시장으로 확장을 참조하십시오.


---
앱을 사용할때 앱을 사용하는 지역(나라)과 사용하는 언어에따라 다르게 표현되어야 할때가있다.

예를 들면 한국에서 날짜를 표기할때는 년, 월, 일 로 표기하고 미국에서는 Years, Month, Day 로 표기해야할수도 있다. 

Label의 Text를 국가(지역)에따라 그나라 언어로 하고 싶을수있다.

이때 Localization을 활용하면 앱사용자의 지역이나 설정한 언어에따라 다르게 표기할수있도록 코드를 작성할수있다.

Localizing 말그대로 다국어 처리를 하는것이다.

어플 유저가 한국에만 한정되어있다면 필요없지만 외국에서 외국 유저가 사용하는 어플이라면 다국어 처리를 해주어야 한다.

지역 혹은 언어에따라 다르게 표현하는것을 지역화 라고 하며 지역화 할수있는것들에는 다음과 같은 종류들이있다.

- 텍스트
- 이미지
- 날짜
- 화폐단위

지역화 는 현지화 한다는 뜻을 가졌는데 해당 언어와 나라 지역에 맞게 앱을 설정해주는것을 뜻한다.

세계화(internationalization)를 I18N, 지역화(localization)를 L10N 으로 표기하기도 한다.

지역화 하기위해서는 먼저 해당앱이 여러국가에 배포되어 세계화되어있는 앱이라는 조건이 필요하다.

해당앱이 한국에서만 사용되는 앱이라면 지역화가 의미가 없겠죠??

그럼 지역화 해보자!

## 1. Text 지역화 하는 방법
**1. StoryBoard에서 Label을 하나 만들어준다!**
![](https://i.imgur.com/1A1YydK.png)

**2. Project - Info - Localizations에서 +버튼을 눌러 지역을 추가한다!**

IOS App의 기본 Localization은 영어다.
요번예시는 Korea(한국)을 추가할거다!

![](https://i.imgur.com/zm7aoEW.png)

![](https://i.imgur.com/oSnF0tW.png)

**3. Localization이 생성된것을 확인한다.**

아래의 사진처럼 StroyBoard에 Strings가 추가된걸 확인할수 있다.
앞에 얘기했듯이 Base는 영어고 좀전에 추가한 Korean이 생긴것이다.
![](https://i.imgur.com/lDimO2O.png)

**4. Korean을 클릭해보자.**
![](https://i.imgur.com/JPxTzZo.png)

요렇게 내용이 추가되어있다.

**5. ObjectID**
ObjectID = "9IR-Fr-xvJ" 요런녀석이 있는데
ObjectID는 다 다르게 되어있는데 요녀석은 아까 만들어두었던 Label의 고유 ID 즉 식별자다.
StoryBoard에서 확인 가능하다!

아래사진의 맨밑쪽에 적혀있다!
![](https://i.imgur.com/uQz3t3p.png)

StoryBoard에서 다국어 처리는 해당 객체의 ObjectID로 한다.

지금까지 StoryBoard에서 작성한 text는 base로 구분되어 영어 로 구분된다.

그렇기 때문에 영어 다국어 처리는 StoryBoard에서, 다른 나라(Korean)은 따로 생성된 파일에서 해주면된다!

**6. 다국어 처리를 해보자!**

위에 설명한데로 Base는 영어니까 StoryBoard에서 Label의 text를 Hello 로 변경해주었다.
![](https://i.imgur.com/sv73BQW.png)

그리고 StoryBoard의 Korean파일에서 Label의 text를 안녕하세요 로 변경해주었다!
![](https://i.imgur.com/e3DKpj1.png)

이렇게 해주면 끝이다!

확인해보면 StoryBoard에 분명 Hello 라고 작성해둔 Label이 안녕하세요 라고 표기된걸 확인할수있다!
![](https://i.imgur.com/QTVdKdM.png)

현재 시뮬레이터의 언어가 Korean(한국어)로 설정되어있기 때문이다!

그럼 시뮬레이터의 언어를 English로 변경해보면 Labele의 text가 Hello로 변경된걸 확인할수 있다!
![](https://i.imgur.com/Z9cavTW.png)

## 새로운 Label을 추가했을때 
새로운 Label을 추가했다면 Korean 파일에 새로운 ObjectID가 추가되지 않는다... 수동으로 추가해주어야 한다.

StoryBoard에 Bye라는 text를 가진 Label을 추가했다.
![](https://i.imgur.com/N6LUamg.png)

그럼 아까전 ObjectID를 확인한것처럼 새로만든 Label의 ObjectID를 
확인한다!
![](https://i.imgur.com/IOgTu2x.png)

그리고 수동으로... Korean 파일들어가서 추가해준다!
![](https://i.imgur.com/XWfEGI5.png)

세미 콜론 빼먹으면 안된다!

## 2. Code로 다국어 처리하기
**1. String 파일을 추가한다!**
![](https://i.imgur.com/auauzDN.png)

**2. 파일 이름은 Localizable.strings 로 만든다!**

이름 다른걸로하면 안된다!

![](https://i.imgur.com/03VPYEJ.png)

**3. 파일이 생겼는지 확인한다.**
![](https://i.imgur.com/9m2YyeX.png)

**4. 우측 네비게이터에서 Localization을 클릭한다!**
![](https://i.imgur.com/Oq4lZxk.png)

그럼 요런 녀석이 생기는데 아까 StoryBoard에서 Base가 영어라고 했듯이 IOS의 iOS App Localization의 기본 Base는 영어다.
![](https://i.imgur.com/MaeNrgf.png)

**5. 원하는 다국어 파일을 추가하자.**

스토리보드에서 했던것처럼 Project - Info - Localizations에서 +버튼을 눌러 지역을 추가한다!

![](https://i.imgur.com/zm7aoEW.png)

![](https://i.imgur.com/oSnF0tW.png)

지역을 추가할때 아까와 달리 코드로 할거기때문에 Localizable.strings만 체크해준다.

![](https://i.imgur.com/rIDP1TD.png)

그럼 요렇게 Korean파일이 추가된걸 확인할수있다!

![](https://i.imgur.com/49kSwQI.png)

**6. Korean파일 내부의 다국어 처리**

그럼 이제 Korean파일 내부 에서 다국어 처리를 해주어야하는데 처음 생성했을때는 비어있다.

내부에 "Key" = "value" 방식으로 다국어 처리를 해주면된다.

```swift
"Hello" = "안녕하세요";
```
 요렇게 파일 내부에 작성해보자!

여기서 Key는 Hello Value는 안녕하세요 다.

Korean 파일 내부에 위의 Key와 Value를 작성해주었으니 Hello라는 Key는 다음과 같이 사용하면된다!

예제로 사용할 Label을 하나 만들어두자!

![](https://i.imgur.com/Wob4qrQ.png)

**7. String(format:)메서드를 이용하자!**
```swift
hello.text = String(format: NSLocalizedString("Hello", comment: ""))
```
위에 만들어둔 예제Label 을가져와서 String(format:) 메서드를 사용해서 넣어주면 된다!

파라미터로는 NSLocalizedString 을 초기화 해서 넣어주면되는데 Key값에 아까 정의해둔 Hello 를 넣어준다.

comment는 주석을뜻하는데 필요없어서 "" 로 비워두었다.

요렇게 하면 Localizable에 정의한 Hello 라는 Key의 Value값을 현재국가에 맞는 value값으로 가져와서 반환해준다.

## 화폐단위 지역화 하기!
```swift
func currency() {
    let locale = Locale.current
    let price = 3000.33 as NSNumber
    let formatter = NumberFormatter()
    
    formatter.numberStyle = .currency
    formatter.currencyCode = locale.currencyCode
    formatter.locale = locale
    
    currencyLabel.text = formatter.string(from: price)
}
```

## 숫자 지역화 하기!
```swift
func numbers() {
    let quantity = NumberFormatter.localizedString(from: 5000, number: .decimal)
    
    numberLabel.text = String.localizedStringWithFormat(quantity)
 }
```

## 날짜 지역화 하기!
```swift
func date() {
    let date = DateFormatter.localizedString(from: Date(), dateStyle: .long, timeStyle: .short)
    
    dateLabel.text = date
}
```

## 참고한 문서및 자료
https://green1229.tistory.com/72
https://babbab2.tistory.com/59
https://zeddios.tistory.com/368
