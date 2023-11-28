# Java 프로젝트에 Kotlin 얹기

`build.gradle` 파일에 해당 부분 추가
```gradle
plugins {
  ...
  id 'org.jetbrains.kotlin.jvm' version '{버전명시}'
  ...
}

dependencies {
  ...
  // 코틀린
  implementation 'org.jetbrains.kotlin:kotlin-stdlib:{버전}'
  // 코루틴
  implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:{버전}'
  runtimeOnly 'org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:{버전}'
}
```