To create a UI component in Compose, we must annotate a function with **@Composable** annotation.

-> tells the Compose compiler that this function is intended to convert data into UI.


## Composition
```kotlin
// SurveyAnswer.kt

@Composable
fun SurveyAnswer(answer: Answer) {
  Row {
    Image(answer.image)
    Text(answer.text)
    RadioButton(false, onClick = { /* ... */ })
  }
}
```

```kotlin
// SurveyAnswer.kt

@Composable
fun SingleChoiceQuestion(answers: List<Answer>) {
  Column {
    if (answers.isEmpty()) {
      Text("There are no answers to choose from!")  // we can display something else conditionally
    } else {
      answers.forEach { answer ->
        SurveyAnswer(answer = answer)
      }
    }
  }
}
```
this function takes in a list of answers and then call the SurveyAnswer. No returns, emits UI.

## Recomposition
```kotlin
@Composable
fun SingleChoiceQuestion(answers: List<Answer>) {
  var selectedAnswer: MutableState<Answer?> =
    mutableStateOf(null)                        // no answer selected initally
    // remember { mutableStateOf(null) }        // if use this line instead upper line, guarantees that the value is remembered and not reset when the composable recomposes
  answers.forEach { answer ->
    SurveyAnswer(
      answer = answer,
      isSelected = (selectedAnswer.value == answer),    // update the value of isSelected
    )
  }
}
```

if use this delegated property syntax by using the by keyword,
```kotlin
@Composable
fun SingleChoiceQuestion(answers: List<Answer>) {
  var selectedAnswer: Answer? by
  rememberSaveable { mutableStateOf(null) }
  answers.forEach { answer ->
    SurveyAnswer(
      answer = answer,
      isSelected = (selectedAnswer == answer),
      onAnswerSelected = { answer -> selectedAnswer = answer }  // can pass a lambda function for the onAnswerSelected parameter
    )
  }
}
```
