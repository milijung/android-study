# android platform

# 앱 구성 요소
앱 구성 요소에는 네가지 유형이 있습니다:
### 1. **액티비티(Activity)**
액티비티는 앱 안의 단일 화면을 나타냅니다. 즉, 사용자 인터페이스를 포함한 화면 하나를 나타냅니다. 
### 2. **서비스(Service)**
서비스는 사용자 인터페이스 없이 백그라운드에서 앱을 계속 실행하기 위한 다목적 진입점입니다. 이것은 백그라운드에서 실행되는 구성 요소로, 오랫동안 실행되는 작업을 수행하거나 원격 프로세스를 위한 작업을 수행합니다. 예를 들어 서비스는 사용자가 다른 앱에 있는 동안에 백그라운드에서 음악을 재생하거나, 사용자와 액티비티 간의 상호작용을 차단하지 않고 네트워크를 통해 데이터를 가져올 수도 있습니다. 
### 3. **브로드캐스트 리시버(Broadcast Receiver)**
Broadcast Receiver는 모든 앱이 수신할 수 있는 메시지입니다. 시스템이 정기적인 사용자 플로우 밖에서 이벤트를 앱에 전달하도록 지원하는 구성 요소로, 앱이 시스템 전체의 브로드캐스트 알림에 응답할 수 있게 합니다. Broadcast Receiver도 앱으로 들어갈 수 있는 또 다른 명확한 진입점이기 때문에 현재 실행되지 않은 앱에도 시스템이 브로드캐스트를 전달할 수 있습니다. 예를 들어 앱이 사용자에게 예정된 이벤트에 대해 알리는 알림을 게시하기 위한 알람을 예약할 경우, 그 알람을 앱의 Broadcast Receiver에 전달하면 알람이 울릴 때까지 앱을 실행하고 있을 필요가 없습니다. 대다수의 브로드캐스트는 시스템에서 발생합니다. 예컨대 화면이 꺼졌거나 배터리가 부족하거나 사진을 캡처했다고 알리는 브로드캐스트가 대표적입니다. 
### 4. **콘텐츠 제공자(Content Provider)**
파일 시스템, SQLite 데이터베이스, 웹상이나 앱이 액세스할 수 있는 다른 모든 영구 저장 위치에 저장 가능한 앱 데이터의 공유형 집합을 관리합니다. 

# 앱 아키텍처

## MVC (Model-View-Controller)
* **Controller** - 사용자의 입력을 전달받아, Model이 동작하도록 명령합니다.
* MVC의 가장 큰 문제는 View와 Model 간의 의존성과, 시대가 지남에 따라 Controller의 역할이 이미 위젯에 포함되어 불필요해짐에 있습니다. View와 Controller의 역할, 즉 **UI와 사용자의 입력을 받는 Controller의 역할**을 위젯이 위임받아 액티비티(Activity)/프래그먼트(Fragment)에서 처리가 가능해졌기 때문입니다. 따라서 안드로이드의 경우 위의 모델으로는 구현할 수 없는 문제가 있습니다.

## MVP (Model-View-Presenter)
* **Presenter** - 사용자의 입력을 전달받은 View로부터 호출을 받고, Model에게 비즈니스 로직을 수행 및 데이터를 수신받으며, 다시 View에게 전달하여 UI를 업데이트합니다.
* MVP는 MVC의 문제를 개선하고자 사용자의 입력을 View에서 처리하도록 하고, 불필요한 Controller 대신 Presenter 컴포넌트가 추가된 구조입니다.

## MVVM (Model-View-ViewModel)
* **ViewModel** - View는 ViewModel의 데이터를 바인딩하여 UI를 업데이트하고, ViewModel은 Model을 바인딩하여 데이터를 가져옵니다.
* 프로그래밍 패러다임이 **반응형 프로그래밍**(Reactive Programming)으로 변화함에 따라, UI를 가진 프로그램에서 변경사항을 관찰(Observing)하고 개별적으로 문제를 해결할 수 있다는 것이 증명됐습니다.
### 주요 특징:
1. View는 ViewModel을 알고 있지만, ViewModel은 View에 대해 알지 못합니다. 마찬가지로 ViewModel은 Model을 알지만, Model은 ViewModel의 존재를 알지 못합니다. (참조가 없습니다)
2. ViewModel의 데이터를 트리거하여 View를 자동으로 업데이트하기 위해 Observer 패턴 구현 라이브러리(RxJava, Flow)를 사용합니다.
3. View를 업데이트하는 트리거 방법이 없습니다.(외부에서 View의 업데이트를 호출할 수 없습니다.)
### 안드로이드에 적용해보면:
1. **Xml 방식**: Data binding(데이터 바인딩)을 통해 ViewModel의 객체 변화를 UI에 반영하고 사용자의 입력을 ViewModel에 전달
2. **Compose 방식**: Flow, LiveData 등을 활용하여 Composable 내에서 데이터를 수집 및 UI에 반영하고, 사용자의 입력 등의 이벤트가 발생하면 ViewModel의 메서드를 호출
# 인텐트 및 인텐트 필터

