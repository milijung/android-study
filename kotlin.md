# kotlin

## 널안정성(Null Safety)
Kotlin 변수는 기본적으로 null 값을 보유할 수 없습니다. 변수가 null값을 포함하려면 nullable 유형이어야 합니다.
* ```?```를 변수 유형의 접미사로 지정하여 변수를 nullable로 지정할 수 있습니다.
* 어설션 연산자 ```!!``` 연산자는 왼쪽에 있는 모든 것을 null이 아닌 것으로 취급하므로, 이 문을 실행할 때는 NullPointerException을 발생시킬 수 없습니다.

## 시퀀스(Sequence)
시퀀스는 대용량 데이터를 처리할 때 성능 및 메모리 효율성을 개선할 수 있으며, 중간 연산과 최종 연산을 이용하여 코드를 간결하게 유지할 수 있습니다.

1. **게으르게 처리(Lazy Evaluation)**: 시퀀스는 요청이 있을 때만 컬렉션 요소를 계산하고 반환합니다. 이로써 필요한 요소만 처리하므로 성능과 메모리 효율성을 향상시킬 수 있습니다.
2. **중간 연산(Intermediate Operations)**: 중간 연산은 시퀀스에서 데이터를 변환하거나 필터링하는 작업을 수행합니다. 예를 들어 map, filter, sorted 등이 중간 연산에 해당합니다.
3. **최종 연산(Terminal Operations)**: 최종 연산은 중간 연산을 통해 변환된 데이터를 가져와 결과를 반환하는 작업을 수행합니다. 예를 들어 toList, first, sum 등이 최종 연산에 해당합니다.
4. **무한 시퀀스(Infinite Sequences)**: 시퀀스는 무한한 요소를 포함할 수 있으며, 필요에 따라 무한한 데이터를 생성할 수 있습니다.
```
val numbers = sequenceOf(1, 2, 3, 4, 5)
val result = numbers
    .filter { it % 2 == 0 } // 중간 연산
    .map { it * it } // 중간 연산
    .toList() // 최종 연산

println(result) // [4, 16]
```

## 데이터 클래스(Data Class)
데이터를 단순하게 표현하기 위한 특수한 종류의 클래스입니다. 데이터 클래스는 주로 데이터 구조를 모델링하는 데 사용되며, 클래스 내에 데이터를 저장하고 이 데이터를 다루기 위한 메서드와 기능을 자동으로 생성하여 유용합니다. 데이터 클래스는 주로 **모델 클래스, DTO(Data Transfer Object), 데이터베이스 엔터티** 등에서 많이 활용됩니다.

1. **주요 생성자(Primary Constructor)**: 데이터 클래스는 주요 생성자를 통해 속성(프로퍼티)을 정의합니다. 이 속성은 주요 생성자의 매개변수로 선언됩니다.
2. **자동 생성 메서드**: 데이터 클래스는 다음과 같은 메서드를 자동으로 생성합니다.
    * equals(): 객체 간의 내용 동등성 비교를 지원합니다.
    * hashCode(): 해시 코드를 계산하여 해시 기반 컬렉션에서 사용할 수 있게 합니다.
    * toString(): 객체를 문자열로 표현하는 문자열 템플릿을 생성합니다.
    * copy(): 객체를 복사하면서 일부 속성을 변경할 수 있는 메서드를 생성합니다.
3. **구조 분해(Deconstruction)**: 데이터 클래스는 구조 분해를 지원하며, 속성을 변수에 풀어내어 사용할 수 있습니다.
4. **기타 메서드**: 데이터 클래스에서 추가 메서드를 정의할 수 있습니다.
```
data class Person(val name: String, val age: Int)

val person1 = Person("Alice", 30)
val person2 = Person("Bob", 25)

// equals() 메서드로 내용 동등성 비교
val result = person1 == person2 // false

// copy() 메서드로 객체 복사
val person3 = person1.copy(age = 28)

// 구조 분해
val (name, age) = person1

println(person1) // 출력: "Person(name=Alice, age=30)"
```

## 리시버(Receiver)
"리시버(Receiver)"라는 용어는 Kotlin 확장 함수(Extension Function)와 관련이 있습니다. Kotlin에서 확장 함수는 이미 존재하는 클래스에 새로운 함수를 추가할 수 있는 기능입니다. 이때, 확장 함수를 추가하는 대상이 되는 클래스의 인스턴스를 "리시버"라고 부릅니다.

리시버는 확장 함수가 적용될 클래스의 인스턴스를 가리킵니다. 이렇게 하면 이미 존재하는 클래스의 코드를 수정하지 않고도 새로운 함수를 추가할 수 있어 유용합니다.

