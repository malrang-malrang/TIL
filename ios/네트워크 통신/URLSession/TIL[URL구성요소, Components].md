# Today I Learned.

## 학습내용
# URL구성요소, Components

## URL을 구성하는 요소
```swift
- let urlString = "https://ios-development.tistory.com/search/users?id=123&age=20"
- let url = URL(string: urlString)

/// 주소 전체 "https://ios-development.tistory.com/search/users?id=123&age=20"
- print(url?.absoluteURL)
 
/// 어떤식으로 네트워킹 하는지 "https"
- print(url?.scheme)
 
/// baseURL과 같이 메인 주소 "ios-development.tistory.com"
- print(url?.host)
 
/// host뒤에 query parameter를 제외한 주소 "/search/users"
- print(url?.path)
 
/// query parameter 값 "id=123&age=20"
- print(url?.query)
 
/// 설정하지 않으면 디폴트는 nil
- print(url?.baseURL)

let baseURL = URL(string: "https://ios-development.tistory.com")
let relativeURL = URL(string: "/search/users?id=123&age=20", relativeTo: baseURL)
print(relativeURL?.absoluteURL)
print(relativeURL?.scheme)
print(relativeURL?.host)
print(relativeURL?.path)
print(relativeURL?.query)
print(relativeURL?.baseURL) 
// 이제 baseURL 확인 가능 "https://ios-development.tistory.com"
```

## URLComponents
>URLComponents를 사용하는 이유: URL을 사용하는 것보다는 URLComponents를 이용하면 query parameter부분을 URLQueryItem과 같은 객체로 쉽게 추가 가능
```swift
// https://ios-development.tistory.com/search/users?id=123&age=20
var urlComponents = URLComponents(string: "https://ios-development.tistory.com/search/users?")

// query 설정
let userIDQuery = URLQueryItem(name: "id", value: "123")
let ageQuery = URLQueryItem(name: "age", value: "20")
urlComponents?.queryItems?.append(userIDQuery)
urlComponents?.queryItems?.append(ageQuery)

/// 전체 경로 (문자열이 아닌 형태) https://ios-development.tistory.com/search/users?id=123&age=20
print(urlComponents?.url)
/// "https://ios-development.tistory.com/search/users?id=123&age=20"
print(urlComponents?.string)
/// [id=123, age=20]
print(urlComponents?.queryItems)
```
## URLSession과 URLSessionTask
URLSessionConfiguration 생성 > URLSession 생성 > URLSession과 URLComponents를 이용하여 URLSessionDataTask 생성 > dataTask를 resume()하여 네트워크 통신
```swift
// URLSessionConfiguration 생성 (세 가지 존재): .default / .ephemeral / .background
let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

// URLComponents를 생성하여 query 설정
var urlComponents = URLComponents(string: "https://ios-development.tistory.com/search/users?")
let userIDQuery = URLQueryItem(name: "id", value: "123")
let ageQuery = URLQueryItem(name: "age", value: "20")
urlComponents?.queryItems?.append(userIDQuery)
urlComponents?.queryItems?.append(ageQuery)

// URLComponents와 URLSession을 이용하여 URLSessionDataTask 생성
guard let requestURL = urlComponents?.url else { return }
let dataTask = session.dataTask(with: requestURL) { (data, response, error) in

    // error가 존재하면 종료
    guard error == nil else { return }

    // status 코드가 200번대여야 성공적인 네트워크라 판단
    let successsRange = 200..<300
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode,
          successsRange.contains(statusCode) else { return }

    // response 데이터 획득, utf8인코딩을 통해 string형태로 변환
    guard let resultData = data else { return }
    let resultString = String(data: resultData, encoding: .utf8)
    print(resultData)
    print(resultString)
}

// network 통신 실행
dataTask.resume()
```

# URL, URI, path, query string 쿼리추가및 파싱하는법

## URL에 parameter 삽입 (query string)
- "https://domainABC" 를 "https://domainABC?memberID=1234"로 변경

```swift
let url = "https://domainABC"
var components = URLComponents(string: url)
let id = URLQueryItem(name: "memberID", value: "1234")
components?.queryItems = [id]

guard let newURL = components?.url else {
    return
}
print(newURL) // Optional(https://domainABC?memberID=1234)
```
## URL의 parameter 파싱 (query string)
- "https://domainABC?memberID=1234"를 memberID 데이터만 획득

```swift
let url = "https://domainABC?memberID=1234"
let components = URLComponents(string: url)
let items = components?.queryItems ?? []
for item in items {
    print("\(item.name), \(item.value)") // memberID, Optional("1234")
}
```
## URL의 path에 "{}" 형식으로 내려오는 url 파싱
- "https://domainABC/{id}/{scope}"를 id, scope값에 각각 값을 채워서 삽입

```swift
let url = "https://domainABC/{id}/{scope}/"
let template = URITemplate(template: url)
let newURL = template.expand(["id": "123", "scope": "inHouse"])
print(newURL) // https://domainABC/123/inHouse/
print(template.variables) // ["id", "scope"]
```
