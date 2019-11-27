---
title: Пять простых способов сделать espresso тесты лучше и немного об allure-android
date: 2019-12-02
categories: 
- Автотесты на APP
tags:
- kotlin
- allure
- android
- espresso
---
Несмотря на то что espresso в первую очередь нацелен на разработчиков, сами тесты можно сделать более дружелюбными и удобными как для ручного тестирования, так и для людей никак с тестированием не связанных. 
Это можно сделать как с точки зрения кода, так и с точки зрения запуска почти без использования сторонних библиотек. 
Поэтому представляю **пять простых способов** сделать espresso тесты лучше. 

## Page-object/робот паттерн в APP

Чаще всего в документации espresso тесты выглядят так:
```kotlin
@Test
fun greeterSaysHello() {
    onView(withId(R.id.name_field)).perform(typeText("Steve"))
    onView(withId(R.id.greet_button)).perform(click())
    onView(withText("Hello Steve!")).check(matches(isDisplayed()))
}
```
Для UI тестов очень важно разделять элементы на странице(экране телефона) и действия которые могут с ними происходить. 
Тесты становятся легче читать и поддерживать, так как они становятся похожи на сценарии.
Таким образом получится что-то вроде такого сценария:
```kotlin
@Test
fun greeterSaysHello() {
   onMainScreen {
       interactions.nameField().perform(typeText("Steve"))
       interactions.greetButton().perform(click())
    }.check {
       interactions.textField("Hello Steve!").check(matches(isDisplayed()))
    }
}
```

Тут был использован чуть-чуть доработанный [jake-wharton робот паттерн](https://academy.realm.io/posts/kau-jake-wharton-testing-robots/).

## Ожидания

Из коробки espresso предоставляет механизм Idling Resources. 
Это универсальный механизм, который должен решать все проблемы с ожиданием асинхронных операций. 
Но не на всех проектах это может работать. 
Мало кто в здравом уме будет вносить изменения в код приложения ради тестов, как это рекомендует официальная документация:
> When adding idling resources into your app, we highly recommend performing only the registration and unregistration operations in your tests and placing the idling resource logic itself in your app's production code.

Самый простой способ добавить "retry" wrapper для своих целей. 
Например периодически выполнять check() - до тех по пока не будет выкидываться Exception.
Этот способ не рекомендован официальной документацией, но легок в применении и прекрасно работает для большинства ситуаций.
Например, для предыдущего теста, можно добавить расширение waitUntil:

```kotlin
textField("Hello Steve!").waitUntil(matches(isDisplayed()))
```

где waitUntil - вот такое простое расширение для ViewInteraction:

```kotlin
fun ViewInteraction.waitUntil(assertion: ViewAssertion): ViewInteraction = this.apply {
    retry { check(assertion) }
}
```

## Параллельное выполнение тестов
 
Несмотря на то что espresso тесты, относительно быстрые, настанет момент, когда их количество перевалит за сотню, а может и тысячу.
Что делать, если тестов стало слишком много?
В android документации есть стандартный способ шардинга [тестов из коробки](https://developer.android.com/training/testing/junit-runner#sharding-tests):
```
adb shell am instrument -w -e numShards 10 -e shardIndex 2
```
По факту, для того, чтобы расспараллелить тесты на n device-ов, достаточно запустить эту комманду n раз с numShard = n и
shardIndex = {i}, указав при этом "-s" = device{i}.
Также можно поробовать использовать кастомный раннер или написать свой.

## Ретраи тестов

Несмотря на то что espresso тесты довольно стабильны, все равно бывают случаи, когда они падают без видимой на то причины. 
Никто не любит flaky тесты. 
Из коробки тесты не ретраятся, но есть возможность запускать [отдельные тесты](https://developer.android.com/studio/test/command-line#RunTestsDevice). 
```
adb shell am instrument -w <test_package_name>/<runner_class>
```
Самый простой способ как ретраить тесты -  сохранить куда-нибудь падающие тесты и уже потом их запустить заново. 

## Отчет

Gradle репорт в espresso тестах из коробки предоставляется по умолчанию. Он не очень информативный и его нет возможности кастомизировать. 
Из него понятно какие тесты упали, но не всегда понятно на каком этапе и почему. 
![Alt text](/images/2019-12-02-gradle-report.png)

На всех моих проектах используется allure-framework. Это крутой opensource framework для построения отчета с множеством интеграций.
В качестве отчета для espresso тестов я использую [allure-android](https://github.com/allure-framework/allure-android) - это allure интеграция для android. Представляет из себя listener, который сохраняют информацию для посторояния отчета на sdcard-е девайса, по которой уже строится отчет. Allure-android никак не привязан к раннеру, его можно использовать как дополнительный или основной отчет.
Он отлично ложиться на паттерн page-object/robot. 
Немного закастомизировав стандартный ViewInteraction и заоверрайдив метод perform, можно получить такой отчет для android приложения:

![Alt text](/images/2019-12-02-allure-android.png)

Из плюсов - можно отметить полную кастомизацию - любой файл можно прикрепить к данному тесту в отчете. 
Дополнительно к скриншоту в allure-android-е (в случае падения теста) я добавил adb log  и иерархию элементов.
И конечно все крутые возможности и фичи allure остались. Одни из самых полезных - графики со статистикой, историей и ретраями.









