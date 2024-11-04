# 데이터 클래스 만들기

## 1. 폴더
![img1](./images/1.png)

## 2. 데이터 클래스 타입 만들기
Affirmation.kt는 데이터 클래스 타입을 저장, Data class로 생성
```kotlin
//안드로이드에서는 리소스에 고유한 정수형 ID를 부여
data class Affirmation(
    val stringResourceId: Int,
    val imageResourceId: Int
)
```
```kotlin
import androidx.annotation.DrawableRes
import androidx.annotation.StringRes

//어노테이션을 이용해 타입 검증이 일어남
data class Affirmation(
    @StringRes val stringResourceId: Int,
    @DrawableRes val imageResourceId: Int
)
```

## 3. 데이터 클래스 만들기
Datasource.kt : 데이터를 저장

**클래스의 프로퍼티로 저장**

객체 생성 시 데이터가 생성 되어 유연성이 부족하고 메모리를 차지
```kotlin
//Datasource.kt
import com.example.affirmations.R
import com.example.affirmations.model.Affirmation

class Datasource {
    val affirmations = listOf(
        Affirmation(R.string.affirmation1, R.drawable.image1),
        Affirmation(R.string.affirmation2, R.drawable.image2),
        ...
    )
}

//MainActivity.kt
affirmationList = Datasource().affirmations
```

**메서드로 정의**

유연성: 메서드를 호출할 때마다 데이터를 생성할 수 있어, 필요에 따라 데이터를 동적으로 생성하거나 업데이트할 수 있습니다.

캡슐화: 데이터 생성 로직이 함수 내부에 감춰져 있어 코드의 유지보수와 확장이 용이합니다.

메모리 효율성: 필요할 때만 메서드를 호출해 데이터를 가져오기 때문에 메모리 관리가 효율적일 수 있습니다.

```kotlin
//Datasource.kt
import com.example.affirmations.R
import com.example.affirmations.model.Affirmation

class Datasource() {
    fun loadAffirmations(): List<Affirmation> {
        return listOf<Affirmation>(
            Affirmation(R.string.affirmation1, R.drawable.image1),
            Affirmation(R.string.affirmation2, R.drawable.image2),
            ...
        )
    }
}

//MainActivity.kt
affirmationList = Datasource().loadAffirmations()
```