## 인텐트 소개
구성 요소 유형 네 가지 중 세 가지(액티비티, 서비스, Broadcast Receiver)는 인텐트라는 비동기식 메시지로 활성화됩니다. (콘텐츠 제공자는 인텐트로 활성화되지 않습니다.) 이것은 일종의 메신저라고 생각하면 됩니다. 즉 구성 요소가 어느 앱에 속하든 관계없이 **다른 구성 요소로부터 작업을 요청하는 역할**을 합니다. 인텐트는 Intent 객체로 생성되며, 이것이 특정 구성 요소(명시적 인텐트)를 활성화할지 아니면 구성 요소의 특정 유형(암시적 인텐트)을 활성화할지 나타내는 메시지를 정의합니다.

## 명시적 인텐트(Explicit Intents)
시작하고자 하는 액티비티 또는 서비스의 클래스 이름을 알고 있을 때, 해당 구성 요소를 시작하기 위해 사용합니다.
```
// Executed in an Activity, so 'this' is the Context
// The fileUrl is a string URL, such as "http://www.example.com/image.png"
val downloadIntent = Intent(this, DownloadService::class.java).apply {
    data = Uri.parse(fileUrl)
}
startService(downloadIntent)
```

## 암시적 인텐트(Implicit Intents)
수행할 일반적인 작업을 선언하여 다른 앱의 구성 요소가 이를 처리할 수 있도록 해줍니다. 예를 들어 사용자에게 지도에 있는 한 위치를 표시하고자 하는 경우, 암시적 인텐트를 사용하여 해당 기능을 갖춘 다른 앱이 지정된 위치를 지도에 표시하도록 요청할 수 있습니다.
암시적 인텐트를 사용하면 Android 시스템에서 시작할 적절한 구성 요소를 찾습니다. 이때 인텐트의 내용을 기기에 있는 다른 여러 앱의 매니페스트 파일에서 선언된 인텐트 필터와 비교하는 방법을 사용합니다. 해당 인텐트와 일치하는 인텐트 필터가 있으면 시스템에서 해당 구성 요소를 시작하고 이를 Intent 객체를 전달합니다. 호환되는 인텐트 필터가 여러 개인 경우, 시스템에서 대화상자를 표시하여 사용자가 어느 앱을 사용할지 직접 선택할 수 있게 합니다.
```
// Create the text message with a string
val sendIntent = Intent().apply {
    action = Intent.ACTION_SEND
    putExtra(Intent.EXTRA_TEXT, textMessage)
    type = "text/plain"
}

// Verify that the intent will resolve to an activity
if (sendIntent.resolveActivity(packageManager) != null) {
    startActivity(sendIntent)
}
```
startActivity()를 호출하면 시스템이 설치된 앱을 모두 살펴보고 이런 종류의 인텐트를 처리할 수 있는 앱이 어느 것인지 알아봅니다(ACTION_SEND 작업이 있는 인텐트이며 "텍스트/일반" 데이터가 담긴 것). 이것을 처리할 수 있는 앱이 하나뿐이면, 해당 앱이 즉시 열리고 이 앱에 인텐트가 주어집니다. 인텐트를 허용하는 액티비티가 여러 개인 경우, 시스템은 아리의 그림과 같이 대화상자를 표시하여 사용자에게 앱을 선택하게 합니다.

