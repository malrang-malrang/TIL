# Today I Learned

# 학습내용

# Type Casting(타입 캐스팅)
## 타입 검사 (Checking Type)
인스턴스가 특정 하위 클래스 타입인지 확인하기 위해 타입 검사 연산자 (type check operator) (is)를 사용합니다. 이 타입 검사 연산자는 인스턴스가 하위 클래스 타입이면 true 아니면 false 를 반환합니다.

예시
```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}

let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]

var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}
```

위의 예시에서 library의 타입은 MediaItem 즉 Array<MediaItem>, library[MediaItem] 라고 할수있다.

movie, song 인스턴스를 상위 클래스인 MediaItem 로 묶어 library라는 배열에 저장한경우 이다.
    
이때 library의 element 들이 어떤 타입인지 확인할때 위와 같이 is 를 사용할수있다. 
library 의 0번째 인덱스가 Movie 타입이라면 movieCount += 1 하라는 뜻이다.

## Downcasting(다운 캐스팅)

상위 클래스의 인스턴트 는 하위 클래스의 인스턴스를 참조 할수있다.
이때 참조할수 있는지 없는지 확인한후 참조 할수 있는데 이때 사용되는것이 다운캐스팅, `as?`또는`as!`를 사용할수있다.
다운 캐스팅에 실패할수 있기때문에 반환값은 옵셔널이다!

예시
```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}
```
위의 예시는 `library`의 `element`가 `Movie`타입으로 다운캐스팅에 성공할경우 `if let`을 이용해 바인딩해주어 `Non-Optional` 값을 `movie`에 할당 해준 경우이다.
다운캐스팅에 성겅했으니 `movie`는 `Movie`타입의 프로퍼티에 접근해 사용할수있게된다.
`song`의 경우도 마찬가지!
    
## Any와 AnyObject에 대한 타입 캐스팅 (Type Casting for Any and AnyObject)    
    
Swift는 비특정 타입 작업을 위해 2개의 특별한 타입을 제공합니다
- Any 는 함수 타입을 포함하여 모든 타입의 인스턴스를 나타낼 수 있습니다.
- AnyObject 는 모든 클래스 타입의 인스턴스를 나타낼 수 있습니다.

Any의 예시
```swift
var things: [Any] = []

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })
```
`things` 에는 지금 `Int타입`, `Double타입`, `Strin타입`, `tuple타입`, `class의 인스턴스` 가 저장되어있다.
참고(이때 튜플 타입의 타입은 내부의 값타입들에 의해 결정된다!)
   
이때 `is`, `as` 를 사용하여 `things` 의 `element` 들이 어떤 타입인지 조회 하고 분기를 나누어 실행시켜줄수도 있다.

예시
```swift
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}
```
- `is` 를 사용한경우
    - `element` 가 어떤 타입인지 물어보고 맞다면 해당 case 실행구문을 실행시키라는 뜻이다.
- `as` 를 사용한경우 `element` 가 `Int타입`으로 다운캐스팅 할수있는지 확인할수있으며 사용된 모습은 다음과같다.
    - `case let someInt as Int:` 처럼 바인딩해준뒤 `print(someInt)` 처럼 Non-Optional 값을 사용할수 있게된다.
- 클래스 타입에서 as 를 사용한경우
    - 현재 `things`에는 Movie타입의 인스턴스가 하나 저장되어있다.
    - 이녀석은 things에 저장될때는 `Any`타입으로 업캐스팅? 되어 저장되었기 때문에 `_:Any = Movie()` 처럼 저장되어있다. 이때 Movie타입의 프로퍼티나 메서드를 사용하고 싶다면 다운캐스팅해주어야 하는데 실패할경우 nil 이반환 되기때문에 Optional 타입이므로 위의 예시와 같이`case let movie as Movie`처럼 바인딩 해주어 `movie`에 Non-Optional 값을 넣어줄수 있다. 다운캐스팅에 성공하면 Movie타입의 프로퍼티나 메서드를 사용 가능하다!
    
**클래스에서 as 를 사용한 예시**
```swift
class A {
    var a: String = "ee"
}

class Z: A {
    var z: String = "dd"
}

var a: A = Z()
print(a.a)
// "ee"
```
위의 예시를 보면 `A`를 상속받는 `Z class`가 있다.
이때 변수로 선언된 `a`는 `Z` 의 프로퍼티를 사용 불가능하며 
`A` 의 프로퍼티인 `a`만 사용할 수 있는 상황이다.

```swift
if let z = a as? Z {
    print(z.z)
} else {
    print("다운 캐스팅 실패")
}
```
이때 다운 캐스팅을 하면 `Z`의 프로퍼티를 사용할수 있게 해줄수있다.
바인딩 작업을 해주어야 하기때문에 `if let` 을 이용해 `z` 라는 상수를 선언해주고 여기에 `a` 를 `Z` 로 다운캐스팅에 성공한다면 `a`에 속한 `Z`참조 할수있게되므로 `a`안에있는 `Z`의 부분을 `z`가 참조한다고 할수있다.
    
```swift
class Asd {
    var asd: String = ""
}

class Zxc: Asd {
    var zxc: String = "dd"
}

var a: Asd = Zxc()

func xx() -> String {
    guard let zxc = a as? Zxc else {
        return "x"
    }
    zxc.asd = "aa"
    return zxc.asd
}

print(a.asd)
print(xx())
print(a.asd)

// ""
// aa
// aa
```
위와같이 `zxc`의 `asd`프로퍼티의 값을 변경하니 `a`의 프로퍼티인`asd`도 값이 변경된걸 확인할수있다. 
`a`, `zxc` 가 같은 메모리주소를 사용하고있다는것을 알수있다.