예를 들어, String 클래스에 대한 확장 함수를 만들어 보겠습니다. 이 함수는 문자열을 거꾸로 뒤집는 역할을 합니다:
```
fun String.reverse(): String {
    return this.reversed()
}
```
위의 코드에서 String 클래스가 리시버입니다. reverse() 함수는 이미 존재하는 String 인스턴스에서 호출할 수 있고, 해당 인스턴스를 this 키워드를 통해 참조할 수 있습니다.

이제 이 확장 함수를 사용해보겠습니다:
```
val originalString = "Hello, World!"
val reversedString = originalString.reverse()

println(reversedString) // 출력: "!dlroW ,olleH"
```
여기서 originalString이 String 클래스의 인스턴스이고, reverse() 함수는 확장 함수로서 해당 인스턴스에 대해 호출되었습니다.

확장 함수와 리시버를 사용하면 기존 클래스에 대한 확장 기능을 추가하거나, 새로운 기능을 라이브러리로 제공할 때 유용하게 활용할 수 있습니다. Kotlin 표준 라이브러리와 여러 라이브러리에서 확장 함수와 리시버를 활용하여 더 풍부하고 간결한 코드를 작성하고 있습니다.

## 스코프 함수(scope functions)
객체의 범위 내에서 코드 블록을 실행하는 데 사용되는 특수한 함수입니다. 이러한 함수들은 주로 객체 초기화, 객체 조작 및 코드 블록 내에서의 특정 작업을 간편하게 수행할 때 유용합니다. Kotlin은 다음 다섯 가지 스코프 함수를 제공합니다:

1. **let**: let 함수는 it을 사용하여 수신 객체를 참조하고 람다 함수 내에서 추가 작업을 수행합니다. 주로 null 검사와 함께 사용됩니다.
```
val result = someValue?.let {
    // it은 someValue를 가리킴
    // 여기에서 it을 사용한 추가 작업 수행
    "Result is: $it"
}
```
2. **run**: run 함수는 수신 객체를 this를 사용하여 참조하며, 객체 초기화 블록과 같은 목적으로 사용됩니다.
```
val result = someObject.run {
    // this는 someObject를 가리킴
    // 여기에서 this를 사용한 추가 작업 수행
    "Result is: $this"
}
```
3. **with**: with 함수는 this를 사용하여 수신 객체를 참조하며, 주로 수신 객체에 대한 일련의 작업을 순차적으로 수행하는 데 사용됩니다.
```
val result = with(someObject) {
    // this는 someObject를 가리킴
    // 여기에서 this를 사용한 작업을 순차적으로 수행
    "Result is: $this"
}
```
4. **apply**: apply 함수는 수신 객체 자체를 반환하고, 객체의 속성 및 메서드에 대한 설정을 연이어 수행하는 데 사용됩니다.
```
val someObject = SomeClass().apply {
    property1 = "Value 1"
    property2 = "Value 2"
}
```
5. **also**: also 함수는 수신 객체를 반환하고 추가 작업을 수행하는 데 사용됩니다. 주로 중간 결과를 출력하거나 로깅에 활용됩니다.
```
val result = someObject.also {
    // 여기에서 추가 작업을 수행
}
```
이러한 스코프 함수들은 코드를 더 간결하게 만들고 객체 초기화 및 조작을 단순화하며, Kotlin에서 자주 사용되는 기능 중 하나입니다. 선택한 스코프 함수는 작업의 목적과 요구 사항에 따라 다를 수 있습니다.

## 컴패니언 객체(Companion Object)
클래스 내에 정의되며, 이 객체는 해당 클래스의 인스턴스 없이 직접 액세스할 수 있습니다. 컴패니언 객체를 사용하면 **클래스 수준의 멤버**를 관리하고 **클래스 내에서 공유 데이터 또는 동작**을 구현할 수 있습니다. 컴패니언 객체는 주로 팩토리 메서드, 상수, 정적 메서드, 패턴 등과 관련된 유틸리티 멤버를 클래스 내에서 관리하거나 공유할 때 사용됩니다.
```
class Person(val name: String) {
    companion object {
        const val TAG = "PersonTag"
        fun create() {
            println("A new person has been created.")
        }
    }
}

fun main() {
    // 컴패니언 객체의 상수에 액세스
    println("Tag: ${Person.TAG}")

    // 컴패니언 객체의 함수 호출
    Person.create()
}

```

