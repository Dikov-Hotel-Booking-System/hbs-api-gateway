# 🚀 Hbs-api-gateway

> Внешняя точка входа в Hotel Booking System

---

## 📌 Описание

API Gateway направляет `GET /api/hotels` в `hbs-hotel-service`. Eureka помогает найти экземпляр сервиса на его текущем порту.

---

## 📘 Документация по процессам

- ### [GitHub Flow](docs/github_flow.md)

---

## 🛠️ Стек технологий

- ☕ Java 21 (язык разработки)
- 🌱 Spring Boot 4.1.2 (основа приложения)
- 🚪 Spring Cloud Gateway Server WebFlux (основа API Gateway)
- 🔎 Spring Cloud 2025.1.4 и Eureka Discovery Client (регистрация сервисов)
- 🐘 Gradle 9.5.1 с Kotlin DSL (сборка)
- 🐳 Docker и GitHub Actions (сборка образа и CI)

---

## 🚀 Локальный запуск

1. Убедиться, что установлена JDK 21, и запустить [hbs-eureka-server](https://github.com/Dikov-Hotel-Booking-System/hbs-eureka-server)

   Адрес Eureka по умолчанию: `http://localhost:8761/eureka/`.

2. Создать папку `config` в корне проекта и файл `config/application.yaml` с содержимым ниже
   ```yaml
   server:
     port: 8080

   eureka:
     client:
       service-url:
         defaultZone: "http://localhost:8761/eureka/"
   ```

   Локальный файл переопределяет значения из `src/main/resources/application.yaml`
   через `spring.config.import: optional:file:./config/application.yaml`.
   Папка `config` исключена из Git.

   Если значения по умолчанию подходят, локальный файл можно не создавать.

3. Из корня проекта выполнить команду
   ```shell
   .\gradlew.bat bootRun
   ```

   Порт приложения по умолчанию: `8080`. Его можно изменить в локальном файле настроек.

---

## 📦 Сборка JAR и Docker-образа

```shell
.\gradlew.bat bootJar
docker build --tag hbs-api-gateway:local .
```

Исполняемый JAR находится в `build/libs/app.jar`. Dockerfile использует JRE 21
и запускает приложение от пользователя `10001:10001`.

---

## 🔗 Полезные ссылки

| Ссылка | Примечание |
|--------|------------|
| [Репозиторий](https://github.com/Dikov-Hotel-Booking-System/hbs-api-gateway) | Исходный код сервиса |
| [GitHub Actions](https://github.com/Dikov-Hotel-Booking-System/hbs-api-gateway/actions) | Запуски CI |

---

## 🧪 Тестирование сервиса

Тестовых зависимостей и тестовых классов в сервисе нет.
В общем CI задача `test` завершается с `NO-SOURCE`.

---
