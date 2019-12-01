# 객체 생성과 파괴
## 정적 팩토리 메소드
정적 팩토리 메소드란 해당 클래스의 인스턴스를 반환하는 단순한 정적 메소드 (static)입니다. 본 메소드가 public 객체 생성보다 좋은 점은 다음과 같습니다.

- 네이밍
- 매 호출 마다 새로운 객체 생성 방지
- 하위 타입 반환의 유연성
- 매개변수에 따른 다른 객체 반환
- 메소드를 작성하는 시점에 반환할 객체가 존재할 필요가 없다 (무슨 말인지 모르겠다)

### 네이밍
---
이름을 가지는 생성자를 만들 수 있습니다.
```
BigInteger(int, int, Random) // 어떠한 객체를 생성하는지 모호하다

BigInteger.probablePrime() // 솟수를 객체를 생성하는 것을 알 수 있다
```
이러한 구현 방식은 API를 사용하는 앤드 유저(프로그래머)로 하여금 잘못된 호출을 방지하는 역할합니다.

### 매 호출 마다 새로운 객체 생성 방지
---
미리 만들어진 인스턴스, 생성한 인스턴스의 캐싱 등을 통해 인스턴스를 재활용할 수 있다. 이는 생성 비용이 큰 객체의 생성에서의 성능을 극대화해준다. 

플라이웨이트 패턴 (FlyWeight Pattern)과 비슷한 역할을 수행한다.
> 빌더 클래스 내에서 공유할 객체를 static final로 생성하여 객체 생성 시, 할당해주는 기법

추가 정보
- 인스턴스 통제 클래스 - 인스턴스의 생성과 소멸을 통제할 수 있는 클래스
- 싱글턴 - 단일 객체를 보장하는 방식으로 인스턴스 통제 클래스로부터 만들 수 있다
- 인스턴스화 불가 
- 열거타입(enum)은 단일 인스턴스를 보장한다

### 하위 타입 반환의 유연성
본 방식을 이용하면 구현부를 감춘 채로 최소한의 API를 유지할 수 있습니다.
```
class Parent(){
    companion object{
        fun createChild() : Parent{
            return Child()
        }
    }
}

class Child() : Parent(){}
```

### 매개변수에 따른 다른 객체 반환
이는 하위 타입 반환의 유연성 내에 포함되는 내용입니다. 정적 패토리 메소드의 매개변수를 다르게 설정하여, 여러 타입의 하위 객체를 반환할 수 있습니다. 


## 빌더 패턴
---
> 코틀린에서는 빌더패턴을 고려할 필요성이 없습니다
> ```
> class Car(val model: String? = null, val year = 0)
> 
> var car = Car(model="소나타")
> ```
> 이렇게 인자를 선택으로 받으면 됩니다.


매개변수가 많을 때, 고려할 수 있는 객체 생성 기법은 다음과 같다.
- 점층적 생성자 패턴 : 모든 상황을 고려하여 생성자를 만든다 -> 생성자가 많아질 수 있음
- 자바빈즈 패턴 - 필수 파라미터만 받으며, 필요하면 setter를 통해 선택적 파라미터를 받는다  
-> 하나의 객체를 생성하는데 있어서 일관성을 보장할 수 없음

### 빌더 패턴
점층적 생성자 패턴과 자바빈즈 패턴의 장점만 취한 객체 생성 기법

```
class NutritionFacts(
    val servingSize : Int,
    val servings : Int,
    val calories : Int,
    val fat : Int
    ){
    private constructor(builder : Builder) : this(builder.servingSize, builder.servings, builder.calories, builder.fat)
    
    class Builder(servingSize:Int, servings:Int){
        val servingSize : Int
        val servings : Int
        
        var calories = 0
        	private set
        var fat = 0
        	private set
        
        fun calories(value : Int) = apply {this.calories = value}
		fun fat(value: Int) = apply {this.calories = value}
        
        init{
            this.servingSize = servingSize
            this.servings = servings
        }
        
        fun build() = NutritionFacts(this)
    }
}

fun main(){
    var car = NutritionFacts.Builder(2, 10).fat(4).build()
    println(car)
}
```

추가정보
- 코틀린에서 const는 컴파일 타임에 결정되는 객체를 지칭한다
- 코틀린에서 val은 런타임 시에 결정되는 객체를 지칭하며, java의 final과 동일하다

# 정리할 내용
- 싱글톤
- private 생성자
- 의존 객체 주입
- 불필요한 객체 생성 금지?
- 객체 참조 해제
- finalizer / cleaner 
- try-with-resource
