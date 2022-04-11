# Today I Learned.

## 학습내용
# Json 제이슨..!누구냐 넌?

## Json(JavaScript Object Notation)
>메모리에서 살고있는 객체를 디스크에 저장했다가 다음에 다시 사용한다던지, 다른 컴퓨터 다른 시스템으로 전달을 하고 싶을때 사용할수있다.
>
>메모리에서 살고있는 객체는 0,1 로 이루어져 있다.
>읽고 쓰는 방식이 컴퓨터 마다 다르다. 그렇기에 0,1 그대로를 usb 등 매개체에 담아 다른곳에 보내게 되면 제대로 읽고 쓸수 없게될 확률이 높다.
>
>이때 Json 형식으로 저장하여 옮기게되면 다른 컴퓨터 혹은 시스템에서 내가 만들어둔 객체를 다른곳에서도 동일하게 해석하여 내가 만들어둔 객체와 동일하게 작동하도록 해준다.
>
>쉽게 얘기하면 컴퓨터와 컴퓨터가 0,1 을 동일하게 해석할수있게끔 하는 약속이 필요했다.

## Json 의 규칙들
>**1. { }: 객체 (딕셔너리)**
>**2. [ ]: 배열**
>**3. " ": 문자열**
>**4. 문자열 외: 숫자**
>**5. key 값은 String 만!**
>**6. value 값은 String, 숫자, 논리값, 배열, Json객체, Null**
>

## 예시 
**위의 규칙들을 살펴보자**
>**1. { }**
>- 객체가 시작되고 끝남을 의미한다.
```
{
    "이름": "홍길동"
    "나이": 150
    "성별": "남"
}
```
>**2. [ ]**
>- 평소 사용하던 배열과 똑같다.
>- 아래의 예시처럼 1개이상의 객체를 넣어주거나 문자열, 숫자, 논리값, Json배열, Json객체, Null 값을 배열 내부에 넣을수있다.
```
[
    {
        "이름": "홍길동"
        "나이": 150
        "성별": "남"
    },
    {
        "이름": "홍길동"
        "나이": 150
        "성별": "남"
    }
]
```
## Codable
>Swift4 에 추가된 프로토콜 이다.
>JSON 데이터를 Encoding, Decoding 할 수 있게 해준다.

**Codable 의 정의**
```swift
public typealias Codable = Decodable & Encodable
```
>Codable은 Decodable 과 Encodable 로 이루어져있다.
>Decodable, Encodable 얘내 둘다 프로토콜인데 합쳐진놈 으로 생각하자.

## Json을 Encoding 해보자!
>원하는 Struct, class, enum 등 의 인스턴스를 Json 형태의 Data로 만들어주는것이다.
>
>이때 JSON Data 로 변환할 객체의 타입이 Codable 프로토콜을 채택 하게되면 쉽게 Encoding 이 가능하다.
>
>JSONEncoder 클래스에 encode 메서드를 호출해서 객체를 넣어주면된다!
>
>Encoding 중 에러가 발생할수 있으니 try 와 같이 사용해야한다!
```swift
struct Json: Codable {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

let malrang = Json(name: "malrang", age: 29)
let data = try? JSONEncoder().encode(malrang)
```
## Json을 Decoding 해보자!
>JSON 형태의 Data 를 struct, class, enum 등의 인스턴스에 파싱해주는 것 이다.
>
>JSONEncoder 클래스에 decode 메서드를 호출해서 객체를 넣어주면된다!
>
>Decoding 중 에러가 발생할수 있으니 try 와 같이 사용해야한다!
>

```swift
struct Json: Codable {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

let data = """
{
    "name": "malrnag",
    "age": 29
}
""".data(using: .utf8)!

let malrnag = try? JSONDecoder().decode(Json.self, from: data)
```
>위의 예시에서 data 상수는 임의의 전달받은 JSON 타입이라고 가정해보자.
>
>data 를 decode 할때에는 어떤 타입으로 파싱 할것인지 타입을 작성해준후 어떤 data 를 파싱할것인지 넣어주면 된다!
>
>**작동방식**
>1. Json 타입의 구조체를 만들고
>2. data 의 Key 값과 동일한 이름의 구조체 변수에 value 값을 할당한다.
>3. 그후 할당한 구조체를 반환한다.
>4. 그렇기 때문에 변환할 Codable 을 채택한 구조체 혹은 다른 타입 의 프로퍼티 이름과 똑같아야만 문제 없이 사용할 수 있다.

## 항상 전달받은 JSON Data 의 키값과 동일하게 변수명을 사용해야하나?
> 기본적으로는 JSON Data 의 키값과 동일하지 않을 경우 매칭이 되지않아 Decoding 과정에서 실패하게 된다.
> 그렇기 때문에 Key 값에 대응하는 이름을 바꿔주고 싶다면 CodingKey 프로토콜을 이용해서 이름이 바뀌었다는걸 명시해줘야 한다.
### CodingKey
> 사용법은 간단하다 매칭시킬 타입 내부에 enum 으로 CodingKey 프로토콜을 채택하게 해준후 JSON Data 의 이름을 다른이름으로 받겠다고 정의 해주어야한다.
```swift
struct Json: Codable {
    var name: String
    var age: Int

    enum CodingKeys: String, CodingKey {
        case name = "Name"
        case age = "age"
    }
}

let data = """
{
    "Name": "malrnag",
    "age": 29
}
""".data(using: .utf8)!

let malrnag = try? JSONDecoder().decode(Json.self, from: data)
```
>이때 변경할 이름 말고도 제이슨으로 매칭 될 프로퍼티 모두 enum 내부에 case 로 명시해주어야한다.
>위의 enum 에서 case age 처럼 말이다.(이거떄문에 왜 오류났는지 고생함)

## 특정 Key-Value 가 없는 경우, 혹은 Value 값이 NULL 인 경우
>서버에서 받은 JSON data 가 Key-Value 가 누락되어 전달받은 경우
>Decoding 과정에서 에러가 나게된다.
```swift
struct Json: Codable {
    var name: String
    var age: Int
}

let data = """
{
    "Name": "malrnag",
}
""".data(using: .utf8)!

let malrnag = try? JSONDecoder().decode(Json.self, from: data)
```
**변수를 옵셔널로 선언**
```swift
struct Json: Codable {
    var name: String
    var age: Int?
}
```

## 주의 사항!
> JSON 으로 인코딩 시에 데이터가 아닌 함수가 있을경우 JSON 으로 변환과정에서 제거됨! 변환된 JSON 파일에는 함수가 제거된 상태임!
