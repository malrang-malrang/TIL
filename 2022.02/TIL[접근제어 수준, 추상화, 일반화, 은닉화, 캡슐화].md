Today I Learned

## **접근제어(Access Control)**
접근제어란 코드끼리 상호작용을 할때 파일 또는 모듈 간에 접근을 제한 할수있는 기능이다.

객체지향 프로그래밍 패러다임에서 외부에서 보이지 않거나 접근하지 말아야 할 코드가 있다. 불필요한 접근으로 의도치 않은 결과를 초래하거나 꼭 필요한 부분만 제공해야하는데 전체 코드가 노출될 가능성이 있을때 접근제어를 통해 이를 방지 할수 있다.

이때 사용되는 개념이 은닉화, 추상화 이다.

### **모듈(Module)**
- import 를 통해 다른 모듈로부터 불러들일 수 있는 하나의 코드 묶음 단위
- Library, Framework, Application 등

### **소스파일(Source File)**
- 모듈 내에 포함된 각각의 Swift 소스 코드 파일


## **Access Levels**
Swift 에서는 5가지의 접근 레벨 을 제공한다.
open(가장 개방적) -> public -> internal(Default) ->
fileprivate -> private(가장 제한적)

### **Open, Public**
- 공통점
  - 모듈 외부에서 접근이 가능하다 
- 차이점
  - Open은 모듈 밖의 다른 모듈에서 상속 가능하다.(클래스)
  - Open은 내부의 프로퍼티나 메소드들이 다른 모듈에서 override가능하다.(재정의, 수정, 덮어쓰기? 라고 이해하면될까)

- 예시 UIKit 은 개발자들이 만들어 놓은 외부 모듈로 그중 UIViewController 는 접근레벨이 Open으로 되어있다. 그렇기 때문에 UIViewController 를 상속받는 여러 ViewController를 생성할수 있다.
- Open으로 선언되어 있기때문에 상속이 가능한것이며 상속받는 클래스에서 메소드 들을 override 할수 있게된다.

### **internal**
- internal 은 따로 접근제어를 선언해주지 않으면 기본으로 할당되는 접근제어 레벨 이다. 
- internal 은 같은 모듈 내에서는 어디서든 접근이 가능하고 클래스의 경우 어느곳에서도 해당 클래스를 상속받을수 있다.

### **filePrivate**
- filePrivate는 하나의 스위프트 파일(.swift) 내부에서만 접근이 가능한 접근제어 레벨이다.
- 같은 파일내에 2개의 타입이 정의되어있고 그중 하나의 타입이 다른하나의 타입에만 상속되어 사용되는경우 타입을 filePrivate으로 선언하게되면 다른 swift파일 에서는 접근할수 없게된다.

### **private**
- private는 private가 선언된 영역 내에서만 접근이 가능하다.
```swift
class PrivateClass {
    private var name = "malrang"
    private var age = 0
}
let privateClass = PrivateClass()
private.name
//에러 발생! name에 접근할수 없다! name은 private으로 선언되어 있기 때문에
```

## 타입
대문자 카멜케이스
개체의 설계도

## 인스턴스
소문자 카멜케이스
설계도를 통해 만들어진 개체(실체화 되어 메모리에 할당됨)

## 추상화
복잡함속에서 필요한관점만 추출하는것.
- 지하철 노선도 에서 역과 역까지의 거리 등 복잡한 정보들은 알려주지 않고
  이용자가 보기쉽게 역의 위치, 인접한역 등 만을 나타낸것. 
  추상화 했다고 할수있다.

## 일반화
서로다른 개체들로부터 공통된 개념을 추출하는것.
- 사람,곰,고래 를 일반화 한다면 포유류라 할수있다.

## 은닉화
외부에서 특성과 기능에대해 접근할수없도록 접근을 제한해주는것.

## 캡슐화
불필요한 정보를 은닉하고 은닉한 특성,기능을 외부에서 소통(상호작용)할수있도록 창구를 만들어주는것.

### 추상화 일반화 예시
#### 개체: 세탁기
 특성
 종류
 색
 제조사
 제조번호
 가격

행위
 불리기
 세탁하기
 행구기
 탈수하기

```swift
class washer {
    var color = "white"
    let kinds = "drum"
    let manufacturer = "samsung"
    let serialNumber = "123-123-123"
    let price = 200000
    var laundry: [String]
    
    init(laundry: [String]) {
        self.laundry = laundry
    }
    
    func soakInWater() {
        laundry.forEach { print("빨랫감 \($0)을 물에 불립니다.")}
    }
    
    func wash() {
        laundry.forEach { print("빨랫감 \($0)을 세탁 합니다.")}
    }
    
    func rinseWithWater() {
        laundry.forEach { print("빨랫감 \($0)을 물에 행굽니다.")}
    }
    
    func dehydration() {
        laundry.forEach { print("빨랫감 \($0)을 탈수 합니다.")}
    }
}

var laundryLump = ["잠옷", "겨울옷", "여름옷"]
var washer1 = washer(laundry: laundryLump)
washer1.soakInWater()

```

### 은닉화 캡슐화 예시
```swift
class washer {
    private var color = "white"
    private let kinds = "drum"
    private let manufacturer = "samsung"
    private let serialNumber = "123-123-123"
    private let price = 200000
    private var laundry: [String]
    
    init(laundry: [String]) {
        self.laundry = laundry
    }
    
    func washLaundry() {
        soakInWater()
        wash()
        rinseWithWater()
        dehydration()
    }
    
    private func soakInWater() {
        laundry.forEach { print("빨랫감 \($0)을 물에 불립니다.")}
    }
    
    private func wash() {
        laundry.forEach { print("빨랫감 \($0)을 세탁 합니다.")}
    }
    
    private func rinseWithWater() {
        laundry.forEach { print("빨랫감 \($0)을 물에 행굽니다.")}
    }
    
    private func dehydration() {
        laundry.forEach { print("빨랫감 \($0)을 탈수 합니다.")}
    }
}

var laundryLump = ["잠옷", "겨울옷", "여름옷"]
var washer1 = washer(laundry: laundryLump)
washer1.washLaundry()
```
특성들을 외부에서 변경할수 없도록 private 접근제어를 해줌 = 은닉화
작동되는 모든 기능을 은닉화.
은닉화된 기능들을 washLaundry라는 메서드를 통해 작동될수 있도록 캡슐화함.

### 은닉화 캡슐화 예시2
```swift
class washer {
    private var color = "white"
    private let kinds = "drum"
    private let manufacturer = "samsung"
    private let serialNumber = "123-123-123"
    private var price = 200000
    private var laundry: [String]
    
    func breakWasher() {
        print("세탁기가 고장났습니다. 가격이 떨어집니다.")
        price -= 50000
        print("세탁기의 현재가격은 \(price) 입니다. 가격이 더떨어지기전에 중고로 팔아버립시다.")
    }
```
은닉화된 price 의 값을 내부에서 변경해줄수있도록 캡슐화 해줄수있다.
은닉화된값을 변경 할수있도록 소통창구를 열어준것 이라 볼수있다.
