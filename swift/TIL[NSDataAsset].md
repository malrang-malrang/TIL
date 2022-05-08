# Today I Learned.

# Assets 에서 Json 파일 읽기

>프로젝트에서 Assets 에 포함된 Json 파일에 접근하기 위해서는 `NSDataAsset` 을 사용할수 있다.

## NSDataAsset
>공식 문서의 선언 부를 보자!
```swift
class NSDataAsset: NSObject
```
>요녀석을 사용하려면 UIKit 을 import 해줘야 한다!
>이녀석을 사용하면 JSON 파일을 NSData 타입으로 변환 해준다.
>
**사용 방법**
```swift
static func parse( name: String) -> Self? {
    guard let asset = NSDataAsset(name: name) else {
        return nil
    }
    let jsonData = try? JSONDecoder().decode(Self.self, from: asset.data)

    return jsonData
    }
```
>**`NSDataAsset` 을 사용한 부분을 보자.**
>`Assets` 의 `Json` 을 디코딩 해주는 함수를 구현해보았다.
> 파라미터 부분에는 `Assets` 에 있는 `Json` 파일의 이름을 작성해주면된다!
>`String` 타입으로 작성해주어야 한다!
>`NSDataAsset` 으로 변환에 실패할수도 있으니 `guard` 를 사용해 바인딩 을 해주었다.
>이렇게 `NSDataAsset` 타입으로 변환된 `Json` 을 다시 `data` 타입으로 변환 하여 `JsonDecoder` 를 사용하면 해당 `Json` 파일에 접근하여 사용할수 있게된다.

**사용예시**
```swift
//JSON 파일을 디코딩 해주는 메서드
extension Decodable {
    static func parse(_ name: String) -> Self? {
        guard let asset = NSDataAsset(name: name) else {
            return nil
        }
        let jsonData = try? JSONDecoder().decode(Self.self, from: asset.data)

        return jsonData
    }
}

//JSON 파일과 매칭될 녀석
struct ExpositionPoster: Codable {
    let title: String?
    let visitors: Int?
    let location: String?
    let duration: String?
    let description: String?
}
//사용 예시
let malrang = ExpositionPoster.parse("exposition_universelle_1900")
        malrang?.title
```
>이렇게 `Assets` 에 저장된 `JSON` 을 `Decoding` 하여 사용할수 있게된다.

## 참고문서
[공식문서](https://developer.apple.com/documentation/uikit/nsdataasset)
