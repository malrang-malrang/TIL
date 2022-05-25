# Today I Learned.

## 학습내용
# 비동기 메서드, 테스트를 위한 객체 만들기

## 비동기 메서드 테스트 하기
>비동기 메서드를 테스트하기위해서는 파생된 스레드에서의 작업을 기다리게 해주어야한다.
>
>기다리게 하지않으면 비동기적으로 실행되는 코드이기때문에 비동기 코드의 실행결과가 나오지 않았음에도 테스트에 통과했다고 처리될것이기 때문이다.
>
>아래의 3가지 테스트 메서드를 활용해 테스트를 할수있다.
>
>**1. `expectation(description:)`**
>   - 어떤것이 수행되어야하는지를 `description` 으로 정해준다.
>   
>**2. `fulfill()`**
>   - 정의해둔 `expectation`이 충족되는 시점에 호출하여 동작을 수행했음을 알린다.
>   
>**3. `wait(for: timeout:)`**
>   - `expectation` 을 배열로 담아 전달하여 배열속의 `expectation` 이 모두 `fulfill`될때 까지 기다린다.
>   - `timeout`을 설정하여 시간을 제한할수 있다.
>
>**예시 코드**
>```swift
>func test_makeRandomValue호출시_randomValue를_잘설정해주는지() {
>    // given
>    let promise = expectation(description: "It makes random value") // expectation
>    sut.randomValue = 50 // 기본값이 0~30에 포함되면 무조건 테스트에 통과하므로 범위에서 벗어난 값을 할당
>
>    // when
>    sut.makeRandomValue {
>        // then
>        XCTAssertGreaterThanOrEqual(self.sut.randomValue, 0)
>       XCTAssertLessThanOrEqual(self.sut.randomValue, 30)
>        promise.fulfill() // fulfill
>    }
>```
>
>**URLSessionTests 예시코드**
>```swift
>func test_URL요청시_상태코드200번이_돌아오는지() {
>        // given
>        let promise = expectation(description: "StatusCode is 200")
>       
>        // when
>       let task = sut.dataTask(with: url) { data, response, error in
>            guard let response = response as? HTTPURLResponse else {
>                return
>            }
>            
>            let statusCode = response.statusCode
>            // then
>            XCTAssertEqual(statusCode, 200)
>            promise.fulfill()
>        }
>        
>        task.resume()
>        wait(for: [promise], timeout: 10)
>    }
>```
>
## 테스트를 위한 객체 만들기
>네트워크 환경이 구축된 상태에서 테스트를 수행하게되면
>`FIRST`원칙중 Repeatable(반복 가능한)원칙에 위배된다.
>
> Repeatable 원칙: 테스트는 언제나 같은 결과가 반복되어야한다.
> 즉 모든 환경을 통제하여 매번 예상한 결과대로 테스트가 징행되게 해야한다.
> 
>네트워크를 통해 URL 에 접근해야 값을 얻어올수 있을때는 네트워크 환경을 통제할수 없기때문에 네트워크 없이 테스트를 할수있도록 해야한다.
>
>테스트를 위한 객체를 만들어 테스트 하면된다!
>
>### Test Double
>테스트 더블이란 테스트를 진행하기 어려운 경우 이를 대신하여 테스트를 진행할 수 있도록 만들어주는 객체를 말한다.
>
>테스트 하는 동안에 테스트 더블과 실체 객체를 바꿔치기해서 테스트를 진행하는것이다.
>
>**테스트 더블의 역할**
>테스트 대상 코드를 격리한다.
>테스트 속도를 개선한다.
>예측 불가능한 실행 요소를 제거한다.
>특수한 상황을 시뮬레이션 한다.
>감춰진 정보를 얻어낸다.
>
>**테스트 더블의 종류**
>
>**1. Dummy(모조의, 가짜의)**
>- Dummy는 가장 기본적인 테스트 더블이다.
>- 기능이 구현되어 있지 않으며, 인스턴스화된 객체로 사용된다.
>- 객체를 전달하기 위한 목적으로 주로 사용된다.
>
>**2. Stub(쓰다남은 물건의 토막, 남은부분)**
>- Stub는 Dummy가 실제로 동작하는 것 처럼 만들어 실제 코드를 대신해서 동작해주는 객체다.
>- 테스트가 곤란한 부분의 객체를 도려내어 역할을 최소한으로 대신해줄 만큼만 간단하게 구현한다.
>
>**3. Fake**
>- Fake는 Stub보다 구체적으로 동작해서 실제 로직처럼 보이지만 실제 앱의 동작에서는 적합하지 않은 객체를 말한다.
>- 로직 자체는 실제 앱의 코드와 비슷하지만 동작을 단순화하여 구현한 객체를 Fake라 한다.
>
>**4. Spy**
>- Spy는 Stub의 역할을 가지면서 호출된 내용에 대한 방법 혹은 과정 등 약간의 정보를 기록하는 객체다.
>- 호출되었는지 몇번 호출되었는지 등 에 대한 정보를 기록한다.
>
>**5. Mock**
>- 실제 객체와 가장 비슷하게 구현된 수준의 객체다.
>- Stub이 상태 기반 테스트(State Base Test)라면 Mock은 행위 기반 테스트(Behavior Base Test)라고 이야기 한다.
>- 상태 기반 테스트: 메서드를 호출하고 결과값과 예상 값을  비교하는 식으로 동작하는 테스트
>- 행위 기반 테스트: 예상되는 행위들에 대한 시나리오를 만들어 놓고, 시나리오대로 동작 했는지에 대한 여부를 확인하는 테스트
>
>### URLSession 이 동작하는 순서와 방식
>![](https://i.imgur.com/3xNeHz2.jpg)
>
>URLSession 은 URLSessionDataTask를 만들어 네트워킹을 통해 얻고자 하는 값을 얻어와서 completionHandler 를 실행해준다.
>
>### TestDouble 을 만들어 네트워킹 없이 completionHandler 를 실행하는 방법
>![](https://i.imgur.com/gRFVBox.jpg)
>
>1. Networking 방식으로 받아오지말도 직접 Data 를 만들어서 CompletionHandler 까지 전달
>2. 기존의 URLSession과 URLSessionDataTask 자리에 DummyData 를 전달할수 있는 Stub 객체 만들어서 바꿔치기
>
>Dummy, Stub 등의 TestDouble 을 활용해 미리 request에 대한 답을 설정하고 전달하는 식으로 테스트를 진행한다.
>
