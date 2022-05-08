# Today I Learned.

## 학습내용
# DispatchQueue의 초기화

## DispatchQueue의 초기화

>만들어져있는 `DispatchQueue.main` 와`DispatchQueue.global()` 말고도 `DispatchQueue`를 커스텀하여 사용할수 있다.

## DispatchQueue 초기화 메서드
>```swift
>convenience init(label: String,
>                 qos: DispatchQoS = .unspecified,
>                 attributes: >DispatchQueue.Attributes = [],
>                 autoreleaseFrequency: >DispatchQueue.AutoreleaseFrequency = .inherit,
>                 target: DispatchQueue? = nil)
>```
> `label` 을 제외한 파라미터는 기본값이 설정되어있기 때문에 커스텀에 필요한 파라미터만 초기화를 해줄수있다.

### label
>`DispatchQueue` 의 `label` 을 설정해주는 파라미터.
>
>커스텀 `DispatchQueue` 를 생성할때 필수다.
>
>`label` 은 디버깅환경에서 추적하기 위한 `String` 값이다.
>
>식별자 라고 생각하면 될것같다.
>```swift
>let myDispatchQueue = DispatchQueue(label: >"malrang")
>```

### qos
>`qos` 는 `DispatchQos` 타입의 값을 받는 파라미터다.
>
>`Qos` 는 `Quality of Service` 의 약자로 실행될 `Task` 의 우선순위를 정해주는 값이다.
>
>여기서 우선순위는 무엇을 먼저 처리할까? 의 우선도가 아니다.
>
>무엇에 더 많은 에너지를 쏟을까?? 를 뜻한다.
>
>100 의 에너지로 두가지 일을 동시에 처리한다고 할 때, 70, 30 처럼 에너지를 소모해서 처리하도록 할수있다.
>
>GCD 에서 스레드 관리는 시스템이 해준다고했다. 그렇기 때문에 Qos로 우선 순위를 결정하는 일은 꼭 정량적인 수치나 절대적인 값을 할당하는 개념이 아니다.
>
>초기화 구문 뒤에 작성되는`async`, `sync`의 파라미터로 `DispatchQos` 우선순위를 할당할수 있다.
>
>시스템은 Qos 정보를 통해 스케쥴링, CPU 및 I/O 처리량, 타이머 대기 시간 등의 우선순위를 조정한다.
>
>`DispatchQos`는 열거형 타입으로, 총 6개의 Qos 클래스가 있으며 4개의 주요 유형과 2개의 특수유형 으로 구분이 가능하다.
>
>우선순위가 높을수롣 더 많은 전력을 소모한다.
>그러므로 수행 작업에 적절한 Qos를 할당하면 앱이더 반응적이고 효율적인 에너지 사용이 가능해진다.
>
#### Qos 우선순위의 종류(우선순위가 높은순서대로 나열)
>**1. User-interactive**
>main thread 에서 작업하며, 사용자 인터페이스 새로고침, 애니메이션 등 사용자와 상호작용하는 작업에 할당한다.
>
>작업이 빠르게 수행되지 않으면, 유저 인터페이스는 멈추게된다.
>
>반응성과 성능에 중점을 둔다.
>
>**2. User-initated**
>문서를 열거나 버튼을 클릭해 액션을 수행하는 것처럼 빠른 결과를 요구하는 유저와의 상호작용 작업에 할당한다.
>
>몇초 이내의 짧은 시간 내에 수행해야 하는 작업으로 반응성과 성능에 중점을 둔다.
>
>**3. Default**
>Qos를 할당해주지 않을 경우 기본값으로 사용되며 User initated 와 Utillity 의 중간 수준의 레벨.
>
>**4. Utility**
>데이터를 읽거나 다운로드 하는 작업처럼 작업이 완료되는데에 어느정도 시간이  걸리거나 즉각적인 결과가 요구되지 않는 작업에 할당한다.
>반응성, 성능, 에너지 효율의 밸런스에 중접을 둔다.
>
>**5. Background**
>index 생성, 동기화, 백업 등 사용자가 볼 수 없는, 백그라운드의 작업에 할당한다.
>에너지 효율에 중접을 둔다.
>
>**6. Unspecified**
>Unspecified 는 Qos의 정보가 없음을 나타내며 시스템이 Qos 를 추론해야 한다.

### attributes
>`attributes` 는 `DispatchQueue` 의 속성값을 정해주는 값이다.
> 속성에는 2가지가 있다.
>
> **1. `serial`**
>    - 아무것도 설정해주지 않으면 자동으로 설정되는 기본값이다!
>    - 스레드 하나만 만들어 사용하는 큐! 

> **2. `concurrent`**
>    - 다중 스레드 환경에서 코드를 처리하는 `DispatchQueue` 가된다.
>    - `.global()` 처럼!
>
> **3. `.initiallyInactive`**
>    - 지금까지 사용했던 일반적인 `DispatchQueue` 는 `sync`, `async`등 코드 블럭을 호출하는 즉시 `DispatchQueue` 작업이 처리되었다.
>    - 요녀석으로 속성값을 주면 작업을 즉시처리해주는것이 아닌 언제 실행할지 제어해줄수있다.
>    - `sync`, `async`를 호출하더라도 작업을 큐에 담아놓을뿐 `active()`를 호출하기 전까지는 작업을 처리하지 않는다.
>    
>**`.initiallyInactive` 예시코드**
>```swift
>let yellow = DispatchWorkItem {
>    for _ in 1...5 {
>        print("😀😀😀😀😀")
>        sleep(1)
>    }
>}
>
>let myDispatch = DispatchQueue(label: "malrang", attributes: .initiallyInactive)
>
>myDispatch.async(execute: yellow) // 코드 블록 호출 안됨.
>myDispatch.activate()
>```

### autoreleaseFrequency
>`DispatchQueue`가 자동으로 객체를 해제하는 빈도의 값을 결정하는 파라미터다.
>
>즉 객체를 `autorealease` 해주는 빈도 라고 할수있다.
>기본값은 `inherit` 이다.
>
>**종류**
>**1. `inherit`**
>   - `target`과 같은 빈도를 가진다.
>**2. `workitem`**
>   - `workitem`이 실행될 때마다 객체들을 해제 한다.
>**3. `never`**
>   - `autorelease` 를 하지 않는다.

### target
>코드 블럭을 실행할 큐를 target 으로 설정할수 있다.

