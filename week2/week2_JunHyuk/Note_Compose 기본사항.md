# Compose 기본 사항

### Compose vs View

- Compose와 View의 주요 차이점 : How가 아니라 What을 묘사하는지의 차이
- View : XML과 setter 함수, 상태가 변경되었을 때 뷰를 다시 구성하기 힘들고, 충돌이 발생할 수 있다.
- Compose : 선언형, UI element가 function이다. Recomposition을 통해 재렌더링을 한다.

### 상태 유지

*remember 함수는 컴포저블이 컴포지션에 유지되는 동안에만 작동*

*rememberSaveable 함수는 구성 변경(예: 회전)과 프로세스 중단에도 각 상태를 저장*

### Compose의 이벤트

![image.png](https://developer.android.com/static/codelabs/jetpack-compose-state/img/f415ca9336d83142_1920.png?hl=ko)

- Compose의 [`State`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/State?hl=ko) 및 [`MutableState`](https://developer.android.com/reference/kotlin/androidx/compose/runtime/MutableState?hl=ko) 유형을 사용하여 Compose에서 상태를 관찰
- **컴포지션:** 컴포저블을 실행할 때 Jetpack Compose에서 빌드한 UI에 관한 설명입니다.
- **초기 컴포지션:** 처음 컴포저블을 실행하여 컴포지션을 만듭니다.
- **리컴포지션:** 데이터가 변경될 때 컴포지션을 업데이트하기 위해 컴포저블을 다시 실행하는 것을 말합니다.

**State가 변경될 때마다 Recomposition이 발생한다.**

### 상태 호이스팅

- `remember`를 사용하여 객체를 저장하는 컴포저블에는 내부 상태가 포함되며 이는 컴포저블을 `스테이트풀(Stateful)`로 만듭니다. 이는 호출자가 상태를 제어할 필요가 없고 상태를 직접 관리하지 않아도 상태를 사용할 수 있는 경우에 유용합니다. 그러나 **내부 상태를 갖는 컴포저블은 재사용 가능성이 적고 테스트하기가 더 어려운 경향이 있습니다**.
- **상태를 보유하지 않는 컴포저블을 스테이트리스(Stateless) 컴포저블이라고 합니다**. 상태 호이스팅을 사용하면 **스테이트리스(Stateless)** 컴포저블을 쉽게 만들 수 있습니다.
- Compose에서 상태 호이스팅은 컴포저블을 스테이트리스(Stateless)로 만들기 위해 상태를 컴포저블의 호출자로 옮기는 패턴입니다. Jetpack Compose에서 상태 호이스팅을 위한 일반적 패턴은 상태 변수를 다음 두 개의 매개변수로 바꾸는 것입니다.

### 유용한 기능

- comp - @Composable
- prev - @Preview @Composable
- WR - Row { }
- WC - Column { }
- W - Box { }
- annotation class를 통해 custom annotation 생성 가능