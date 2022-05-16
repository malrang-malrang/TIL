# Today I Learned.

## 학습내용
# URLsession, URLSessionTask

## URLsession
>![](https://i.imgur.com/6VSYt0q.png)
>
>네트워크 데이터 전송 작업과 관련된 그룹을 조정하는 객체다.
>
>### Declaration
>```swift
>class URLSession : NSObject
>```
>
>### OverView
>`URLSession` 클래스 및 관련 클래스는 URL 로 표시된 엔드 포인터에서 데이터를 다운로드 하고 데이터를 업로드하기 위한 `API` 를 제공한다.
>
>또한 앱은 API를 사용하여 앱이 앱이 실행되고 있지 않거나 IOS 에서 앱이 정지된 상태에서 백그라운드 다운로드를 수행할수 있다.
>
>`URLSessionDelegate`, `URLSessionTaskDelegate` 를 사용하여 인증을 지원하고 리다이렉션, 작업 완료와 같은 이벤트를 수신할 수 있다.
>
>앱은 각자 데이터 전송 작업과 관련된 그룹을 조정하는 하나 이상의 `URLSession` 인스턴스를 생성한다.
>
>예를 들어 웹 브라우저를 만드는 경우 앱은 탭 또는 윈도우당 하나의 세션을 만들거나 대화형 사용을 위한 세션과 백그라운드 다운로드를 위한 세션을 만들수 있다.
>
>각 세션 내에서 앱은 특정 URL 에 대한 요청을 나타내는 작업을 추가한다.
>
>### Types of URL Sessions
>주어진 URL 세션 내의 작업은 단일 호스트에 대한 최대의 동시 연결수, 셀룰러 네트워크를 사용할 수 있는지에 대한 여부와 같은 네트워크를 정의하는 공통 세션 구성 객체를 공유한다.
>
>`URLSession` 에는 기본 요청에 대한 단일 공유 세션이 있다.
>직접만든 세션만큼 맞춤 설정을 할 수는 없지만 요구 사항이 매우 제한적인 경우 좋은 출발점 역할을 한다.
>
>공유 클래스 메서드를 호출하여 이 세션에 접근한다.
>
>**다른 종류의 세션의 경우 다음 세가지 구성 중 하나를 사용하여 URLSession 을 만든다.**
>- **기본세션(Default Session)**:
>   - 기본세션은 공유 세션과 유사하게 작동하지만 직접 만들수 있다.
>   - 데이터를 점진적으로 가져오기 위해 기본 세션에 델리게이트를 할당할 수도 있다.
>- **임시세션(Ephemeral Session)**:
>   - 임시세션은 공유 세션과 비슷하지만 캐시, 쿠키, 사용자 인증 정보를 디스크에 쓰지 않는다.
>- **백그라운드 세션(Background Session)**:
>   - 백그라운드 세션을 사용하면 앱이 실행되지 않는 동안 백그라운드에서 콘텐츠 업로드 및 다운로드를 수행할 수 있다.
>
>### Types of URL Session Tasks
>세션 내에서 선택적으로 데이터를 서버에 업로드 한 뒤 디스크의 파일 또는 메모리의 NSData 객체로 서버에서 데이터를 검색 하는 작업을 생성한다.
>URLSession API 는 네가지 유형의 작업을 제공한다.
>- **Data tasks**:
>   - 데이터 작업은 NSData 객체를 사용하여 데이터를 보내고 받는다.
>   - 데이터 작업은 서버에 대한 짧은 대화형 요청을 위한 것이다.
>- **Upload tasks**:
>   - 업로드 작업은 데이터를 전송하고 앱이 실행되지 않는동안 백그라운드 업로드를 지원한다.
>- **Download tasks**: 
>   - 다운로드 작업은 파일 형식으로 데이터를 검색하고 앱이 실행되지 않는 동안 백그라운드 다운로드 및 업로드를 지원한다.
>- **WebSocket**:  
>   - 웹소켓 작업은 RFC 6455 에 정의된 WebSocket 프로토콜을 사용하여 TCP, TLS 를 통해 메세지를 교환한다.
>   
>
>### Using a Session Delegate
>세션의 작업은 델리게이트 객체를 공유한다.
>다음과 같은 경우를 포함하여 다양한 이벤트가 발생할때 정보를 제공하고 얻기위해 델리게이트를 구현한다.
>
>- 인증에 실패했을때
>- 데이터가 서버에서 도착했을때
>- 데이터를 캐싱할수 있게 되었을때
>  델리게이트가 제공하는 기능이 필요하지 않은 경우 세션을 만들때 nil 을 전달하여 API 를 제공하지 않고도 API 를 사용할수있다.
>
>세션 객체는 앱이 종료되거나 세션을 명시적으로 무효화 할때까지 델리게이트에 대한 강한 참조를 유지한다.
>세션을 무효화 하지않으면 앱이 종료될 때 까지 앱에서 메모리가 누출된다.
>
>### Asynchronicity and URL Sessions
>대부분의 네트워킹 APU와 마찬가지로 URLSession API 는 비동기적이다.
>호출하는 메서드에 따라 다음 두가지 방법중 하나로 앱에 데이터를 반환한다.
>
>1. 전송이 성공적으로 완료되거나 오류가 있는경우 완료 핸들러 블록을 호출한다.
>2. 데이터가 도착하고 전송이 완료되면 세션의 대리자에서 메서드를 호출한다.
>
>이 정보를 델리게이트에 전달하는것 외에도 URLSession은 작업의 현재 상태를 기반으로 프로그래밍 방식으로 결정을 내려야하는경우 쿼리 할 수 있는 상태 및 진행률 프로퍼티를 제공한다.
>
>### Protocol Support
>URLSession 클래스는 기본적으로 데이터, 파일 ftp, http, http URL 체계를 지원하며 사용자의 시스템 기본 설정에 구성된 대로 프록시 서버 및 SOCKS 게이트웨이를 투명하게 지원한다.
>
>URLSession은 HTTP/1.1, HTTP/2 프로토콜을 지원한다.
>HTTP/2 지원에는 ALPN(Application-Layer Protocol Negotiation)을 지원하는 서버가 필요하다.
>
>또한 URLProtocol 을 서브 클래싱하여 자체 맞춤 네트워킹 프로토콜 및 URL 스키마에 대한 지원을 추가할 수 있다.
>
>### App Transport Security (ATS)
>IOS 9.0, macOS 10.11 이상은 URLSession 으로 이루어진 모든 HTTP 연결에 ATS(App Transport Security) 를 제공한다.
>ATS 는 HTTP 연결에서 HTTPS 를 사용해야 한다.
>
>### Foundation Copying Behavior
>세션과 작업 객체는 다음과 같이 NSCopying 프로토콜을 준수한다.
>1. 앱이 세션 또는 작업 객체를 복사하면 동일한 객체를 다시 가져온다.
>2. 앱이 구성 객체를 독립적으로 수정할 수 있는 새로운 사본이 생성된다.
>
>### Thread Safety
>URL 세션 API는 스레드로부터 안전하다.
>모든 스레드 컨텍스트에서 세션과 작업을 자유롭게 만들수 있다.
>델리게이트 메서드가 제공된 완료 처리기를 호출하면 작업이 올바른 델리게이트 큐에 자동으로 스케쥴링된다.

## URLSessionTask
>![](https://i.imgur.com/0dTNTqR.png)
>
>### Declaration
>```swift
>class URLSessionTask : NSObject
>```
>URLSession으로 다운로드나 resource 관련한 작업들을 처리하는 모듈이다.
>
>**메서드 종류**
>
>**1. `cancel()`**
>- task 를 중지
>
>**2. `resume()`**
>- task 가 일시중지 되어있던경우 다시시작
>
>**3. `suspend()`**
>- task 를 일시중지(인스턴스 생성시 초기값이다)
>
>**URLSessionDataTask 의 상속관계**
>![](https://i.imgur.com/zZ7RvxJ.png)
>
>
>### URLSessionDataTask
>![](https://i.imgur.com/Q1RV2qc.png)
>
>#### Declaration
>```swift
>class URLSessionDataTask : URLSessionTask
>```
>서버로 부터 POST, PUT 메서드를 통해 웹서버로 파일을 전송하는 경우 사용한다.
>
>
>### URLSessionUploadTask
>![](https://i.imgur.com/XajSSTm.png)
>
>#### Declaration
>```swift
>class URLSessionUploadTask : URLSessionDataTask
>```
>서버에서 데이터, 파일 을 다운로드 할경우 사용한다.
>
>## URLSessionDownloadTask
>![](https://i.imgur.com/xLKL83j.png)
>
>#### Declaration
>```swift
>class URLSessionDownloadTask : URLSessionTask
>```

##  URLsession 을 이용해 네트워크 통신하기
>**URLSession**: 
>- HTTP 를 포함한 여러가지 포로토콜을 지원하고, 인증, 쿠키, 캐시 등의 관리를 지원한다.
>- Request 와 Response 를 기본 구조로 갖는다.
>
>**Request**: 
>- 요청에대한 정보를 표현하는 객체, 이 객체를 URLSession 을 사용하여 서버로 요청을 보낸다.
>- URL 객체를 통해 직접 통신하는 형태와, URLRequest 객체를 만들어 옵션을 설정하여 통신하는 형태가 있다.
>
>**Response**:
>- 설정된 Task 의 Completion Handler 형태로 response 를 받거나, URLSessionDelegate 를 통해 지정된 메서드를 호출하여 response를 받는 형태가 있다.
>
>**URLSession 의 사이클**
>1. Session configuration을 결정하고, Session을 생성한다.
>2. 통신할 URL과 Request 객체를 설정한다.
>3. 사용할 Task를 결정하고, 그에 맞는 Completion Handler나 Delegate 메소드들을 작성한다.
>4. 해당 Task를 실행한다.
>5. Task 완료 후 Completion Handler가 실행된다.
>
>## URLSessionConfiguration을 통해 URL 생성
>- .default: 기본 통신을 할때 사용 (쿠키와 같은 저장 객체 사용)
>- .ephemeral: 쿠키나 캐시를 저장하지 않는 정책을 사용할 때 이용
>- .background: 앱이 백그라운드 상태에 있을 때 컨텐츠를 다운로드/업로드
>
>## URLSessionTask 작업 유형
>- URLSessionDataTask: 기본적인 데이터를 받는 경우, response데이터를 메모리 상>에서 처리
>- URLSessionUploadTask: 파일 업로드 시 사용, 사용하기 편한 request body 제공
>- URLSessionDownloadTask: 실제 파일을 다운받아 디스크에 사용될때 사용

## 참고한 문서
[URLSession](https://developer.apple.com/documentation/foundation/urlsession)
