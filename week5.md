## Navigation vs 상태변경

상태변경을 이용한 화면전환 예시
```kotlin
@Composable
fun ReplyHomeScreen(
    replyUiState: ReplyUiState,
    onEmailCardPressed: (Int) -> Unit = {}
) {
    if (replyUiState.isShowingHomepage) {
        ReplyAppContent(
            replyUiState = replyUiState,
            onEmailCardPressed = onEmailCardPressed

        )
    } else {
        ReplyDetailsScreen(
            replyUiState = replyUiState,
            onBackPressed = onDetailScreenBackPressed
        )
    }
}
// 버튼 클릭시 isShowingHomepage가 true 또는 false로 변경
```
isShowingHomepage 상태가 변경되면, Compose가 리컴포지션(recomposition)을 통해 화면을 다시 그려 새로운 상태에 맞는 화면이 조건에 따라 표시

![0](https://github.com/user-attachments/assets/b388205e-5686-42bc-b1ee-f161ad90f7cd)

상태변경을 이용한 화면전환은 뒤로가기를 지원하지 않기 때문에 따로 구현을 해야한다
```kotlin
fun ReplyDetailsScreen(
    replyUiState: ReplyUiState, // 현재 화면
    modifier: Modifier = Modifier,
    onBackPressed: () -> Unit = {},
) {
    BackHandler {   // 뒤로 가기 버튼을 눌렀을 때 동작
        onBackPressed()
    }
}
// 버튼 클릭시 isShowingHomepage가 true 또는 false로 변경
```

## 다양한 화면 크기에 맞게 레이아웃 조정


중단점 : 반응형 앱 개발에서 특정 크기나 상태에 맞춰 레이아웃을 전환하도록 도와주는 기준값

![1](https://github.com/user-attachments/assets/8056b4cd-fa83-4c82-9c68-be3e9a3e987a)


Columns : 화면에 콘텐츠를 최대 n개의 열에 배치

Minimum margins : 화면의 가장자리와 콘텐츠 사이의 최소 여백 (dp)

```kotlin
//사용 예시
windowSize = windowSize.widthSizeClass // windowSize에 현재 화면의 sizeclass 저장

@Composable
fun MyResponsiveApp(windowSize: WindowWidthSizeClass) {
    when (windowSize) {
        WindowWidthSizeClass.Compact -> {
            // Compact 화면에 맞춘 레이아웃을 설정
            CompactLayout()
        }
        WindowWidthSizeClass.Medium -> {
            // Medium 화면에 맞춘 레이아웃을 설정
            MediumLayout()
        }
        WindowWidthSizeClass.Expanded -> {
            // Expanded 화면에 맞춘 레이아웃을 설정
            ExpandedLayout()
        }
    }
}
```

### 화면 크기에 맞게 미리보기

일반적인 방법의 @Preview 사용 시 원하는 크기로 보여주지 않기 때문에 preview의 사이즈를 설정해야 한다

Compact: widthDp = 400, heightDp = 800

Medium: widthDp = 700, heightDp = 1000

Expanded: widthDp = 1000, heightDp = 1200
```kotlin
@Preview(showBackground = true, widthDp = 1000, heightDp = 1200)
@Composable
fun ReplyAppCompactPreview() {
    ReplyTheme {
        Surface {
            ReplyApp(
                windowSize = WindowWidthSizeClass.Expanded,
            )
        }
    }
}
```
![2](https://github.com/user-attachments/assets/3a7dfd5a-1127-42d2-9ae0-72201a854b6d)


