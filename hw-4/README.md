# Домашнее задание к занятию «Базы данных»

### Легенда

Заказчик передал вам [файл в формате Excel](https://github.com/netology-code/sdb-homeworks/blob/main/resources/hw-12-1.xlsx), в котором сформирован отчёт. 

На основе этого отчёта нужно выполнить следующие задания.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных. Определите:

- какие данные хранятся в этих таблицах,
- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.

Начертите схему полученной модели данных. Можете использовать онлайн-редактор: https://app.diagrams.net/

Этапы реализации:
1.	Внимательно изучите предоставленный вам файл с данными и подумайте, как можно сгруппировать данные по смыслу.
2.	Разбейте исходный файл на несколько таблиц и определите список столбцов в каждой из них. 
3.	Для каждого столбца подберите подходящий тип данных из PostgreSQL. 
4.	Для каждой таблицы определите первичный ключ (PRIMARY KEY).
5.	Определите типы связей между таблицами. 
6.	Начертите схему модели данных.
На схеме должны быть чётко отображены:
   - все таблицы с их названиями,
   - все столбцы  с указанием типов данных,
   - первичные ключи (они должны быть явно выделены),
   - линии, показывающие связи между таблицами.

**Результатом выполнения задания** должен стать скриншот получившейся схемы базы данных.

### Решение 1

[Схема БД Postgres](https://drawdb.vercel.app/editor?shareId=ae7a3b791928fd04916b3399870a215e)
![Картинка](./img/task1.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению. Вы можете их выполнить, если хотите глубже и шире разобраться в материале.


### Задание 2*

1. Разверните СУБД Postgres на своей хостовой машине, на виртуальной машине или в контейнере docker.
2. Опишите схему, полученную в предыдущем задании, с помощью скрипта SQL.
3. Создайте в вашей полученной СУБД новую базу данных и выполните полученный ранее скрипт для создания вашей модели данных.

В качестве решения приложите SQL скрипт и скриншот диаграммы.

Для написания и редактирования sql удобно использовать  специальный инструмент dbeaver.

### Решение 2*
#### Скрипт
```SQL
CREATE TABLE IF NOT EXISTS "Адреса" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"address" VARCHAR(255)
);

CREATE TABLE IF NOT EXISTS "Должность" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"position" VARCHAR(255)
);

CREATE TABLE IF NOT EXISTS "Тип подразделения" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"name" VARCHAR(255)
);

CREATE TABLE IF NOT EXISTS "Проекты" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"name" VARCHAR(255)
);

CREATE TABLE IF NOT EXISTS "Подразделение" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"name" VARCHAR(255),
	"adress_id" INTEGER,
	"type_id" INTEGER,
	foreign key ("adress_id") references "Адреса"("id"),
	foreign key ("type_id") references "Тип подразделения"("id")
);

CREATE TABLE IF NOT EXISTS "Сотрудник" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"name" VARCHAR(255),
	"hired" DATE,
	"salary" INTEGER,
	"division_id" INTEGER,
	"position_id" INTEGER,
	foreign key ("division_id") references "Подразделение"("id"),
	foreign key ("position_id") references "Должность"("id")
);

CREATE TABLE IF NOT EXISTS "Проект-сотрудник" (
	"id" INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
	"project_id" INTEGER,
	"employee_id" INTEGER,
	foreign key ("project_id") references "Проекты"("id"),
	foreign key ("employee_id") references "Сотрудник"("id")
);
```
![Диаграмма](./img/task2.png)