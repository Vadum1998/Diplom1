# Дипломный проект "Автоматизации тестирования комплексного сервиса покупки тура "Путешествие дня"

## Описание приложения

### Бизнес часть

Приложение представляет из себя веб-сервис.

![](pic/service.png)

Приложение предлагает купить тур по определённой цене с помощью двух способов:
1. Оплата по дебетовой карте
1. Покупка в кредит по данным банковской карты

Само приложение не обрабатывает данные по картам, а пересылает их банковским сервисам:
* сервису платежей Payment Gate
* сервису покупки в кредит Credit Gate


### Техническая часть

Приложение [`aqa-shop.jar`](aqa-shop.jar) запускается командой `java -jar aqa-shop.jar` на порту 8080.

Типовые настройки (url-адреса банковских сервисов, учётные данные и url для подключения к СУБД) находятся в файле [`application.properties`](application.properties)

### СУБД

* MySQL
* PostgreSQL

## Задача

Автоматизация позитивных и негативных сценариев покупки тура.

## Документация

[Дипломное задание](https://github.com/netology-code/qa-diploma.git)

[План автоматизации тестирования веб-формы сервиса покупки туров интернет-банка](docs/Plan.md)

[Отчёт о проведенном тестировании](docs/Report.md)

[Отчёт о проведённой автоматизации](docs/Summary.md)

## Запуск приложения

## Предварительно установленные компоненты:
* IntelliJ IDEA Community Edition 2021.3.2
* Docker Desktop 

## Загруженные docker container images:
* mysql:latest
* postgres:latest
* gate-simulator:1.0
#### Команды для скачивания образов:
##### Скачать MySql image:
    docker pull mysql:latest
    
##### Скачать PostgreSql image:
    docker pull postgres:latest

### Подключение к MySQL

1. Запустить Docker Desktop
1. Открыть проект в IntelliJ IDEA
1. В терминале в корне проекта запустить контейнеры:

   `docker-compose up -d`
1. Запустить приложение:

   `java "-Dspring.datasource.url=jdbc:mysql://localhost:3306/app" -jar artifacts/aqa-shop.jar`
1. Открыть второй терминал
1. Запустить тесты:

   `.\gradlew clean test "-Ddb.url=jdbc:mysql://localhost:3306/app"`
1. Создать отчёт Allure и открыть в браузере

   `.\gradlew allureServe`
1. Закрыть отчёт:

   **CTRL + C -> y -> Enter**
1. Перейти в первый терминал
1. Остановить приложение:

   **CTRL + C**
1. Остановить контейнеры:

   `docker-compose down`
   </a>
   
### Подключение к PostgreSQL

1. Запустить Docker Desktop
1. Открыть проект в IntelliJ IDEA
1. В терминале в корне проекта запустить контейнеры:

   `docker-compose up -d`
1. Запустить приложение:

   `java "-Dspring.datasource.url=jdbc:postgresql://localhost:5432/app" -jar artifacts/aqa-shop.jar`
1. Открыть второй терминал
1. Запустить тесты:

   `.\gradlew clean test "-Ddb.url=jdbc:postgresql://localhost:5432/app"`
1. Создать отчёт Allure и открыть в браузере

   `.\gradlew allureServe`
1. Закрыть отчёт:

   **CTRL + C -> y -> Enter**
1. Перейти в первый терминал
1. Остановить приложение:

   **CTRL + C**
1. Остановить контейнеры:

   `docker-compose down`
   </a>