## sealed class
클래스 계층 구조 내에서 상속에 제약을 두는 특수한 종류의 클래스입니다.  sealed class를 사용하면 클래스 계층 내의 하위 클래스가 제한되어 있으므로 예상치 않는 하위 클래스의 추가를 방지하고 **패턴 매칭** 등을 지원합니다. 이 클래스는 주로 **유한한 종류의 상태 또는 이벤트**를 표현하거나, 특정 유형의 결과를 반환하는 데 사용됩니다.
```
sealed class Result<out T>
data class Success<out T>(val data: T) : Result<T>()
data class Failure(val error: String) : Result<Nothing>()
```
이 Result 클래스는 성공(Success) 및 실패(Failure) 두 가지 하위 클래스만을 허용합니다. when 표현식을 사용하여 다음과 같이 패턴 매칭을 수행할 수 있습니다:
```
fun processResult(result: Result<String>): String {
    return when (result) {
        is Success -> "Success: ${result.data}"
        is Failure -> "Failure: ${result.error}"
    }
}
```

## 코루틴(Coroutines)
코루틴은 안드로이드에서 UI 스레드에서 실행시키기 힘든 **Network, SQLite Operation**과 같이 비동기적으로 실행되는 코드를 간결하게 만들어주기 위해 안드로이드에서 사용되는 실행 설계 패턴입니다. 코루틴에서 사용되는 Suspending functions은 일시 중지 가능한 함수로, 코루틴 내에서 사용되며 비동기 작업을 수행하는 동안 실행을 일시 중지하고 다른 작업을 처리할 수 있습니다.

1. 단일 스레드에서는 많은 코루틴을 실행할 수 있다.
    Dispatcher는 코루틴을 적당한 스레드에 할당하여 실행 도중 발생하는 pause나 resume을 담당한다.
    * Default : CPU를 많이 쓰는 작업에 최적화 되어있는 디스패쳐
    * IO : Input/Output의 약자처럼 이미지 다운로드, 파일 입출력 등과 같은 입출력 작업에 최적화 되어있는 디스패쳐
    * Main : 메인이라하면 역시 UI 아니겠는가? UI 작업과 찰떡궁합인 디스패쳐
2. 코루틴에서 다른 코루틴이 호출되면 그 코루틴을 차단하는게 아니라 잠시 정지시킨다. (**non-blocking**)
   반면 Thread는 코드가 끝날 때까지 다른 Thread를 블로킹한다.
3. 코루틴이 시작된 스레드를 중단하지 않으면서 비동기적으로 실행되는 코드
4. 코루틴은 **스코프 내**에서 실행되어야 한다.
    ```
    GlobalScope.launch {  }
    CoroutineScope(Dispatchers.IO).async {  }
    ```
    * GlobalScope - 특정 액티비티나 프래그먼트의 생명주기와 함께 동작해서 실행 도중 별도 생명주기 관리가 필요없다. 시작부터 종료까지 실행 시간이 비교적 긴 코루틴의 경우에 적합하다.
    * CoroutineScope - 필요할때만 열고 완료되면 닫아주는 코루틴 스코프에 사용하기 적합하다.
    * ViewModelScope - 뷰모델 컴포넌트를 사용한다면, ViewModel 인스턴스에서 사용하기 위해 제공되는 스코프이다. GlobalScope와 비슷하게, 뷰모델 인스턴스가 소멸될 때 자동으로 소멸되고 작업도 자연스럽게 취소된다.
5. 코루틴의 상태 관리 (코루틴 빌더를 이용한 코루틴 제어)
    * launch - 코루틴 시작
    * async -  코루틴 스코프의 결과를 받아서 쓸 수 있다. await()를 호출하여 기다렸다가 코드를 실행시킬 수 있게 된다.
        ```
        CoroutineScope(Dispatchers.Default).launch {
            val async1 = async{
                delay(500)
                200
            }
            val async2 = async{
                delay(1000)
                200
            }

            Log.d("Coroutine", "${async1.await()} + ${async2.await()}")
        }
        ```
    * cancel - 코루틴의 동작을 멈춤
    * join - 하나의 코루틴에는 여러개의 launch 블록이 있어도 상관이 없다. 다만 이렇게 launch 된 작업들은 모두 새로운 코루틴으로 분기가 되어서 실행되기 때문에 순서를 정하지 못한다. 다만 정 순서를 정해야 겠다 싶으면 join을 사용해서 순차적으로 실행되게 할 수 있다.
6. 코루틴은 생명주기에 따라서 onDestroy 시점에서 소멸될 때 관련 코루틴을 한번에 취소하여 메모리 누수를 방지한다.
7. withContext: withContext는 코루틴 내에서 다른 디스패처에서 실행되는 코드 블록을 실행하는 데 사용된다.

