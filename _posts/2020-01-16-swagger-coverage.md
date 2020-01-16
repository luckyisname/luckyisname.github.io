---
title: Swagger-coverage что это и для чего нужно
date: 2020-01-16
categories: 
- Автотесты на API
tags:
- java
- swagger
---
[Swagger-coverage](https://github.com/viclovsky/swagger-coverage) это инструмент для анализа, который нужен для получения картины о "покрытии" регрессионными тестами на основе OAS 2 (Swagger). Говоря о покрытии, имеется ввиду не функциональсть, а именно наличие вызовов с определенными API методами, параметрами и получение всех кодов ответов, которые соответсвуют спецификации тестируемого API.
![Alt text](/images/2020-01-16-swagger-coverage.png)

# Когда это может быть полезно
* Когда распределенная команда занимается автоматизацией тестирования и нужно получить анализ прогресса автоматизации.
* При выборе стратегии автоматизации. Например: либо покрывать api методы на которых совсем нет тестов, либо те, где есть частичное покрытие.
* Для отслеживания добавления новых сущностей(API методы/параметры/и прочее), на которых нет автотестов.

# Как работает
Swagger-coverage состоит из двух частей.
* Фильтры/интерсепторы для клиентов (на данный момент один фильтр для rest-assured). С помощью фильтров собираем всю информацию о ходе выполнения тестов. 
* Консольная приложение. Консольное приложение агрегирует собранную метаинформацию и строит понятный репорт.

Сама архитектура была вдохновленна allure-ом.

# Добавление фильтра
Сначала нужно добавить фильтр в клиент. Добавить репозиторий в ваш pom.xml
```xml
 <repositories>
        <repository>
            <id>viclovsky</id>
            <url>https://dl.bintray.com/viclovsky/maven/</url>
        </repository>
 </repositories>
```
А также зависимость для фильтра (rest-assured)
```xml
<dependency>
     <groupId>com.github.viclovsky.swagger.coverage</groupId>
     <artifactId>swagger-coverage-rest-assured</artifactId>
     <version>${latest-swagger-coverage-version}</version>
 </dependency>
 ```
Для gradle аналогично:
 ```
repositories {
    maven { url 'https://dl.bintray.com/viclovsky/maven' }
}
 ```
и
 ```
compile "com.github.viclovsky.swagger.coverage:swagger-coverage-rest-assured:$latest-swagger-coverage-version"
 ```
Далее добавляем фильтр к клиенту:
 ```java
RestAssured.given().filter(new SwaggerCoverageRestAssured())
 ```
Фильтр создаст папку по умолчанию ```swagger-coverage-output``` с метаинформацией для построения отчета о покрытии.

# Построение отчета
Cкачиваем и распаковываем консольное приложение:
```
wget https://dl.bintray.com/viclovsky/maven/com/github/viclovsky/swagger/coverage/swagger-coverage-commandline/{latest-swagger-coverage-version}/swagger-coverage-commandline-{latest-swagger-coverage-version}.zip
unzip swagger-coverage-commandline-{latest-swagger-coverage-version}.zip
```
Далее его можно запускать, например так:
```
./swagger-coverage-commandline -s swagger.json -i swagger-coverage-output --ignoreHeaders X-Request-Id,X-Some-Header --ignoreNotOkStatusCodes
```




