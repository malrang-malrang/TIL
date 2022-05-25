# Today I Learned.

## 학습내용
# UIImagePickerController
> 
>![](https://i.imgur.com/A24IEQi.png)
>
>사용자의 media library에서 사진 촬영, 동영상 녹화 및 사진 선택을 위한 시스템 인터페이스를 관리하는 ViewController
>
>**아래의 3개 하는놈이다.**
>1. 사진을 직접 찍어서 불러오기
>2. 동영상을 직접 찍어서 불러오기
>3. 사진 또는 영상을 앨범에서 가져오기
>
>## Declaration
>```swift
>@MainActor class UIImagePickerController : UINavigationController
>```
> 
>## Overview
>`UIImagePickerController`는 사용자 상호 작용을 관리하고 이러한 상호 작용의 결과를 `delegate`에 전달한다.
>
> `image picker controller` 의 역할과 모습은 해당 image picker controller를 present하기 이전에 source type에 할당한 값에 따라 달라진다.
> 
>- `UIImagePickerController.SourceType.camera`
>   - 실행되는 장치에서 새 사진 또는 동영상을 촬영하기 위한 사용자 인터페이스를 제공한다.
>- `UIImagePickerController.SourceType.photoLibrary`
>   - 저장되어 있는 사진과 영상들 중에서 고를 수 있도록 해주는 UI를 제공한다.
>- `UIImagePickerController.SourceType.savedPhotoAlbum`
>   - 저장되어 있는 사진과 영상들 중에서 고를 수 있도록 해주는 UI를 제공한다.
>   
>## `UIImagePickerController` 를 사용하기 위한 단계
>
>**1. 사용해야 하는 기기가 해당 기능을 사용가능한지 확인해야한다.**
>
>- isSourceTypeAvailable(_:) 클래스 메서드를 호출하여 확인해보고 싶은 타입 UIImagePickerController.SourceType 을 넣어서 Bool 값을 확인한다.
> - 카메라, 앨범, 사진 출처가 존재하지 않거나 사용 불가능할수도 있다.
>   앨범에 가서 사진을 가져 오려고 했는데 아무런 사진이없거나 카메라가 다른 앱에서 이미 사용중이거나 등등 의 경우가 있을수 있다.
> 
>**2. 출처가 사용가능할 경우, UIImagePickerController의 출처 속성에 안전하게 지정해주기**
>- 특정 sourceType에서 사용할 수 있는 mediaType을 확인하려면 availableMediaTypes(for:) 클래스 메서드를 호출한다.
> 이를 통해서 사진만 사용할 수 있는지 비디오도 사용할 수 있는지 구별할 수 있다.
> 
>**3. 사용하기로한 출처에 대해 어떤 미디어 타입을 가져오는것을 목표로 하는지 명시해주기.**
>- mediaTypes 프로퍼티 값을 설정함으로써 사용할 미디어 유형에 따라 UI를 조정할 수 있도록 Image Picker Controller 에 지시한다.
>- 위의 단계는 사진인지 비디오인지 정해주는거라면 요거는 어떤 미디어타입을 가지고 올건지 정해주는거다.
>- 즉 UIImagePickerController 에게 위의 예시가 카메라 였다면 카메라가 미디어의 출처고 카메라를 쓰되 이앱에서는 동영상만, 사진만, 혹은 둘다 가져와도돼 라고 명시해주는 것이다.
>
>**4. UIImagePickerController 띄우기**
>- 현재 뷰컨트롤러를 픽커뷰 컨트롤러의 delegate 로 지정한다.
>- present(_: animated: completion: ) 메서드를 이용해서 iPhone, iPad touch 의 경우는 (full screen) modal 창을 띄우며, iPad의 경우는 source type에 따라 아래와 같이 present를 한다.
>   - camera : Use full screen
>   - Photo Library : Must use a popover
>   - Saved Photos Album : Must use a popover
>
>**5. image picker dissmiss 하기**
>- 유저가 새로 찍거나 저장되어있던 이미지 혹은 비디오를 선택하기 위해서 버튼을 눌렀을 때 혹은 작업을 취소하면 delegate 개체를 이용하여 image picker 를 dismiss 한다.
>- 카메라로 새롭게 찍어진 media의 경우는 delegate 가 해당 디바이스 카메라 롤에 저장할 수 있다. 
>- 이전에 저장한 media의 경우 대리인이 앱의 목적에 따라 이미지 데이터를 사용할 수 있다.
>
>## important
>```
>UIImagePickerController class 는 세로모드만 지원한다.
>이 클래스는 있는 그대로 사용하기 위한것이며 서브 클래싱을 지원하지 않는다.
>그렇기때문에 UIImagePickerController 의 클래스 계층 구조는 비공개이며 한가지 예외를 제외하고서는 수정하면 안된다.
>cameraOverlayView 프로퍼티 뷰를 커스텀하거나 사용하여 추가 정보를 제>공하는 카메라 인터페이스와 코드간의 상호작용을 관리할수있다.
>```
> 
>## Providing a Delegate Object
>
>image picker controller 를 사용하기 위해서는, UIImagePickerControllerDelegate 를 채택해서 delegate를 사용해야만 한다.
>
>## Adjusting Flash Mode
>
>## Working with Movies
>
>## Working with Live Photos
>
>## Fully-Customized Media Capture and Browsing
>
>## 이녀석을 이용해 앨범에서 사진을 가져오기위해 설정해주어야 하는것들
>
>**프로젝트의 Info 에서 두가지 기능을 추가해주어야 한다.**
>1. Privacy - Camera Usage Description (카메라 사용 허가 해주는것)
>2. Privacy - PhotoLibraryAddUsageDescription(사진 앨범 가져오게 허가 해주는것)
>
>![](https://i.imgur.com/jA2Iuti.png)
>
>위의 사진처럼 Info 에서 Information Property List 에 + 버튼을 클릭해 위의것들을 추가해주면 된다!