## 스마트 캐스트(Smart Cast)
변수 또는 프로퍼티의 타입을 컴파일러가 자동으로 추론하여 안전하게 형 변환하는 메커니즘입니다. 
```
fun printLength(text: Any) {
    if (text is String) {
        // text는 스마트 캐스트로 자동으로 String 타입으로 변환됨
        println("Length: ${text.length}")
    }
}
```
위 코드에서 text는 Any 타입으로 전달되지만 if 문에서 is String으로 확인될 때 스마트 캐스트가 발생하여 text를 자동으로 String으로 변환합니다. 이로써 안전하게 문자열의 길이를 출력할 수 있습니다.

스마트 캐스트는 코드의 가독성을 향상시키고 타입 안전성을 보장하는 데 매우 유용한 기능입니다.

## 고차함수(Higher-Order Function)
함수를 인자로 받거나 함수를 반환하는 함수를 말합니다. 고차함수는 함수형 프로그래밍 패러다임을 지원하며 코드의 재사용성과 추상화를 높이는 데 유용합니다. 함수는 **(input 클래스) -> output 클래스** 의 형식으로 표현합니다.
```
fun stringMapper(str: String, mapper: (String) -> Int): Int {
    // Invoke function
    return mapper(str)
}

// function call 1
stringMapper("Android", { input ->
    input.length
})

// function call 2 - only if the function is the last parameter 
stringMapper("Android") { input ->
    input.length
}
```

## 인라인함수(Inline Function)
인라인 함수는 일반적으로 작은 함수나 람다 표현식과 함께 사용됩니다. 함수 호출이 반복되는 루프나 컬렉션 처리와 같은 상황에서 특히 유용합니다. 이는 함수 호출 오버헤드를 줄이고 성능을 최적화하며, 코드의 재사용성과 가독성을 유지하는 데 도움이 됩니다.
```
inline fun calculateResult(a: Int, b: Int): Int {
    return a + b
}

fun main() {
    val result = calculateResult(3, 4)
    println("Result: $result")
}
```

## lateinit Proterty vs lazy
lateinit 프로퍼티와 lazy 함수는 Kotlin에서 지연 초기화를 달성하기 위한 두 가지 주요 방법입니다. 각각의 사용 사례와 특징에 대해 알아봅시다:

1. **lateinit 프로퍼티**:
lateinit은 클래스의 프로퍼티에 사용되며, 변수를 선언할 때 초기화를 미룹니다.
lateinit은 var 프로퍼티에만 사용할 수 있으며, null 초기화가 허용됩니다.
초기화하지 않고 사용하려고 시도하면 UninitializedPropertyAccessException이 발생합니다.
주로 의존성 주입 및 안드로이드 뷰 바인딩과 같이 객체 초기화에 사용됩니다.
```
lateinit var name: String

fun initializeName() {
    name = "John"
}
```
2. **lazy 함수**:
lazy 함수는 변수가 처음으로 필요할 때만 초기화됩니다.
lazy는 val 프로퍼티에 사용하며, 초기화 블록을 람다로 정의합니다.
초기화되는 값은 val로 선언되어 변경되지 않는 값입니다.
lazy는 **변수에 처음 액세스할 때** 초기화되며 이후로는 동일한 값이 반환됩니다.
```
val name: String by lazy {
    "John"
}
```
lateinit은 주로 **클래스 프로퍼티의 지연 초기화**에 사용되며, 초기화가 필요한 시점에서 직접 값을 할당합니다. lazy는 **val 프로퍼티의 지연 초기화**에 사용되며, 초기화 블록 내에서 값을 계산하거나 다른 복잡한 초기화 로직을 수행할 수 있습니다.

## 확장 프로퍼티(Extension Property)
기존 클래스에 새로운 프로퍼티를 추가하는 방법 중 하나입니다. 확장 프로퍼티는 이미 존재하는 클래스에 대해 사용자 정의된 getter와 setter를 제공하며, 이를 통해 클래스의 인스턴스에 새로운 프로퍼티를 마치 기본 프로퍼티처럼 사용할 수 있습니다. 확장 프로퍼티를 사용하면 기존 클래스에 새로운 동작을 추가하거나 코드를 더 읽기 쉽게 만들 수 있습니다.

예를 들어, 다음은 String 클래스에 대한 확장 프로퍼티인 isPhoneNumber를 정의하는 예제입니다:
```
val String.isPhoneNumber: Boolean
    get() = matches(Regex("\\d{3}-\\d{3}-\\d{4}"))

```

