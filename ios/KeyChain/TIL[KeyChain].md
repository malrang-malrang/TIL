# Today I Learned.

## 학습내용
# KeyChain
>![](https://i.imgur.com/NgMl4TW.png)
>
>사용자를 대신하여 작은 데이터 덩어리를 안전하게 저장하는 서비스.
>>Apple에서 개발한 비밀번호 관리 시스템.
>디바이스 안에 암호화된 데이터 저장공간을 의미한다.
>
>## OverView
>컴퓨터 사용자들은 종종 안전하게 저장이 필요한 작은 비밀을 가지고 있습니다.
>
>예를 들어 많은 사람들은 많은 온라인 계정을 관리합니다.
>
>각 계정에 대해 복잡하고 고유한 암호를 기억하는 것은 불가능하지만 그것들을 적어 두는 것은 불안하고 귀찮습니다.
>
>사용자들은 일반적으로 많은 계정에서 간단한 비밀번호를 재활용함으로써 이러한 상황을 해결하는데 이 또한 안전한 방법이 아닙니다.
>
>키체인 서비스 API는 키체인이라고 불리는 암호화된 데이터베이스에 사용자 정보의 작은 비트을 저장할 수 있는 메커니즘을 제공함으로써 이 문제를 해결 할 수 있도록 도와줍니다.
>
> 암호를 안전하게 저장하면, 사용자는 복잡한 암호를 선택할 수 있습니다.
>
>키체인은 비밀번호에 국한되어 있지 않습니다. 사용자가 걱정하는 신용카드 정보나 작은 메모같은 다른 정보도 저장할 수 있습니다.
>
> 또한 사용자에게 필요하지만, 알 필요가 없는 정보를 저장할 수도 있습니다.
> 
> 예를들어 Certificate, Key, and Trust Services로 관리하는 암호키, 인증서는 사용자가 안전하게 통신할 수 있도록 하고, 다른 유저나 기기와의 신뢰를 보장할 수 있도록 합니다.
> 
> ![](https://i.imgur.com/TX6B8Hh.png)
>
>## KeyChain 의 구성요소
>1. KeyChain Items: 키체인에 저장되는 데이터
>2. ItemClass: 아이템에 저장되는 데이터 종류(인터넷 패스워드, 제네릭패스워드 등등)
>3. Attribute: 아이템 클래스에 대한 속성값
>
>## KeyCahin 을 사용하는 이유
>UserDefault 에 저장하면 되는데 왜 키체인을 사용해야할까??
>
>UserDefault 에 저장되는 정보들은 base64 인코딩되어 plist 형식으로 저장되며 이러한 정보는 유출되기 쉽다.
>
>게다가 UserDefault 는 앱이 삭제됨과 동시에 정보가 삭제되지만 키체인은 정보를 직접 살제하기전까지 삭제되지 않는다.
>
>따라서 보안이 필요하거나 앱이 삭제되어도 유지되어야하는 정보는 키체인을 사용하는것이 적절하다.
>
>비밀번호, 개인 키, 인증서 및 보안 메모 등등 에 사용하는것이 적절하다.
>
>### 키체인의 특징
>1. 사용자가 직접 제거하지 않으면 앱을 제거하고 설치해도 데이터는 남아있다.
>2. 디바이스를 Lock 하면 KeyChain 도 잠기게되고 디바이스를 UnLock 하면 KeyChain 도 풀리게 된다.
>3. 같은 개발자가 개발한 여러 앱에서 키체인 정보를 공유할수 있다.
>
>## 유저의 데이터를 저장하는 다른것들과 다른 점은 무엇일까?
>**유저 데이터를 저장하는 방법들 에는 아래와 같은 것들이 있다.**
>**1. Keyed Archiver**: 
>- coreData보다 덜 복잡하고 느리지만 사용이 간단하다. 저장할 데이터가 NSConfig 프로토콜을 채택해야하며 데이터를 저장할 시 key/value 값으로 인코딩을하고 꺼낼때 디코딩을 이용한다.
>
>**2. CoreData**: 
>- UserDefault보다 더 복잡하지만 저장된 정보에 구조가 필요할 때 유용하다.대용량 데이터 저장에 사용되는 관계형 DB, 간단한 UI에서 자동으로 모델을 생성.
>
>**3. User Defaults**:
>- 데이터를 저장 및 유지하는 가장 간단한 방법으로 데이터를 구조화하지 않는다.
>
>### 위의 방법들과 KeyCahin의 차이점
>1. User Defaults는 info.plist에 데이터(패스워드 등)가 저장되기에 앱이 삭제되면 정보도 삭제되지만 키 체인은 별도의 키 체인에 데이터를 저장함으로 앱이 삭제되도 키 체인을 삭제해주기전에는 삭제되지 않는다. 
탈옥하면 info 에 저장된 값을 볼수 있어 보안에 취약하다.
>
>2. 키 체인 그룹을 사용하면 다른 앱에서도 데이터 공유 가능
>
>3. 키 체인은 가장 안전하고 민감한 데이터를 저장 및 유지할때 사용하지만 가장 느리다.
>
>### iOS의 키체인과 macOS의 키체인의 차이점
>IOS 는 iCloud 키체인을 포함하는 단일 키체인에 접근합니다. 앱은 자신의 item이나 자신이 속한 그룹에서 공유하는 항목에만 접근할 수 있다.(사용자)
>
>Mac OS 는 여러개의 키체인을 지원한다. 일반적으로 애플이 제공하는 Keychain Access(키체인 접근) 앱에서 키체인을 관리하고 직접 조작할 수 있습니다.(사용자, 시스템, 앱)
>키체인    | 특징
>--------|-----------
>ios     | Single Keychain
>macOs   | arvitrary number of keychains</br>키체인을 직접 조작하는 데 사용할 수 있는 기능을 제공
>
>
>## 키체인 서비스를 사용하기 위한 k- 접두어를 사용하는 여러 타입과 값들
>### k- 접두어의 의미
>
>Constant(상수) 변하지 않는 값을 의미한다.
>
>NOTE: Previous convention was for public constant names to begin with a lowercase k followed by a project-specific prefix. This practice is no longer recommended.
>
> 참고: 이전 규칙은 공용 상수 이름은 소문자 k로 시작하고 프로젝트별 접두사가 붙는 것이었습니다. 이 방법은 더 이상 권장되지 않습니다.
> 
>Constant 를 K 로 표현한다고 objcguide 가이드에 나와있다.
>[objc가이드]>(https://google.github.io/styleguide/objcguide.html)
>
>### KeyChain에서 K접두어가 붙은 값은 어떤 프레임워크의 값일까?
>**CoreFoundation**
> - 프로퍼티가 CFString 타입이고, 해당 타입은 Core Foundation에 포함된 타입
> - Low-Level functions으로 원시 데이터 타입에 접근하여 Foundation 프레임워크와 원활하게 연동시켜주는 기능을 한다.
> 
>### CoreFoundation과 Foundation 프레임워크와의 차이는?
>Foundation 이 CoreFoudation 을 포함한다.
>
>![](https://i.imgur.com/YwlhCDj.png)
>
>**CoreFoundation:** 
>- Foundation 프레임워크와 원활하게 연결된 저수준 함수, 기본 데이터 유형 및 다양한 컬렉션 유형에 액세스
>- 애플리케이션 서비스, 애플리케이션 환경, 애플리케이션 자체에 유용한 기본적인 소프트웨어 서비스를 제공하는 프레임워크
>
>**Foundation:** 
>- 필수 데이터 유형, 컬렉션 및 운영 체제 서비스에 액세스하여 앱의 기본 기능 계층을 정의
>- 데이터 저장 및 지속성, 텍스트 처리, 날짜 및 시간 계산, 정렬 및 필터링, 네트워킹을 포함하여 앱 및 프레임워크를 위한 기본 기능 계층을 제공
>
>좀더 로우한것? 이라고 이해했다.(다루기는 좀더 어렵지만 성능에 이점을 두는)
>
>하드웨어에 좀더 관련이있는 프레임워크로 코코아 환경에서 Foundation 과 같은 계층에 속하지만 CoreFoundation 을 Foundation 이 래핑한 형태다. 

# Keychain Items
>![](https://i.imgur.com/SnUnaLK.png)
>
>키 체인에 저장하는 항목에 기밀 정보를 포함시킵니다. ????
>키체인에 저장할 정보, 속성을 포함하는 놈이다.
>
>## OverView
>키체인에 정보를 저장할때는 키체인 아이템으로 패키징 해야하며, 키체인 아이템은 Data(정보), Attribute(속성)로 구성되어있다.
>
>키체인 서비스 API 는 아이템을 삽입하기 전에 정보를 암호화하고 속성과 함께 패키징한다.
>
>정보는 저장하고자 하는 Data 이고 속성(Attribute)를 사용해 정보를 식별하고 저장하거나 저장된 항목에 대한 접근을 제어 한다.
>
>구현을 위해서는 Security 프레임워크가 필요하다.
>키체인 아이템은 CFDictionary 를 사용해 접근 할 수 있다.
>
> ![](https://i.imgur.com/WKBAqG1.png)
> 
>## 키 체인 클래스
>- kSecClass: 키체인 아이템의 타입
>   - kSecClassGenericPassword
>   - kSecClassInternetPassword
>   - kSecClassCertificate
>   - kSecClassKey
>   - kSecClassIdentity
>   
> ## 속성
>- kSecAttrService: 키체인 아이템과 연관되어 있는 서비스의 이름
>   - kSecClassGenericPassword 타입에서 사용
>- kSecAttrAccount: 저장할 아이템의 계정 이름(아이디)
>   - kSecClassGenericPassword, kSecClassInternetPassword 타입에서 사용
>- kSecAttrGeneric: 저장할 아이템의 데이터(비밀번호)
>   - kSecClassGenericPassword 타입에서 사용
>- kSecMatchLimit: 키의 값을 저장할 경우, 이 값은 반환하거나 다른 조치를 취할 최대 결과 수를 지정
>   - kSecMatchLimitOne
>   - kSecMatchLimitAll
>- kSecReturnAttributes: 키의 값으로 true를 저장할 경우, CFDictionary 타입으로 반환
>- kSecReturnData: 키의 값으로 true를 저장할 경우, CFData 타입으로 반환
>
>등등이 있으며 필요에따라 사용하면된다.
