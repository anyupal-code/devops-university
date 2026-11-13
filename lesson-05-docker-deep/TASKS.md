# Задания к Уроку 5

## Задание 1. Многоэтапная сборка
1. Откройте ваш Java-проект (Spring Boot) и найдите `Dockerfile`.
2. Замените его содержимое на многоэтапный вариант:

~~~dockerfile
# Этап 1: Сборка
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Этап 2: Запуск
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/target/backend-app-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
~~~

3. Соберите образ: `docker build -t backend-app-optimized .`
4. Посмотрите на размер: `docker images`.
5. Сравните с размером старого образа (если он у вас есть). Разница должна быть ощутимой (в 2–3 раза меньше).

## Задание 2. Использование переменных окружения
1. Создайте файл `.env` в папке проекта:
   ~~~env
   DB_PASSWORD=mypassword
   ~~~
2. В `docker-compose.yml` замените пароль на переменную:
   ~~~yaml
   environment:
     POSTGRES_PASSWORD: ${DB_PASSWORD}
   ~~~
3. Запустите `docker-compose up`. Убедитесь, что база данных запустилась с паролем из `.env`.

## Задание 3 (*). Проверка сохранности данных (Volumes)
1. Запустите базу данных с томом (как в `docker-compose.yml` из прошлого урока).
2. Создайте таблицу и добавьте в неё запись.
3. Остановите и удалите контейнер: `docker-compose down` (без флага `-v`).
4. Снова запустите `docker-compose up`.
5. Проверьте, что данные на месте (том сохранил их).

**Ожидаемый результат:** данные не исчезли, потому что они хранятся в томе `postgres_data`, а не внутри контейнера.