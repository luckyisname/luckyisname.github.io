---
title: Allure scalatest для scala 2.13
date: 2020-01-16
categories: 
- Автотесты на API
tags:
- scala
- scalatest
- allure 
---
У allure есть огромное количество интеграций для разных языков и фреймворков, в том числе и для [scalatest](http://www.scalatest.org/) - наиболее популярного тестового фреймворка для scala. 
Совсем недавно я добавил поддержку для allure-scalatest версии 2.13 к уже существующей 2.12. 
Я коротко расскажу как подключать и использовать [allure-scalatest](https://github.com/allure-framework/allure-java/tree/master/allure-scalatest).

## Как подключить
* Добавить зависимость в ```build.sbt```
```sbt
val allureScalaTestVersion = "2.13.3"
...
val allureScalaTest = "io.qameta.allure" % "allure-scalatest_2.13" % allureScalaTestVersion % Test
```

* Добавить testOptions для scalatest
```sbt
  testOptions in Test ++= Seq(
      Tests.Argument(TestFrameworks.ScalaTest, "-oD"),
      Tests.Argument(TestFrameworks.ScalaTest, "-C", "io.qameta.allure.scalatest.AllureScalatest")
    )
```
Подробно что эти опции делают можно почитать [тут](http://www.scalatest.org/user_guide/using_scalatest_with_sbt) 
Если коротко, то scalatest позволяет указать класс reporter-а. Reporter позволяет отображать результат тестов различными путями, чем мы пользуемся. 
 
> You can create classes that extend <code>Reporter</code> to report test results in custom ways, and to
> report custom information passed as an event "payload"

*  Для всех тестов необходимо прописать trait ```AllureScalatestContext```.
>As ScalaTest uses different thread for reporter you can't use Allure Java API until you populate context. So you need add AllureScalatestContext trait for each test

## Пример теста
```scala
import io.qameta.allure.Allure.{StepContext, step}
import io.qameta.allure.{Feature, Owner}
import io.qameta.allure.scalatest.AllureScalatestContext
import org.scalatest.flatspec.AnyFlatSpec

@Owner("viclovsky")
@Feature("feature")
class AllureApiSpec  extends AnyFlatSpec {

  "test" should "be passed" in new AllureScalatestContext {
    step("first")
    step("second", () => {
      step("child1")
      step("child2")
      step("child3")
      () =>
    })
    step("third", (context: StepContext) => {
      val a = context.parameter("a", 123L)
      val b = context.parameter("b", "hello")
          println(a)
          println(b)
      () =>
    })
  }
}
```

## Результат
Далее просто запускаем ```sbt test``` и генерируем отчет из папки ```allure-results```
![Alt text](/images/2020-05-18-allure-scalatest.png)
