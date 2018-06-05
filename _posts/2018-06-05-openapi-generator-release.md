---
title: Первый публичный релиз OpenAPI Generator
date: 2018-06-05
categories:
- Автотесты на API
tags:
- openapi-generator
---
2 июня состоялся первый публичный релиз openapi-generator. Актуальная версия 3.0.0. Все изменения можно найти [тут](https://github.com/OpenAPITools/openapi-generator/releases). **Этот релиз включает мой Rest-assured клиент.**

## Rest-assured клиент
Посмотреть можно так:
```
docker run --rm -v ${PWD}:/local openapitools/openapi-generator-cli generate -i http://petstore.swagger.io/v2/swagger.json -g java --library rest-assured -o /local/out/java 
``` 

Также есть maven плагин для генерации клиента
```xml
 <plugin>
      <groupId>org.openapitools</groupId>
      <artifactId>openapi-generator-maven-plugin</artifactId>
      <version>3.0.0</version>
      <executions>
          <execution>
              <goals>
                  <goal>generate</goal>
              </goals>
              <configuration>
                  <inputSpec>http://petstore.swagger.io/v2/swagger.json</inputSpec>
                  <output>${project.build.directory}/generated-sources/swagger</output>
                  <language>java</language>
                  <configOptions>
                      <dateLibrary>java8</dateLibrary>
                  </configOptions>
                  <library>rest-assured</library>
                  <generateApiTests>true</generateApiTests>
                  <generateApiDocumentation>false</generateApiDocumentation>
                  <generateModelDocumentation>false</generateModelDocumentation>
                  <apiPackage>${default.package}.api</apiPackage>
                  <modelPackage>${default.package}.model</modelPackage>
                  <invokerPackage>${default.package}</invokerPackage>
              </configuration>
          </execution>
      </executions>
  </plugin>
```

Более подробно я писал в своем [посте](https://viclovsky.github.io/%D0%B0%D0%B2%D1%82%D0%BE%D1%82%D0%B5%D1%81%D1%82%D1%8B%20%D0%BD%D0%B0%20api/2018/05/09/swagger-rest-assured/).

 
 

