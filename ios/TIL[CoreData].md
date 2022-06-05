# Today I Learned.

## 학습내용
# CoreData
>![](https://i.imgur.com/ftv4Byn.png)
>
>Device 에 데이터를 저장하거나 CloudKit 을 사용해 여러 Device. 에 데이터를 동기화 할수있게 하는놈이다.
>
>## OverView
>핵심 데이터를 사용하여 오프라인에서 사용할 수 있도록 응용 프로그램의 영구 데이터를 저장하고 임시 데이터를 캐시하며 단일 장치에서 응용 프로그램에 실행 취소 기능을 추가할 수 있습니다. 단일 iCloud 계정의 여러 장치에서 데이터를 동기화하기 위해 Core Data는 자동으로 스키마를 CloudKit 컨테이너에 미러링합니다.
>
>핵심 데이터의 데이터 모델 편집기를 통해 데이터의 유형 및 관계를 정의하고 각 클래스 정의를 생성합니다. 그러면 코어 데이터는 런타임에 개체 인스턴스를 관리하여 다음과 같은 기능을 제공할 수 있습니다.
>
>## Persistence(저장)
>Core Data는 개체를 저장소에 매핑하는 세부 정보를 추상화하여 데이터베이스를 직접 관리하지 않고도 Swift 및 Objective-C에서 데이터를 쉽게 저장할 수 있도록 합니다.
>
>![](https://i.imgur.com/dtI3hs7.png)
>
>## Undo and Redo of Individual or Batched Changes(실행 취소 및 다시 실행)
>Core Data의 실행 취소 관리자는 변경 사항을 추적하고 개별적으로, 그룹으로 또는 한 번에 모두 롤백할 수 있으므로 앱에 실행 취소 및 다시 실행 지원을 쉽게 추가할 수 있습니다.
>
>![](https://i.imgur.com/nGJXxB6.png)
>
>## Background Data Tasks(백그라운드 작업)
>백그라운드에서 JSON을 개체로 구문 분석하는 것과 같은 잠재적인 UI 차단 데이터 작업을 수행합니다.
> 그런 다음 결과를 캐시하거나 저장하여 서버 왕복을 줄일 수 있습니다.
> 
>![](https://i.imgur.com/xdgUgK8.png)
>
>## View Synchronization(동기화 보기)
>Core Data는 또한 테이블 및 컬렉션 보기에 대한 데이터 원본을 제공하여 보기와 데이터를 동기화된 상태로 유지하는 데 도움이 됩니다.
>
>## Versioning and Migration(버전 관리 및 마이그레이션)
>Core Data에는 데이터 모델의 버전을 관리하고 앱이 발전함에 따라 사용자 데이터를 마이그레이션하는 메커니즘이 포함되어 있습니다.
>
>CoreData 는 위의 공식문서를 보면 알수 있듯 Framework다.
>
>CoreData 를 사용하면 디바이스에 permanent data(영구적인 데이터)를 저장할 수 있다.
>
>**UserDefault 와 CoreData의 차이점**
>UserDefault 가 app setting 같은 간단한 정보를 저장하기에 적합하다면 CoreData 는 복잡하고 큰 user data 를 저장하기에 적합하다.
>
>CoreData 는 무언가 영구적으로 저장하기위해 사용하지만 CoreData 의 기능중 하나일뿐이며 그렇기에 CoreData 는 DataBase 가 아니다.
>
>CoreData 는 DataBase 가 아니며 데이터를 유지하기 위한 API 도 아니다.
>
>CoreData 는 넓은 의미로 앱의 모델 계층이며 객체 그래프를 관리하는 Framework 이다.
>
>## CoreData 를 구현해보자
>Core Data 를 사용해서 디바이스에 영구적인 데이터를 저장해보자!
>저장도하고 저장된 데이터를 불러올수도 있다!
>
>처음 프로젝트를 새로 만들때 아래의 사진과 같이 Use CoreData 에 체크를 하게되면 CoreData 파일이 추가된채로 프로젝트가 생성된다.
>![](https://i.imgur.com/hqqgPa6.png)
>
>Use CoreData 를 체크하지 않았다면, 혹은 기존에 작업중이던 프로젝트에 Core Data 를 적용하고 싶다면 Command + N을 눌러 파일 추가 윈도우를 띄운다음 아래의 파일을 추가해주면된다.
>![](https://i.imgur.com/jRrO6iR.png)
>
>이렇게 하면 이렇게 data 파일이 추가된걸 확인할수 있다.
>![](https://i.imgur.com/11IRI9D.png)
>
>다음으로 해주어야 할것은 Entity 를 만들어야한다.
>Entity 는 저장되고 관리되어야 하는 데이터의 집합을 의미한다.
>![](https://i.imgur.com/o829Ezz.png)
>
>하단의 AddEntity 눌러서 추가해주면된다!
>
>전화번호를 저장할 CoreData 를 만들 예정이기 때문에 Entity 의 이름을 Contact 라고 변경했다.
>![](https://i.imgur.com/tz3xvIV.png)
>
>그후 Attribute 를 추가해줄건데 요녀석도 하단의 Add Attribute 를 눌러서 추가하면된다!
>![](https://i.imgur.com/IceAmOB.png)
>
>누르면 요런녀석이 생겨나는데 여기에 CoreData 에 저장될 정보들을 추가해주면된다.!
>![](https://i.imgur.com/kUnu8Ey.png)
>
>예를들면 지금 연락처에대한 CoreData 를 만들고있으니 연락처에 저장해야할 정보 를 입력하면된다!
>이름이랑 전화번호 등등! 어떤 타입으로 저장할것인지 설정해줄수있다!
>![](https://i.imgur.com/W7a4wSs.png)
>
>어떤 정보를 저장할것인지 설정해주었다면 이제는 우측의 Codegen 이라는 녀석을 봐야한다.
>![](https://i.imgur.com/ns1qDbE.png)
>
>옵션이 3개 있는걸 확인할수있다.
>![](https://i.imgur.com/RCUxDQU.png)
>
>옵션들을 한번 살펴보자!
>- **Class Definition**
>   - CoreData가 생성한 managed object subclass 및 프로퍼티 또는 기능을 편집할 필요가 없는경우.
>   - 생성된 소스코드가 프로젝트의 소스목록에 나타나지 않음.
>   - 요점은 CoreData 의 프로퍼티를 수정할필요없을때 쓰는건데 요녀석을쓰면 CoreData가 알아서 소스코드를 만들어준다 근데 파일에는 안보이는거 뿐이다.
>
>- **Category/Extension**
>   - managed object subclass 내에 메소드 또는 비즈니스 로직을 추가하려는 경우 사용한다.
>   - 이 옵션을 사용하면 클래스 파일을 완전히 제어할수 있다.
>   - CoreData 가 만들어주는 class 내부에 메서드 같은것을 추가하려 할때 사용되는것 같다.
>   
>- **Manual/None**
>   - access modifiers 를 변경하고 추가로 편리한 메서드 또는 비즈니스 로직을 추가하기 위해 managed object subclass 를 편집하기 위한용도.
>   - 이 옵션 선택하면 CoreData 는 Class 를 지원하기 위한 파일을 생성하지 않는다.
>   - 아무껏도 안만들어준다는 소리임 사용자 너가알아서 다만들어
>   
>옵션을 알아서 상황에맞게 선택해준다!
>여기서는 Manual/None 옵션으로 진행 ㄱㄱ!
>
>Manual/None 이니까 직접 CoreData 에서 사용될 모델을 직접만들어줘야한다!
>위쪽의 Editor 에 보면 Create NSManagedObject subclass 저거 누르면된다!
>![](https://i.imgur.com/WWIhoXN.png)
>
>next 누르면된다!
>![](https://i.imgur.com/j1kypRD.png)
>
>그럼 이렇게 파일이 2개 생겨난다!
>![](https://i.imgur.com/s4rtKPM.png)
>
>이때 CoreData 에서 사용될 class 의 이름은 처음에 등록했던 Entity 의 네이밍으로 class 가 생성되게 된다!
>
>자 이제부터가 중요하다 CoreData 를 사용하는 느낌은 Commit 하는거랑 비슷?하다 스테이징한후 커밋해서 저장하는느낌 이랄까??
>
>위의 말을 기억하고 이제 해야할것은
>1. 입력받은 데이터를 CoreData 에 저장한다.
>2. 앱을 종료한다.
>3. CoreData 에서 저장한 Data 를 불러온다.
>
>자 진행해 보자!
>```swift
>class ViewController: UIViewController {
>
>    @IBOutlet weak var savedData: UILabel!
>    @IBOutlet weak var nameTextField: UITextField!
>    @IBOutlet weak var phoneNumberTextField: UITextField!
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>    }
>    
>    @IBAction func didTapSaveButton(_ sender: UIButton) {
>    
>    }
>}
>```
>이렇게 텍스트필드에 입력된 값을 가지고 Person타입의 인스턴스를 만들어주도록 했다!
>
>UI는 테스트 용도니까 요로코롬
>텍스트 필드에 입력하고 저장 버튼 누르고 가져옴 버튼 누르면 저장된거 라는 레이블이 변하는 UI!!
>![](https://i.imgur.com/WICJktG.png)
>
>자 이제는 저렇게 만들어준 Person 타입의 newPerson 을 CoreData 에 저장해야하는데 어떻게 하느냐!?
>
>CoreDataStack 에 대해 알아야한다!
>CoreDataStack 은 앱의 모델 layer 를 관리하고 유지하는 역할을 한다.
>![](https://i.imgur.com/DyFAWYS.png)
>
>하나씩보자..
>- **NSPersistentContainer**
>   - Core Data Stack을 나타내는 필요한 모든 객체를 포함한다. 
>
>- **NSManagedObjectModel**
>   - Entity 를 설명하는 DataBase 스키마 라고 한다.
>   - managed objects 의 structure를 정의한다.
>  
>- **NSManagedObjectContext**
>   - transaction 이다.
>   - managed objects를 생성하고, 저장하고, 가져오는(fetch) 작업등등을 제공한다.
>
>- **NSPersistentStoreCoordinator**
>   - persistent storage(영구 저장소)와 managed object model을 연결한다.
>![](https://i.imgur.com/pJ8YSjM.png)
>
>그럼 이제 Person 을 CoreData 에 저장해보자!
>위의 설명에서 Core Data Stack 을 나타내는 필요한 모든 객체를 포함하는 녀석 NSPersistentContainer 요녀석을 사용하자!
>
>**순서는 다음과 같다**
>1. NSManagedObjectContext를 가져온다.
>2. Entity 를 가져온다.
>3. NSManagedObject를 만든다.
>4. NSManagedObject에 값을 세팅해준다.
>5. NSManagedObjectContext를 저장해준다.
>
>**1. NSManagedObjectContext를 가져온다.**
>처음에 프로젝트 만들때 Use CoreData 체크했다면 AppDelegate 에 아래의 녀석이 있을건데 
>```swift
>    // MARK: - Core Data stack
>
>    lazy var persistentContainer: NSPersistentContainer = {
>        let container = NSPersistentContainer(name: "CoreDataTest")
>       container.loadPersistentStores(completionHandler: { (storeDescription, error) in
>            if let error = error as NSError? {
>                fatalError("Unresolved error \(error), \(error.userInfo)")
>            }
>        })
>        return container
>    }()
>
>    // MARK: - Core Data Saving support
>
>    func saveContext () {
>        let context = persistentContainer.viewContext
>        if context.hasChanges {
>            do {
>                try context.save()
>            } catch {
>                let nserror = error as NSError
>                fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
>            }
>        }
>    }
>```
>체크안했다면 저녀석이 없을거다 그럼 만들어주면된다!
>AppDelegate 에 없어도된다 편한곳에 만들자!
>
```swift
let container = NSPersistentContainer(name: "CoreDataTest")
```
>파라미터로 전달해야하는 name 은 처음 만들어두었던 Data 파일 이름을 작성하면된다.
>나는 CoreDataTest 라고 파일이름을 정해두었기 때문에 위와 같이 작성했다.
>![](https://i.imgur.com/t8vFAjf.png)
>
>자 이렇게 해두었으면 이제 만들어둔 AppDelegate 의persistentContainer 요녀석을 활용해주면된다
>
>persistentContainer 요녀석은 아까 얘기한것 처럼 CoreDataStack 을 나타내는 필요한 모든 객체를 가지고 있는놈이니 이녀석 한테서 Context 를 가져와야한다.
>
>왜 context 를 가져와야 하냐면 CoreData 를 활용할때는 git 의 commit 과 비슷하다 저장할데이터를 context에 스테이징한후 commit 한다 라고 생각하면된다.
>
>```swift
>let appDelegate = UIApplication.shared.delegate as! AppDelegate
>lazy var context = appDelegate.persistentContainer.viewContext
>```
>viewController 지역변수로 위와 같이 추가해주었다.
>
>**2. Entity 를 가져온다.**
>이제는 어떤 Entity에 저장해야하는지 알아야 하기 때문에 아래와같이 코드를 추가한다!
>
>```swift
>lazy var entity = NSEntityDescription.entity(forEntityName: "Contact", in: context)
>```
>현재 사용할 Entity 이름이 Contact 이기 때문에 Contact 이니 Entity 이름을 다르게 설정해두었다면 그거 입력해주면된다.
>
>**3. NSManagedObject를 만든다.**
>입력받은 값들로 NSManagedObject를 만들면된다!
>
>```swift
>@IBAction func didTapSaveButton(_ sender: UIButton) {
>        if let entity = entity {
>            let newPerson = NSManagedObject(entity: entity, insertInto: context)
>            newPerson.setValue(person.name, forKey: "name")
>            newPerson.setValue(person.phoneNumber, forKey: "phoneNumber")
>        }
>
>        do {
>            try context.save()
>        } catch {
>            print(error.localizedDescription)
>        }
>    }
>```
>이제 저장된걸 확인하려면 저장한 데이터에서 꺼내와야 확인할수있다.
>
>요녀석은 어렵지 않다.
>```swift
>@IBAction func fetchContact() {
>        do {
>            let contact = try context.fetch(Contact.fetchRequest()) as! [Contact]
>        } catch {
>            print(error.localizedDescription)
>        }
>    }
>```
> 아까 만들어진 Contact 모델 내부에 이미 구현되어있다.
> 그냥 호출만 해주면된다!
> cotext의 fetch메소드는 [Any]타입을 리턴하기 때문에 [Contact]타입으로 캐스팅 해주면된다.
> 
>이제 드디어결과물을 보도록하자!
>![](https://i.imgur.com/hLSmS6b.gif)
>
>전체코드는 이렇다!
>```swift
>import UIKit
>import CoreData
>
>class ViewController: UIViewController {
>    
>    @IBOutlet weak var savedData: UILabel!
>    @IBOutlet weak var nameTextField: UITextField!
>    @IBOutlet weak var phoneNumberTextField: UITextField!
>    
>    let appDelegate = UIApplication.shared.delegate as! AppDelegate
>    lazy var context = appDelegate.persistentContainer.viewContext
>    lazy var entity = NSEntityDescription.entity(forEntityName: "Contact", in: context)
>    
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>    }
>    
>    @IBAction func saveData(_ sender: UIButton) {
>        if let entity = entity {
>            let newPerson = NSManagedObject(entity: entity, insertInto: context)
>            newPerson.setValue(nameTextField.text, forKey: "name")
>            newPerson.setValue(phoneNumberTextField.text, forKey: "phoneNumber")
>        }
>        do {
>            try context.save()
>        } catch {
>            print(error.localizedDescription)
>        }
>    }
>    
>    @IBAction func fetchData(_ sender: UIButton) {
>        do {
>            let contact = try context.fetch(Contact.fetchRequest()) as! [Contact]
>            savedData.text = contact.last?.name
>        } catch {
>            print(error.localizedDescription)
>        }
>    }
>}
>```
>구현해보니 Appdelegate 말고 다른방식으로 만들수 있을것같다!
>User 의 정보를 가지고 CoreData 에 저장하거나 저장된 값을 불러오는 메서드를 갖는 모델을 만든다던지!
>다음에 구현해보도록 하자!

### 참고 문서
[CoreData](https://developer.apple.com/documentation/coredata)
[제드님블로그](https://zeddios.tistory.com/987)
