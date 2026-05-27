# `MARKDOWN2.md`

````md
# ЕТАП 2 — Python ↔ SQL + CRUD „Клубове“ + Chatbot основа

## Цел на проекта

Да се реализира:
- връзка между Python и SQL база данни
- CRUD операции за клубове
- базов чатбот с текстови команди
- първа версия на проекта в GitHub

---

# Структура на проекта

```text
project/
│
├── src/
│   ├── main.py
│   ├── db.py
│   ├── chatbot.py
│   ├── intents.json
│   └── clubs_service.py
│
├── sql/
│   └── schema.sql
│
├── docs/
│
├── README.md
└── .gitignore
````

---

# 1. DB модул (`db.py`)

## Минимални изисквания

* функция за връзка към база данни
* изпълнение на SQL заявки

## Задължително

* централизирана връзка към БД
* обработка на грешки чрез `try/except`

---

# 2. CRUD модул (`clubs_service.py`)

## Задължителни функции

```python
add_club(name)
get_all_clubs()
delete_club(name_or_id)
```

## Препоръчително

```python
update_club(...)
```

## Валидация

* името да не е празно
* да няма дублирани клубове

---

# 3. Chatbot — първа версия

## Основна логика

```text
input → intent → handler → response
```

---

# intents.json

## Минимални intents

* help
* exit
* add_club
* list_clubs
* delete_club

---

# Примерни команди

```text
Добави клуб Левски София
Покажи всички клубове
Изтрий клуб Ботев
помощ
изход
```

---

# Разпознаване на команди

Използва се:

* regex правила
* извличане на параметри

---

# 4. main.py — Chat Loop

## Задължително

* while loop
* четене на input
* извикване на chatbot parser
* извикване на clubs_service
* показване на резултат

---

# 5. Logging (`commands.log`)

При всяка команда се записва:

* timestamp
* въведен текст
* резултат

---

# 6. GitHub изисквания

## Репозитория

Проектът трябва да бъде качен в GitHub.

---

# README.md трябва да съдържа

* описание на проекта
* как се стартира
* примерни команди

---

# Минимални commit-и

```text
initial commit
db module
chatbot base
clubs CRUD
```

---

# 7. Демо (какво трябва да работи)

Учениците трябва да покажат:

* стартиране на чатбот
* добавяне на клуб чрез команда
* показване на списък
* изтриване на клуб
* помощ команда

---

# Минимален резултат (Pass condition)

## Проектът трябва да има:

* работеща база данни
* работещ CRUD
* чатбот, който разбира поне 3 команди
* качен проект в GitHub

````

---

# `PROJECT_STRUCTURE.md`

```md
# Структура на проекта

```text
project/
│
├── src/
│   ├── main.py
│   ├── db.py
│   ├── chatbot.py
│   ├── intents.json
│   └── clubs_service.py
│
├── sql/
│   └── schema.sql
│
├── docs/
│
├── README.md
└── .gitignore
````

---

# Описание на файловете

## src/main.py

Главен файл на приложението.
Стартира chatbot цикъла.

## src/db.py

Модул за връзка с базата данни.

## src/chatbot.py

Разпознаване на intents и обработка на команди.

## src/intents.json

Описание на поддържаните команди.

## src/clubs_service.py

CRUD операции за клубове.

## sql/schema.sql

SQL структура на базата данни.

````

---

# `CHATBOT_REQUIREMENTS.md`

```md
# Chatbot — Изисквания

## Основна логика

```text
input → intent → handler → response
````

---

# Поддържани intents

| Intent      | Описание             |
| ----------- | -------------------- |
| help        | Показва помощ        |
| exit        | Изход от програмата  |
| add_club    | Добавяне на клуб     |
| list_clubs  | Показване на клубове |
| delete_club | Изтриване на клуб    |

---

# Примерни команди

```text
Добави клуб Левски София
Покажи всички клубове
Изтрий клуб Ботев
помощ
изход
```

---

# Разпознаване

Използва се:

* regex
* текстови правила
* извличане на параметри

---

# Примерен flow

```text
User Input
   ↓
Intent Detection
   ↓
Handler Function
   ↓
Database Operation
   ↓
Response
```

````

---

# `GITHUB_REQUIREMENTS.md`

```md
# GitHub Изисквания

## Репозитория

Проектът трябва да бъде качен в GitHub.

---

# README.md

Трябва да съдържа:
- описание на проекта
- инструкции за стартиране
- примерни команди

---

# Минимални commit-и

1. initial commit
2. db module
3. chatbot base
4. clubs CRUD

---

# Демо

Учениците трябва да покажат:

- стартиране на chatbot
- добавяне на клуб
- показване на клубове
- изтриване на клуб
- команда help
````
