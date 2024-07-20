# Spring Boot 프로젝트 설정

## Java 프로젝트 설정

### Kotlin 설정추가

`plugin` 부분에 해당 부분을 추가

```groovy
// build.gradle
plugin {
    id 'org.jetbrains.kotlin.jvm'
    id 'org.jetbrains.kotlin.plugin.spring' version '${코틀린 버전}'
    id 'org.jetbrains.kotlin.plugin.jpa' version '${코틀린 버전}'
}
```

```groovy
// build.gradle.kts
plugin {
  kotlin("jvm") version "1.9.22"
  kotlin("plugin.spring") version "1.9.22"
  kotlin("plugin.jpa") version "1.9.22"
}
```

## Spring Boot 프로젝트 생성시 추가하면 좋은 Dependency

### `Spring Boot Dev Tools`

dev tools는 Spring Boot에서 제공하는 개발 편의를 위한 모듈이다.

크게 다음과 같은 기능을 제공해준다.

- Automatic Restart
- Live Reload
- Global Settings
- Remote Applications

쉽게 말하면 브라우저로 전송되는 내용들에 대한 코드가 변경되면, 자동으로 어플리케이션을 재시작하여 브라우저에도 업데이트를 해주는 역할을 한다.

---

<br>

### `Spring Configuration Processor`

`@Value`를 통해서 `appliction.yaml`파일에 있는 property에 접근할수 있다. 하지만 해당 property가 필요한 곳에서 모두 선언해줘야하는 불편함이 존재한다.

`Spring Configuration Processor`는 설정을 객체로 만들어서 관리하기 편하게 해준다.

예시 코드

```yaml
jwt:
  sercret: secret
```

`application.yaml` 파일에 다음과 같은 속성이 있을때, 다음과 같이 `Bean`으로 만들어서 관리하면 편하다

```kotlin
@Configuration
@ConfigurationProperties(prefix = "jwt")
class JwtConfig {
  lateinit var secret: String
}

// 사용
@RestControllter
@RequestMapping("/test")
class TestController(val jwtConfig:JwtConfig){

  @GetMapping
  fun test(){
    println(jwtConfig.secret) // secret
  }
}
```

---

### `Kotlin JDSL`

코틀린에서 QueryDSL 설정하는 것은 매우 번거로운 일이다. `QueryDSL` 대용으로 라인에서 만든 `Kotlin JDSL`을 사용하면 좋다.

```groovy
dependecy {
  implementation("com.linecorp.kotlin-jdsl:jpql-dsl:{버전}")
  implementation("com.linecorp.kotlin-jdsl:jpql-render:{버전}")
}
```

## Spring JPA 관련 추가 설정

`JPA`를 사용중이라면 해당 설정도 추가해야함

```groovy
// build.gradle

// JPA 엔티티를 생성하려면 기본 생성자가 있어야하므로,
// Entity 관련 어노테이션이 달린 class들에게 기본 생성자를 만들어줘야한다.
noArg {
    annotation 'javax.persistence.Entity'
    annotation 'javax.persistence.Embeddable'
    annotation 'javax.persistence.MappedSuperclass'
}

// JPA 엔티티는 Proxy객체를 만들어서 사용하기 때문에 final class가 아니여야한다.
// allOpen 플러그인을 통해 엔티티 클래스들을 모두 open 시켜준다.
allOpen {
    annotation 'javax.persistence.Entity'
    annotation 'javax.persistence.Embeddable'
    annotation 'javax.persistence.MappedSuperclass'
}

```

```groovy
// build.gradle.kts

allOpen {
  annotation("jakarta.persistence.Entity")
  annotation("jakarta.persistence.Embeddable")
  annotation("jakarta.persistence.MappedSuperclass")
}

noArg {
  annotation("jakarta.persistence.Entity")
  annotation("jakarta.persistence.Embeddable")
  annotation("jakarta.persistence.MappedSuperclass")
}
```

## QueryDSL

QueryDSL는 코틀린에서 사용할수 없고 자바 프로젝트에서만 사용이 가능하다.

다음 디펜던시를 추가해서 QueryDSL을 프로젝트 추가할수 있다.

```groovy
dependencies {
  ...
  implementation 'com.querydsl:querydsl-jpa:${querydsl_version}:jakarta'
  annotationProcessor 'com.querydsl:querydsl-apt:${querydsl_version}:jakarta'
  annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
  ...
}

// QueryDSL generated sources setting
tasks.withType(JavaCompile) {
    options.generatedSourceOutputDirectory = file("src/main/generated")
}


// add QClass in sourceSets
sourceSets {
    main {
        java {
            srcDirs += "src/main/generated"
        }
    }
}

// clean generated sources when clean task is executed
clean {
    delete fileTree('src/main/generated')
}


```
