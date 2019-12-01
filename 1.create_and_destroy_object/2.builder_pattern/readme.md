## 빌더 패턴
> 코틀린에서는 빌더패턴을 고려할 필요성이 없습니다
> ```
> class Car(val model: String? = null, val year = 0)
> 
> var car = Car(model="소나타")
> ```
> 이렇게 인자를 선택으로 받으면 됩니다.


매개변수가 많을 때, 고려할 수 있는 객체 생성 기법은 다음과 같다.
- 점층적 생성자 패턴 : 모든 상황을 고려하여 생성자를 만든다 -> 생성자가 많아질 수 있음
- 자바빈즈 패턴 : 필수 파라미터만 받으며, 필요하면 setter를 통해 선택적 파라미터를 받는다  
-> 하나의 객체를 생성하는데 있어서 일관성을 보장할 수 없음
- 빌더 패턴 : 점층적 생성자 패턴과 자바빈즈 패턴의 장점만 취한 객체 생성 기법

### 빌더 패턴
객체의 생성자를 내부로 숨기고(private), 객체의 생성을 하위 Static class에 위임하는 기법

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
