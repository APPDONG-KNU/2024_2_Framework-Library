# Navigation

NavHostController 사용 방법

1. **이동하고 싶은 Composable 이름을 구분하기** 

```kotlin
// Screen.kt
enum class CupcakeScreen() {
    Start,
    Flavor,
    Pickup,
    Summary
}
```

1. **NavHostController를 생성하여 받아온다.**
- rememberNavController를 기본 생성자로 두어, CupcakeApp 내에 NavHostController는 navController이다.
- **NavHost** 컴포저블은 내비게이션을 관리하는 **navController,** 시작 컴포저블을 의미하는 **startDestination,** 내부 경로를 의미하는 navGraph를 받는다.

```kotlin
@Composable
fun CupcakeApp(
    **navController: NavHostController = rememberNavController()**
) {
    Scaffold(
        topBar = {
            CupcakeAppBar(
                currentScreen = currentScreen,
                canNavigateBack = navController.previousBackStackEntry != null,
                navigateUp = { /* TODO: implement back navigation */ }
            )
        }
    ) { innerPadding ->
    
        **NavHost**(
            **navController** = navController,
            **startDestination** = CupcakeScreen.Start.name,
            modifier = Modifier.padding(innerPadding)
        ) {
            composable(route = CupcakeScreen.Start.name) { StartOrderScreen() }
            composable(route = CupcakeScreen.Flavor.name) { SelectOptionScreen() }
            composable(route = CupcakeScreen.Pickup.name) { SelectOptionScreen() }
            composable(route = CupcakeScreen.Summary.name) { OrderSummaryScreen() }
        }
    }
}
```

1. **다른 경로로 이동하기** 
- navController.navigate 메소드를 통해서 원하는 Composable로 이동할 수 있다.

```kotlin
// 위의 코드를 상세화
composable(route = CupcakeScreen.Start.name) {
                StartOrderScreen(
                    onNextButtonClicked = {
                        **navController.navigate(CupcakeScreen.Flavor.name)**
                    }
                )
            }

// StartOrderScreen.kt            
@Composable
fun StartOrderScreen(
    onNextButtonClicked: (Int) -> Unit,
) {
    Column(
        modifier = modifier,
        verticalArrangement = Arrangement.SpaceBetween
    ) {
        Column {
            quantityOptions.forEach { item ->
                SelectQuantityButton(
                    labelResourceId = item.first,
                    **onClick = { onNextButtonClicked(item.second) }**
                )
            }
        }
    }
}
```

```kotlin
    **navController.popBackStack(CupcakeScreen.Start.name, inclusive = false)
    // 이 함수는 1번째 인자까지 stack에서 pop하여 돌아가는 함수
    // inclusvie = true 라면, 지정된 값마저 pop 한다.**
```