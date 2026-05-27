````markdown id="t9x4mv"
# ✅ Етап 4 — Трансфери (изисквания)

## 🎯 Цел

История на трансфери + коректна бизнес логика, управлявана и от чатбот команди, по модулна архитектура:

```text
NLU → Router → Services → DB
````

---

# 1) База данни (SQL) — задължително

## 1.1 Таблици / полета

### `transfers` (задължително)

| Поле            | Описание                                           |
| --------------- | -------------------------------------------------- |
| `id`            | PK                                                 |
| `player_id`     | FK → `players.id`                                  |
| `from_club_id`  | FK → `clubs.id`, допуска `NULL` ако е „първи клуб“ |
| `to_club_id`    | FK → `clubs.id`                                    |
| `transfer_date` | `DATE` или `TEXT` за SQLite                        |
| `fee`           | *(по избор)* число                                 |
| `note`          | *(по избор)* текст                                 |

---

### `players` (задължително да поддържа текущ клуб)

| Поле      | Описание                     |
| --------- | ---------------------------- |
| `club_id` | FK → `clubs.id`, може `NULL` |

---

## 1.2 Ограничения (минимум)

* `to_club_id ≠ from_club_id`
* `player_id` задължително
* `transfer_date` валиден формат *(поне `YYYY-MM-DD`)*

---

# 2) Архитектура — задължително

Добавяш нов модул:

```text
src/
├── chatbot/
│   ├── intents.json
│   ├── nlu.py
│   └── router.py
├── services/
│   ├── players_service.py
│   └── transfers_service.py   ← ново
├── database/
│   └── db.py
└── utils/
    └── logger.py
```

---

# 3) Transfers Service (`services/transfers_service.py`)

## 3.1 Задължителни функции

```python
transfer_player(player_name, from_club, to_club, date, fee=None)
list_transfers_by_player(player_name)
list_transfers_by_club(club_name)
```

> `list_transfers_by_club(club_name)` е по избор, но препоръчително.

---

## 3.2 Бизнес логика (строго задължителна)

При команда „Трансфер …“ системата трябва:

1. Да намери играча по име *(или да върне грешка)*
2. Да намери клубовете `from` и `to` по име *(или грешка)*
3. Да провери правилото за текущ клуб:

   * ако `players.club_id ≠ from_club_id` → отказ с ясно съобщение
   * ако играчът е без клуб (`NULL`):

     * допуска се трансфер само ако `from_club` е „няма/свободен“
4. Да създаде запис в `transfers`
5. Да обнови `players.club_id = to_club_id`
6. Да върне потвърждение с резюме:

   * играч
   * от
   * към
   * дата

---

## 3.3 Транзакционност (ULTRA)

Операцията трябва да е **атомична**:

```text
или се записват и двете неща:
(transfers + update player)

или нищо:
(rollback / без commit при грешка)
```

### SQLite правило:

`commit` само след като и двете заявки минат успешно.

---

# 4) Chatbot / NLU (интент + параметри)

## 4.1 `intents.json` — добавяш intents

### Задължителни нови intent-и

* `transfer_player`
* `show_transfers_player`
* `show_transfers_club` *(препоръчително)*

---

## 4.2 Команди (минимум)

```text
Трансфер Иван Петров от Левски в Лудогорец 2026-03-10
```

```text
Трансфер Иван Петров от Левски в Лудогорец 2026-03-10 сума 50000
```

*(по избор)*

```text
Покажи трансфери на Иван Петров
```

```text
Покажи трансфери на Левски
```

*(по избор)*

---

## 4.3 Валидации на входа

* дата да е `YYYY-MM-DD`
* клубовете да съществуват
* играчът да съществува
* `from != to`
* сума *(ако има)* да е число `≥ 0`

---

# 5) Router (`chatbot/router.py`)

Router-ът трябва:

* да извика `transfers_service.transfer_player(...)`
* да върне отговори без SQL вътре
* без DB логика в `router`

---

# 6) Logging (`utils/logger.py`)

В `commands.log` да се записва:

* timestamp
* raw input
* intent
* extracted params *(съкратено)*
* резултат:

  * `OK`
  * `ERROR + message`

---

# 7) Тестови данни и сценарии (задължително)

## Минимум тестови данни

* поне 4 клуба
* поне 6 играчи *(разпределени по клубове)*
* поне 5 трансфера

---

## Минимум тестови сценарии

Да бъдат описани в `docs/` или текстов файл.

### Сценарии:

* валиден трансфер *(успех)*
* трансфер с грешен „отбор от“ *(отказ)*
* трансфер към несъществуващ клуб *(отказ)*
* трансфер със същия `from/to` *(отказ)*
* показване история на играч *(успех)*

---

# 8) GitHub изисквания за Етап 4

## Commit-и минимум

```text
feat: transfers table + schema update
```

```text
feat: transfers service + business rules
```

```text
feat: chatbot intent transfer + parsing
```

```text
test: seed data + scenarios
```

```
```