## 인턴트 필터(Intent Filters)
인텐트 필터란 앱의 매니페스트 파일에 들어 있는 표현으로, 해당 구성 요소가 수신하고자 하는 인텐트의 유형을 나타냅니다. 예를 들어 액티비티에 대한 인텐트 필터를 선언하면 다른 여러 앱이 특정한 종류의 인텐트를 가지고 액티비티를 직접 시작할 수 있습니다. 마찬가지로 액티비티에 대한 인텐트 필터를 선언하지 않는 경우, 이것은 명시적인 인텐트로만 시작할 수 있습니다.
```
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```
각 인텐트 필터는 앱의 매니페스트 파일에 있는 `<intent-filter>`요소에서 정의하고, 이는 대응되는 앱 구성 요소에서 중첩됩니다(예: `<activity>` 요소). `<intent-filter>` 내부에서는 다음과 같은 세 가지 요소 중 하나 이상을 사용하여 허용할 인텐트 유형을 지정할 수 있습니다.
1. `<action>`: name 특성에서 허용된 인텐트 작업을 선언합니다. 이 값은 어떤 작업의 리터럴 문자열 값이어야 하며, 클래스 상수가 아닙니다.
예를 들어 허용된 인텐트 작업을 나타내기 위해 인텐트 필터는 0개 이상의 <action> 요소를 선언할 수 있습니다.
```
<intent-filter>
    <action android:name="android.intent.action.EDIT" />
    <action android:name="android.intent.action.VIEW" />
    ...
</intent-filter>
```
2. `<data>`: 허용된 데이터 유형을 선언합니다. 이때 데이터 URI(`scheme`, `host`, `port`, `path`)와 **MIME** 유형의 여러 가지 측면을 나타내는 하나 이상의 특성을 사용합니다.
예를 들어 허용된 인텐트 데이터를 나타내기 위해 인텐트 필터는 0개 이상의 <data> 요소를 선언할 수 있습니다.
```
<intent-filter>
    <data android:mimeType="video/mpeg" android:scheme="http" ... />
    <data android:mimeType="audio/mpeg" android:scheme="http" ... />
    ...
</intent-filter>
```
3. `<category>`: name 특성에서 허용된 인텐트 카테고리를 선언합니다. 이 값은 어떤 작업의 리터럴 문자열 값이어야 하며, 클래스 상수가 아닙니다.
예를 들어 허용된 인텐트 카테고리를 나타내기 위해 인텐트 필터는 0개 이상의 <category> 요소를 선언할 수 있습니다.
```
<intent-filter>
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    ...
</intent-filter>
```

# 매니페스트 파일
매니페스트의 주요 작업은 **시스템에 앱의 구성 요소에 대해 알리는 것**입니다. Android 시스템이 앱 구성 요소를 시작하려면 시스템은 우선 앱의 매니페스트 파일, AndroidManifest.xml을 읽어서 해당 구성 요소가 존재하는지 확인합니다. 앱은 이 파일 안에 모든 구성 요소를 선언해야 하며, 이 파일은 앱 프로젝트 디렉토리의 루트에 있어야 합니다.

## 구성 요소
다음 요소를 사용하여 모든 앱 구성 요소를 선언해야 합니다:

1. 액티비티의 경우 `<activity>` 요소
2. 서비스의 경우 `<service>` 요소
3. Broadcast Receiver의 경우 `<receiver>` 요소
4. 콘텐츠 제공자의 경우 `<provider>` 요소

## 매니페스트의 기타 역할
매니페스트는 앱의 구성 요소를 선언하는 것 이외에도 많은 역할을 합니다. 예를 들면 다음과 같습니다.

1. 앱이 요구하는 모든 사용자 권한(예: 인터넷 액세스, 사용자의 연락처에 대한 읽기 액세스)을 식별합니다.
2. 앱이 어느 API를 사용하는지를 근거로 앱에서 요구하는 최소 API 레벨을 선언합니다.
3. 앱에서 사용하거나 요구하는 하드웨어 및 소프트웨어 기능(예: 카메라, 블루투스 서비스, 멀티터치 화면)을 선언합니다.
4. 앱이 링크되어야 하는 API 라이브러리(Android 프레임워크 API 제외)(예: Google Maps라이브러리)를 선언합니다.

## 앱 리소스
Android 앱은 코드만으로 이루어지지 않습니다. 소스 코드와 별도로 이미지, 오디오 파일, 그리고 앱의 시각적 표현과 관련된 것 등의 여러 리소스가 필요합니다. 

예를 들어 액티비티 사용자 인터페이스의 애니메이션, 메뉴, 스타일, 색상, 레이아웃을 XML 파일로 정의해야 합니다. 

앱 리소스를 사용하면 **코드를 수정하지 않고 앱의 다양한 특성을 쉽게 업데이트**할 수 있습니다. 소스 코드와는 별개로 리소스를 제공하는 것의 가장 중요한 측면 중 하나는 **여러 가지 기기 구성에 맞게 대체 리소스를 제공**할 능력을 갖추게 된다는 점입니다. 