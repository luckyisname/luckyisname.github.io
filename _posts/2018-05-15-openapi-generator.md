---
title: Релиз OpenAPI Generator
date: 2018-05-15
categories:
- Автотесты на API
tags:
- openapi-generator
- swagger
---
12 мая 2018 года состоялся релиз [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator). Проект является форком swagger-codegen, полностью поддерживаемый и развиваемый community. В нем уже добавлена поддержка OpenAPI-spec 3.0 и проект продолжает развиваться. **Я являюсь одним из основателем этого проекта.**

>OpenAPI Generator is a fork of Swagger Codegen. In view of the issues with the Swagger Codegen 3.0.0 (beta) release and the disagreement on the project's direction, more than 40 top contributors and template creators of Swagger Codegen decided to fork Swagger Codegen and maintain a community-driven version called "OpenAPI Generator". Please refer to the Q&A for more information.

## Основные причины
Их можно найти [тут](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/qna.md).
Если быть кратким, то основные проблемы [Swagger Codegen 3.0.0 (beta)](https://github.com/swagger-api/swagger-codegen/releases/tag/v3.0.0-rc0).

1. Отсутствие обратной совместимости
2. Нестабильность. Наличие регресии, отсутствие тестов.
3. Недостаточно документации
4. Независимое развитие Swagger Codegen 3.x от Swagger Codegen 2.x

В следствии всех этих проблем, community решило продолжить работу над версией 2.4.0-SNAPSHOT в независимом проекте.

## Переход на OpenAPI Generator
Инструкция по переходу со Swagger Codegen на OpenAPI Generator [тут](https://github.com/OpenAPITools/openapi-generator/blob/master/docs/migration-from-swagger-codegen.md).

>  Having a community-driven version can bring the project to the next level.


 
 