위의 확장 프로퍼티를 사용하면 문자열이 전화번호와 일치하는지 확인할 수 있습니다. 예를 들어:
```
val phoneNumber = "123-456-7890"
val notPhoneNumber = "Hello, World!"

println(phoneNumber.isPhoneNumber)   // true
println(notPhoneNumber.isPhoneNumber) // false
```

주의할 점은 확장 프로퍼티는 실제로는 프로퍼티가 아니라 getter와 setter 메서드를 정의하는 것이며, 기본 클래스의 내부 상태를 변경하지 않습니다. 또한, 확장 프로퍼티는 원본 클래스의 상속이나 수정 없이 새로운 동작을 추가하는 데 사용됩니다.

## Android KTX(Android Kotlin Extensions)
안드로이드 앱 개발을 위한 Kotlin 확장 라이브러리입니다. Android KTX는 Kotlin 언어의 강력한 특징을 활용하여 Android 앱을 개발하는 데 도움을 주며, Android API 및 프레임워크와 상호 작용하는 데 더 간편하고 가독성 있는 방법을 제공합니다.

1. **확장 함수 및 프로퍼티**: Android KTX는 Android API를 향상시키는 확장 함수 및 프로퍼티를 제공합니다. 예를 들어, View 클래스의 setOnClickListener 메서드를 람다 함수로 대체하여 코드를 간결하게 만들 수 있습니다.
2. **null 안전성**: Android KTX는 Kotlin의 null 안전성 특징을 활용하여 null 값을 처리하고 더 안전한 코드를 작성하는 데 도움을 줍니다.
3. **가독성 향상**: Kotlin의 가독성 향상을 활용하여 Android 코드를 더 읽기 쉽게 만듭니다. 예를 들어, ViewModel 및 LiveData 사용 시 코드가 더 간결해집니다.
4. **리소스 관리**: 리소스 ID를 사용하기 위한 확장 함수와 프로퍼티를 제공하여 리소스 관리를 단순화하고 오류를 줄입니다.
5. **AndroidX 라이브러리와 통합**: Android KTX는 AndroidX 라이브러리와 통합되어 최신 Android API와 호환됩니다.

## Kotlin과 안드로이드의 메모리 누수를 방지하기 위한 주요 전략
Kotlin과 안드로이드 앱에서 메모리 누수를 방지하기 위한 몇 가지 주요 전략은 다음과 같습니다:

1. **약한 참조(Weak Reference) 사용**: 객체가 더 이상 필요하지 않을 때 메모리에서 해제되도록 약한 참조를 사용하세요. 이를 통해 객체가 가비지 컬렉션될 수 있게 되어 메모리 누수를 방지할 수 있습니다.
2. **컨텍스트(Context) 관리**: 안드로이드 앱에서 컨텍스트는 메모리 누수의 주요 원인 중 하나입니다. 컨텍스트 참조를 과도하게 유지하지 말고, 액티비티나 프래그먼트 수명주기와 관련된 컨텍스트에 대한 참조를 조심스럽게 다루세요.
3. **액티비티 및 프래그먼트 릴리스**: 액티비티 및 프래그먼트와 관련된 리소스를 릴리스하고, 수명주기 메서드를 올바르게 구현하여 메모리 누수를 방지하세요. 액티비티나 프래그먼트가 더 이상 사용되지 않을 때 관련 리소스를 해제하십시오.
4. **코루틴 및 스레드 관리**: 코루틴이나 스레드를 사용할 때, 취소되지 않은 작업 또는 무한 루프를 주의하세요. 앱이 백그라운드로 이동하거나 액티비티가 종료되는 경우에도 취소되지 않은 작업을 관리하세요.
5. **인터페이스 및 리스너 해제**: 인터페이스 및 리스너를 등록한 경우, 더 이상 필요하지 않을 때 명시적으로 해제하십시오. 그렇지 않으면 객체가 참조를 계속 유지하고 메모리 누수를 유발할 수 있습니다.
6. **캐시 관리**: 캐시된 데이터나 리소스를 적절하게 관리하세요. 더 이상 사용되지 않는 데이터를 주기적으로 삭제하고, 캐시 사이즈를 제한하세요.
7. **LeakCanary와 같은 도구 사용**: 메모리 누수를 감지하기 위해 LeakCanary와 같은 도구를 사용하세요. 이러한 도구는 메모리 누수를 자동으로 식별하고 앱의 문제를 보고합니다.
8. **프로파일링 및 메모리 분석**: 안드로이드 스튜디오와 같은 개발 도구를 사용하여 앱의 메모리 사용을 분석하고 프로파일링하세요. 이를 통해 메모리 누수를 식별하고 해결할 수 있습니다.