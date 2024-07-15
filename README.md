# Bottles(바틀즈)

## 📖 목차
- [🍷 프로젝트 소개](#-프로젝트-소개)
- [🛠️ 기술스택](#-기술스택)
- [🚀 트러블 슈팅](#-트러블-슈팅)
- [📱 주요기능과 스크린샷](#-주요기능과-스크린샷)
- [💻 개발 도구 및 활용한 기술](#-개발-도구-및-활용한-기술)
- [🏅 수상내역](#-수상내역)
---

## 🍷 프로젝트 소개
<img src = "https://user-images.githubusercontent.com/101093592/223049709-5b6e61a2-bfd3-4285-a7e5-8124b6663010.png">

> 내 주변 바틀샵에 대한 정보를 지도를 통해 제공 <br>
> 바틀샵 정보와 합리적인 구매를 위한 가격 비교 및 앱 내 바틀 픽업예약을 통한 편리한 예약시스템

---

## 🛠️ 기술스택
<p align="leading">
  <img src="https://img.shields.io/badge/Swift-F05138?style=for-the-badge&logo=Swift&logoColor=white"/>
  <img src="https://img.shields.io/badge/SwiftUI-021B97?style=for-the-badge&logo=swift&logoColor=white"/>
  <img src="https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=Firebase&logoColor=white"/>
    <img src="https://img.shields.io/badge/UIKit-2396F3?style=for-the-badge&logo=uikit&logoColor=white"/>
</p>

---

## 🚀 트러블 슈팅
**1) `UIViewRepresentable` 을 활용한 `네이버 지도 API` 구현**
- `SwiftUI` 프레임워크를 기반으로 진행했기 때문에 네이버 지도 API를 사용하기 위해선 `UIViewRepresentable` 프로토콜을 채택하여 구현해야 했습니다. 
- 홈 뷰에서는 전체 바틀샵에 대한 마커와 북마크한 바틀샵에 대한 마커를 띄워줘야 했는데 초기 개발 당시 `makeUIView` 함수에서 마커를 띄워준 후 북마크 버튼 터치 시 `updateUIView`에서 마커를 띄워주도록 구현했습니다. 
- 하지만, 여기서 뷰를 초기화해주지 않으면 마커가 중복되어 쌓이게 될 뿐만 아니라 메모리 효율이 현저히 떨어진다는 문제점이 발생했습니다. 
- 이에 대한 트러블슈팅으로 Apple 개발자 공식 문서를 참고하던 중 "`UIViewRepresentable` 프로토콜 채택 시 다른 SwiftUI 뷰와 상호작용하기 위해서 `Coordinator`를 사용하여 `target-action`을 전달한다"는 부분에 착안하여 해결 방안을 구성했습니다. 
- 따라서, 초기에 전체 바틀샵에 대한 마커를 보여줬던 `makeUIView`에서는 네이버 지도 자체만 전달하도록 했습니다. 또한, 맵 뷰에서 발생하는 모든 이벤트를 처리했던 ‘updateUIView`에 대한 의존도를 최소화하여 불필요한 메모리 누수를 줄였습니다. 
- 이외 발생하는 모든 매소드를 `Coordinator`에서 처리하도록 리팩토링

**2) 불필요한 `Coordinator` 메서드 중복 생성 이슈**
- 네이버 맵을 전달받은 SwiftUI 뷰에서 사용하기 편하게 `shared`라는 상수를 `static`으로 싱글톤화하여 `Coordinator.shared`로 접근할 수 있게 선언
- 코드의 효율성을 위해 `CLLocationManagerDelegate` 또한 `Coordinator`에 채택하여 협업 과정에서 맵에 대한 정보를 받아올 시 팀원들 모두 간편하게 사용할 수 있게 구현

**3) 앱 진입 시 로딩 속도 최적화**
 - 앱 출시 당시 약 100개의 바틀샵 데이터 각 바틀샵 별 바틀 정보 뿐만 아니라 유저 데이터를 로딩 시 불러와야 했으나 정확한 데이터 로드 시점 파악 불가
 - 따라서, `DispatchQueue`를 활용하여 강제적으로 데이터 로드 시간 확보
 - 이후, 각 데이터 별 `ViewModel`에서 데이터가 로드되는 시점을 Boolean 값으로 판단하여 `View`에 전달하는 로직을 통해 강제적인 로딩 시간 지연이 아닌 데이터가 로드된 시점에 메인 화면으로 진입할 수 있게 리팩토링
 - 또한, `FirestoreDatabase` 데이터를 불러올 때 전체 데이터가 아닌 필요한 데이터를 `whereField`로 처리하여 이 과정에서 발생하는 로딩 지연을 리팩토링

---

## 📱 주요기능과 스크린샷

|![IMG_7707](https://github.com/user-attachments/assets/f7e86c56-2286-4cfe-8d85-6bd4a3d7e1bc)|![IMG_7708](https://github.com/user-attachments/assets/5dde8e11-31bb-4b72-ac81-f3475e341119)|![IMG_7709](https://github.com/user-attachments/assets/47db8c0f-f0eb-466f-9ab7-15b9cddad711)|![IMG_7694](https://github.com/user-attachments/assets/9511648d-4196-4698-a784-429c61e69b42)|
|:----:|:----:|:-----:|:----:|
|`소셜 로그인 뷰`|`이메일 로그인 뷰`|`회원가입 뷰`|`스플래시 뷰`|


|![IMG_7711](https://github.com/user-attachments/assets/42269614-269c-4c93-9a52-213dcb4be8e6)|![IMG_7695](https://github.com/user-attachments/assets/317b3bf8-2aa4-4b65-a0ed-d43b937d485b)|![IMG_7696](https://github.com/user-attachments/assets/3dccd45c-1c75-4f37-aa0b-0ced50dab68f)|![IMG_7703](https://github.com/user-attachments/assets/04e7c6e5-efb7-4209-a852-7c2f812728da)|
|:----:|:----:|:-----:|:----:|
|`주변 탭`|`내 주변 둘러보기 뷰`|`북마크 탭`|`검색 뷰`|


|![IMG_7716](https://github.com/user-attachments/assets/d12119a4-2f99-4206-9f90-4aab968fb68f)|![IMG_7719](https://github.com/user-attachments/assets/d71cdfb0-97ce-4176-bf32-e57d101996a8)|![IMG_7718](https://github.com/user-attachments/assets/028388e6-4ac7-4958-9f2b-5165e07bd6d0)|![IMG_7717](https://github.com/user-attachments/assets/3d5798c8-1a0b-47f0-87b3-dda05baba64f)|
|:----:|:----:|:-----:|:----:|
|`바틀샵 디테일 뷰` <br> `(상품검색)`|`바틀샵 디테일 뷰` <br> `(공지사항)`|`바틀샵 큐레이션 뷰`|`매장정보 뷰`|

|![IMG_7713](https://github.com/user-attachments/assets/1875a6dc-e7c4-4ae0-8f48-903cdf7d4691)|![IMG_7706](https://github.com/user-attachments/assets/5a7b6970-b5cf-4393-b4a2-29d1f3ee6735)|![IMG_7715](https://github.com/user-attachments/assets/d0114786-36c2-4d4c-818b-f93fb92902d7)|![IMG_7698](https://github.com/user-attachments/assets/42ceec59-ba14-4df2-9944-584e68f82386)|
|:----:|:----:|:-----:|:----:|
|`예약진행 뷰`|`장바구니 뷰`|`예약화면`|`알림 탭`|

|![IMG_7699](https://github.com/user-attachments/assets/cd8f31c8-6004-426d-a58e-b3398708f5a5)|![IMG_7700](https://github.com/user-attachments/assets/8662798a-a91f-4cd0-92e3-7cc12937333b)|![IMG_7701](https://github.com/user-attachments/assets/c514fd8b-cc17-43b7-b604-c4bcf74abc2f)|![IMG_7712](https://github.com/user-attachments/assets/3c5b0eea-3c13-44ea-8507-4cebe01a7920)|
|:----:|:----:|:-----:|:----:|
|`마이페이지 뷰`|`설정 뷰`|`예약내역`|`번호 변경`|

---

## 💻 개발 도구 및 활용한 기술
- 개발 언어 : Swift
- 개발 환경 : Swift5 16.2, SE ~ iPhone 14 Pro 호환, 가로모드, 다크모드 미지원
- 디자인 툴 : Figma
- 협업 도구 : Github, Team Notion
- 활용한 기술
    - Xcode
    - FCM, Firebase function
    - KakaoSDKAuth, FBSDKCoreKit, GoogleSignIn, AppleLogin
    - FirebaseAuth, FiresStore, Firebase Storage
    - NaverMap API
    - CoreData
    
---

## 🏅 수상내역
|<img width="610" alt="스크린샷 2024-07-15 오후 7 32 44" src="https://github.com/user-attachments/assets/d11030cf-a697-4fc7-80f2-1b4d1932241e">|<img width="610" alt="223365235-da8b4ee6-fb8c-4cf0-b2cb-13a87fffab34" src="https://github.com/user-attachments/assets/7cb21fac-6655-4a54-a14f-a8e8e0f646f5">|
|:---:|:---:|
|`멋쟁이사자처럼 앱스쿨 최종프로젝트 최우수상`|`멋쟁이사자처럼 앱스쿨 해커톤 대상`|

---

## 📓 설치 / 실행 방법

### 1️⃣ 아래 파일은 필수 파일이므로 다음 이메일로 파일을 요청 😝
(changhyun.kyle@gmail.com)
```
- Bottles-V2-Info.plist
- GoogleService-Info.plist
- Podfile
```

### 2️⃣ 아래와 같이 pod 파일을 설치 🤗
```sh
$ pod install
```

### 3️⃣ Bottles_V2.xcworkspace 파일 실행 😎

---

## 👨‍👩‍👦‍👦 참여자
|안은노|강창현|고범석|김영서|서찬호|
|:----:|:----:|:-----:|:----:|:-----:|
|<img src = "https://avatars.githubusercontent.com/u/33450365?v=4">|<img src = "https://avatars.githubusercontent.com/u/101093592?v=4">|<img src = "https://avatars.githubusercontent.com/u/114239407?v=4">|<img src = "https://avatars.githubusercontent.com/u/114224237?s=120&v=4">|<img src = "https://avatars.githubusercontent.com/u/102764542?s=120&v=4">|
|[@Eunno-An](https://github.com/Eunno-An)|[@Changhyun-Kyle](https://github.com/Changhyun-Kyle)|[@bamsak](https://github.com/bamsak)|[@yngddo](https://github.com/yngddo)|[@SeoChanHo](https://github.com/SeoChanHo)|

|봉혜미|신미지|이진아|장다영|최현종|
|:----:|:----:|:-----:|:----:|:-----:|
|<img src = "https://avatars.githubusercontent.com/u/98953443?v=4">|<img src = "https://avatars.githubusercontent.com/u/62836016?v=4">|<img src = "https://avatars.githubusercontent.com/u/55937627?v=4">|<img src = "https://avatars.githubusercontent.com/u/80445363?s=120&v=4">|<img src = "https://avatars.githubusercontent.com/u/108848166?v=4">|  
|[@hyemib](https://github.com/hyemib)|[@SMizzz](https://github.com/SMizzz)|[@l1004ga](https://github.com/l1004ga)|[@Da01002](https://github.com/Da01002)|[@EthanColdChoi](https://github.com/EthanColdChoi)|

---

## 📝 라이센스
### "Bottles" is available under the MIT license. See the [LICENSE](https://github.com/APPSCHOOL1-REPO/finalproject-bottles/blob/main/License) file for more info.
