# Today I Learned.

## 학습내용
# Core Graphics
![](https://i.imgur.com/v3VpIbe.png)

Quarts(쿼츠)라는 그래픽 라이브러리 안에 들어있는 기술을 활용하여 2D 렌더링, 수행 경로 기반 드로잉, 안티 얼리어싱 렌더링, 그라디언트, 이미지, 색상, PDF 문서 등등의 처리를 해주는 프레임워크다.

## OverView
Core Graphics 프레임워크는 Quartz 고급 드로잉 엔진을 기반으로 합니다. 이것은 타의 추종을 불허하는 출력 충실도로 낮은 수준의 가벼운 2D 렌더링을 제공합니다.

이 프레임워크를 사용하여 경로 기반 그리기, 변환, 색상 관리, 오프스크린 렌더링, 패턴, 그라디언트 및 음영, 이미지 데이터 관리, 이미지 생성, 이미지 마스킹, PDF 문서 생성, 표시 및 구문 분석을 처리합니다.

macOS에서 Core Graphics에는 디스플레이 하드웨어, 낮은 수준의 사용자 입력 이벤트 및 윈도우 시스템 작업을 위한 서비스도 포함되어 있습니다.

## Topics
### Geometric Data Types
- struct CGFloat
   - Core Graphics 및 관련 프레임워크의 부동 소수점 스칼라 값에 대한 기본 유형입니다.
- struct CGPoint
   - 2차원 좌표계의 한 점을 포함하는 구조입니다.
- struct CGSize
   - 너비 및 높이 값을 포함하는 구조입니다.
- struct CGRect
   - 직사각형의 위치와 치수를 포함하는 구조입니다.
- struct CGVector
   - 2차원 벡터를 포함하는 구조입니다.
- struct CGAffineTransform
   - 2D 그래픽을 그리는 데 사용하기 위한 아핀 변환 행렬입니다.

### 2D Drawing
- class CGContext
   - Quartz 2D 도면 환경.
- class CGImage
   - 비트맵 이미지 또는 이미지 마스크입니다.
- class CGPath
   - 변경할 수 없는 그래픽 경로: 그래픽 컨텍스트에서 그릴 모양이나 선에 대한 수학적 설명입니다.
- class CGMutablePath
   - 변경 가능한 그래픽 경로: 그래픽 컨텍스트에서 그릴 모양이나 선에 대한 수학적 설명입니다.
- class CGLayer
   - Core Graphics로 그린 콘텐츠를 재사용하기 위한 오프스크린 컨텍스트입니다.

### Colors and Fonts
- class CGColor
   - 색상을 해석하는 방법을 지정하는 색상 공간과 함께 색상을 정의하는 구성 요소 집합입니다.
- class CGColorConversionInfo
   - 다른 시스템 서비스에서 사용하기 위해 색상 공간 간에 변환하는 방법을 설명하는 개체입니다.
- class CGColorSpace
   - 표시할 색상 값을 해석하는 방법을 지정하는 프로필입니다.
- class CGFont
   - 텍스트 그리기를 위한 일련의 문자 상형 문자 및 레이아웃 정보입니다.

PDF 문서 작업, 유틸리티 및 지원 클래스 등등이 있으며 해당 기능이필요할때는 공식문서 들어가서 다시한번 찾아보자!

## CoreGraphics란?
위의 공식문서를 보니 CoreGraphics 는 Quarts 에있는 기술을 사용해서 2차원 그래픽을 그릴수있는 그래픽 프레임워크다.

## Quarts?란?
CoreGraphics에서 Quarts를 사용한다고 되어있는데 Quarts는 뭐지??
2차원을 그리기위한 2D 엔진이다.
Quarts는 CoreGraphics와 CoreAnimation 으로 구성되어있다.
(하나의 라이브러리가 아니다.)

---
# CoreGraphics로 그림을 그려보자!
음..그러니까 CG붙은 것들을 이용해서 2D 그림을 그릴수있게 하는것 이라고 이해했다.
CGPoint, CGRect 등등을 이용해서 말이다.(위의 공식문서 Topic에 나와있다.)

Draw메서드 내부에서 CoreGraphics를 이용해 그림을 그려보자!
CoreGraphics를 이용해 그림을 그리기위해서는 UIGraphicsGetCurrentContext 에대해 알아야한다!

## UIGraphicsGetCurrentContext()
![](https://i.imgur.com/WXZlEWr.png)
현재 그래픽 컨텍스트를 반환합니다.

## Declaration
```swift
func UIGraphicsGetCurrentContext() -> CGContext?
```
요녀석은 CGContext 음.. 도화지라고 이해하면될까?? 어디다 어떻게 그릴건지 그리기위해서는 도화지가 필요하다!

UIGraphicsGetCurrentContext 메서드는 CGContext 즉 도화지를 반환하니 요녀석을 사용해 CGContext를 가져오면된다!

그럼 이제 아래의 그림들을 그려보도록하자!

**Normal State**
![](https://i.imgur.com/ngSnTDj.png)

**Selected State**
![](https://i.imgur.com/MDHL6PA.png)

만들것은 동그라미 모양을가진 버튼인데 클릭하게되면 동그라미 내부에 체크표시가 생기는 버튼을 만들거다!!

```swift
@IBDesignable
class CheckButton: UIButton {
    
    @IBInspectable var color: UIColor = UIColor.systemGreen
    @IBInspectable var lineWidth: CGFloat = 10
    
    //그림을 그리는 메서드
override func draw(_ rect: CGRect) {
        // 위에서 설명한대로 도화지를 가져오자!
        guard let context = UIGraphicsGetCurrentContext() else {
            return
        }
    
    //요녀석들은 버튼 자기자신의 height와 width 이다.
    //버튼 크기가 어떻게 될지모르지만 요렇게하면 항상 자기자신의 크기의 값을 가져올수있다!
    let height = bounds.height
    let width = bounds.width
    
    //context에 그림을 그리기 시작할건데 라인 두께와 색상은 요렇게할거야! 하고 설정해준 코드
    context.beginPath()
    context.setLineWidth(lineWidth)
    context.setStrokeColor(UIColor.black.cgColor)
 
    //context에 원을 추가해준다 
    //원의 크기는 버튼의 Bounds에서 dx, dy 만큼 안쪽에서 원을 그리겠다는 뜻이다.
    let circle = bounds.insetBy(dx: width * 0.05, dy: height * 0.05)
    context.addEllipse(in: circle)
    
    //버튼의 상태값이 isSelected일 경우 원내부에 체크표시를 하겠다는 뜻이다.
    if isSelected {
            context.setStrokeColor(color.cgColor)
            //해당 좌표로이동.
            context.move(to: CGPoint(x: width * 0.2,
                                     y: height * 0.5))
            //위의 좌표에서 해당 죄표까지 선을 그린다.
            context.addLine(to: CGPoint(x: width * 0.45,
                                        y: height * 0.8))
            context.addLine(to: CGPoint(x: width * 0.8,
                                        y: height * 0.3))
            //선끝을 둥글게 해준다.
            context.setLineCap(.round)
            //선과 선이 만나는지점 을 둥글게.
            context.setLineJoin(.round)
        }
    
        //지금까지 설정한 것들을 실제로 그린다.
        context.drawPath(using: .stroke)
        context.closePath()
}
```
위의 코드에서 원을 추가할때 사용한메서드 addEllipse() 말고도 원을그릴수있는 strokeEllipse() 메서드가 있는데 

공식문서의 설명은 아래와 같이나와있는데 비슷하다.
- addEllipse:
  - Adds an ellipse that fits inside the specified rectangle
- strokeEllipse: 
  - Strokes an ellipse that fits inside the specified rectangle

두개의 차이는 여러그림을 겹칠때 그려지는 순서가 달라진다.
add를 2번 사용한후 drawPath() 를 하게되면 한번에 그려지지만 
add, stroke 를 사용한후 drawPath() 를 하게되면 stroke 한 그림이 아래에 깔리게 된다고 한다.

## @IBDesignable
위의 코드에 사용된 녀석이다. 
요녀석은 라이브 렌더링 인데 사용하게되면 draw 메서드를 실행해서 스토리보드에서 바로 확인할수있다.
```swift
@IBDesignable
class CheckButton: UIButton {}
```
객체앞에 추가해주면된다!

## @IBInspectable
요녀석 사용하면 Interface Builder에서 읽을 수 있는 프로퍼티를 추가할 수 있다.

버튼에서 사용될 라인의 width와 체크표시 그림의 색상을 스토리보드에서 설정하고 싶을경우 요렇게 코드를 추가하면된다.
```swift
@IBInspectable var color: UIColor = UIColor.systemblue
@IBInspectable var lineWidth: CGFloat = 5
```
이렇게 코드를 추가하면 초기값은 너비 5에 색상은 파란색으로 설정되지만 스토리보드에서 수정할수 있게된다.

그리고 draw메서드 내부에서 색과 너비를 설정해주는 코드에 위에 선언한 color와 lineWidth를 할당해주면된다.
```swift
//draw 메서드 내부에있는 애들
context.setLineWidth(lineWidth)
context.setStrokeColor(color.cgColor)
```

## 참고한 문서 및 자료
[Core Graphics](https://developer.apple.com/documentation/coregraphics)
[Green](https://green1229.tistory.com/73)
