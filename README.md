# hbs-api-gateway

Внешняя точка входа Hotel Booking System. На этом этапе создан каркас;
маршруты к API будут настроены при реализации сервисов.

Проект сгенерирован через [Spring Initializr](https://start.spring.io): Java 21,
Spring Boot 4.1.2, Spring Cloud 2025.1.4, Gradle Kotlin DSL. Group: `ru.mephi`,
пакет: `ru.mephi.hbs.gateway`.

Зависимости: Spring Cloud Gateway Server WebFlux и Eureka Discovery Client.
Тестовые зависимости и шаблонный `contextLoads` удалены: собственных маршрутов
и фильтров пока нет. Когда они появятся, полезны тесты выбора сервиса,
преобразования пути и заголовков, а также поведения фильтров.

```powershell
.\gradlew.bat build
.\gradlew.bat bootRun
```

Порт приложения: `8080`. Для регистрации сначала запустите `hbs-eureka-server`;
Eureka по умолчанию ожидается по адресу `http://localhost:8761/eureka/`.

## Docker

Исполняемый JAR всегда называется `build/libs/app.jar`; обычный библиотечный
JAR не собирается. Сначала соберите JAR, затем образ:

```powershell
.\gradlew.bat bootJar
docker build --tag hbs-api-gateway:local .
```

Dockerfile использует JRE 21 и запускает приложение от UID/GID `10001:10001`.
В образ копируется уже собранный JAR.

## GitHub Actions

Workflow [`.github/workflows/ci.yml`](.github/workflows/ci.yml) запускается
при push в `master` и `feature/**`, для PR в `master`
и вручную через `workflow_dispatch`. Содержимое workflow одинаково
во всех четырёх репозиториях.

Рабочий поток: `feature/<name>` → PR → `master`.
Проверка `Branch policy` отклоняет PR из других исходных веток.
Правила защиты для GitHub и порядок первоначальной настройки описаны
в [GitHub Flow](docs/github_flow/index.md).

1. `build` выполняет `test bootJar`: собирает JAR; задача тестирования получает `NO-SOURCE`.
2. `docker` получает тот же JAR через artifact и собирает образ с тегом SHA коммита.

Готовый JAR сохраняется в artifacts на 7 дней. Образ проверяется сборкой,
без публикации в registry. Деплоя и Kubernetes-манифестов нет.
