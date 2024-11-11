# 아키텍처 및 상태

## UI가 그려지는 과정

![image.png](image.png)

![image.png](image%201.png)

자식 노드의 크기를 측정하고, 부모에서 자식의 노드를 받아 자기의 크기를 결정하고, 어떻게 레이아웃을 배치할 지 결정한다.

### 컴포저블 매개변수 정의

컴포저블의 상태 매개변수를 정의할 때는 다음 사항을 고려해야 합니다.

- 컴포저블의 재사용 가능성 또는 유연성
- 상태 매개변수가 컴포저블의 성능에 미치는 영향

분리 및 재사용을 유도하기 위해 각 컴포저블에는 가능한 한 최소한의 정보를 포함해야 합니다. 예를 들어 뉴스 기사의 헤더를 저장하는 컴포저블을 빌드하는 경우 전체 뉴스 기사가 아니라 표시해야 하는 정보만 전달합니다.

```kotlin
@Composable
fun Header(title: String, subtitle: String) {
    // Recomposes when title or subtitle have changed.
}

@Composable
fun Header(news: News) {
    // Recomposes when a new instance of News is passed in.
}ArchitectureSnippets.kt

```

개별 매개변수를 사용하면 성능이 향상되는 경우도 있습니다. 예를 들어 `News`에 `title` 및 `subtitle`보다 더 많은 정보가 포함된 경우 `title` 및 `subtitle`이 변경되지 않았더라도 `News`의 새 인스턴스가 `Header(news)`에 전달될 때마다 컴포저블이 재구성됩니다.

전달하는 매개변수의 수를 신중하게 고려하세요. 함수에 매개변수가 너무 많으면 함수의 인체공학적 특성이 감소하므로 이 경우 매개변수를 클래스로 그룹화하는 것이 좋습니다.

## Composable의 State

- State가 바뀌면 Composable의 Recomposition 발생
- State를 선언하는 방법
    - State<T>
    - MutableState<T>
    - State 선언 **잘못된** 예시
    
    ![image.png](image%202.png)
    
    - **상태를 기억**할 수 있도록 **remember 안에 객체로 선언**
    - **rememberSaveable** 선언하면 액티비티와 프로세스가 다시 생성되어도 저장된 값이 남아있음
    
    ![image.png](image%203.png)
    

## State Hoisting (상태 끌어올리기)

- CartItem 내에 quantity를 선언하여 (Stateful 하게) 사용하는 것이 아니라, 다음 예시처럼 **State, Event**를 전달받아서 (Stateless 하게) 상태 변화를 윗단으로 끌어올리기

![image.png](image%204.png)

- CartViewModel → Cart  → CartItem

![image.png](image%205.png)

- UI에 관한 용어 설명

![image.png](image%206.png)

## StateHolder

- 말 그대로 State를 들고 있는 Class
- State , UI Logic Tracking 할 때 덜 복잡하게 사용 가능

![image.png](image%207.png)

## ViewModel

- Compose에서의 Side-Effect란 Composable 함수 바깥에서 발생하는 State 변화를 의미