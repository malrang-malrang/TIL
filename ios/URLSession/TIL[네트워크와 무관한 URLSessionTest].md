# Today I Learned.

## 학습내용
# 네트워크와 무관한 URLSessionTest
## 작동원리
>1. URLSession 에 이미 정의되어있는 dataTask 메서드를 Mock 객체에서 새롭게 구현한다.
>2. 네트워크통신과 무관하게 무조건 성공하는 값을 주도록 한다.
>3. 테스트에 성공하는 값을 가지고 원하는 데이터로 파싱되었는지 확인해본다.

## 구현코드
### 1. 정상적으로 작동하는 네트워크 통신코드
```swift
enum NetworkError: Error {
    case unknownError
    case statusCodeError
    case decodeError
    case urlError
}

import Foundation

//Codable 채택하는 타입만 가능하다!
struct URLSessionProvider<T: Codable> {
    private let session: URLSessionProtocol
    
    init (session: URLSessionProtocol = URLSession.shared) {
        self.session = session
    }

    func fetchData(
        url: Endpoint,
        completionHandler: @escaping (Result<T, NetworkError>) -> Void
    ) {
        
        var request = URLRequest(url: url.url)
            request.httpMethod = "GET"

            getData(from: request, completionHandler: completionHandler)
        }

    func getData(
        from urlRequest: URLRequest,
        completionHandler: @escaping (Result<T, NetworkError>) -> Void
    ) {
        let task = session.dataTask(with: urlRequest) { data, urlResponse, error in
            
            guard error == nil else {
                completionHandler(.failure(.unknownError))
                return
            }
            
            guard let httpResponse = urlResponse as? HTTPURLResponse,
                  (200...299).contains(httpResponse.statusCode) else {
                completionHandler(.failure(.statusCodeError))
                return
            }
            
            guard let data = data else {
                completionHandler(.failure(.unknownError))
                return
            }
            
            guard let products = try? JSONDecoder().decode(T.self, from: data) else {
                completionHandler(.failure(.decodeError))
                return
            }
            
            completionHandler(.success(products))
        }
        task.resume()
    }
}
```

## 2. Mock 객체를 사용하기 위한 Protocol
```swift
typealias DataTaskCompletionHandler = (Data?, URLResponse?, Error?) -> Void

protocol URLSessionProtocol {
    func dataTask(with request: URLRequest,
                  completionHandler: @escaping DataTaskCompletionHandler) -> URLSessionDataTask
}

extension URLSession: URLSessionProtocol {}
```

## 3. Mock 객체 구현부
```swift
import UIKit

struct MockData {
    func load() -> Data? {
        guard let asset = NSDataAsset(name: "products") else {
            return nil
        }
        return asset.data
    }
}

class MockURLSessionDataTask: URLSessionDataTask {
    private let closure: () -> Void

    init(closure: @escaping () -> Void) {
        self.closure = closure
    }

    override func resume() {
        closure()
    }
}

class MockURLSession: URLSessionProtocol {

    func dataTask(
        with urlRequest: URLRequest,
        completionHandler: @escaping DataTaskCompletionHandler
    ) -> URLSessionDataTask {
        let successResponse = HTTPURLResponse(
            url: urlRequest.url!,
            statusCode: 200, httpVersion: "",
            headerFields: nil
        )

        return MockURLSessionDataTask { completionHandler(
            MockData().load(),
            successResponse, nil
        ) }
    }
}
```

## 4. Mock 객체 테스트 코드
```swift
import XCTest
@testable import OpenMarket

class MockURLSessionTest: XCTestCase {
    var sut: URLSessionProvider<ProductCatalog>!
    
    override func setUpWithError() throws {
        let session = MockURLSession()
        sut = URLSessionProvider<ProductCatalog>(session: session)
    }

    override func tearDownWithError() throws {
        sut = nil
    }
    
    func test_네트워크가_연결이_되지않아도_getData의_함수를_호출하면_Mock데이터의_itemspPerPage의_값이_20인지() {
        //given
        let promise = expectation(description: "The Value of items per page 20")
        let url = URL(string: "empty")!
        let requset = URLRequest(url: url)
        
        //when
        sut.getData(from: requset) { result in
            //then
            switch result {
            case .success(let data):
                XCTAssertEqual(data.itemspPerPage, 20)
            case .failure(_):
                XCTFail()
            }
            promise.fulfill()
        }
        wait(for: [promise], timeout: 10)
    }
    
    func test_네트워크가_연결이_되지않아도_getData의_함수를_호출하면_Mock데이터의_totalCount의_값이_10인지() {
        //given
        let promise = expectation(description: "The value of the totalCount is 10")
        let url = URL(string: "empty")!
        let requset = URLRequest(url: url)
        
        //when
        sut.getData(from: requset) { result in
            //then
            switch result {
            case .success(let data):
                XCTAssertEqual(data.totalCount, 10)
            case .failure(_):
                XCTFail()
            }
            promise.fulfill()
        }
        wait(for: [promise], timeout: 10)
    }
    
    func test_네트워크가_연결이_되지않아도_getData의_함수를_호출하면_Mock데이터의_pages의_첫번째값의_id값이_20인지() {
        //given
        let promise = expectation(description: "The first value of pages is 20")
        let url = URL(string: "empty")!
        let requset = URLRequest(url: url)
        
        //when
        sut.getData(from: requset) { result in
            //then
            switch result {
            case .success(let data):
                XCTAssertEqual(data.pages?.first?.id, 20)
            case .failure(_):
                XCTFail()
            }
            promise.fulfill()
        }
        wait(for: [promise], timeout: 10)
    }
}
```
