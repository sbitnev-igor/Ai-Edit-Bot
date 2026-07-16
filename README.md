# AI Edit Bot

Telegram-бот на базе [aiogram 3](https://docs.aiogram.dev/), использующий SQLAlchemy (async) для работы с базой данных.

## Стек технологий

- **Python 3** + **asyncio**
- **aiogram 3.24** — фреймворк для Telegram Bot API
- **SQLAlchemy 2.0** + **aiosqlite** — асинхронная работа с БД (SQLite)
- **environs / python-dotenv** — загрузка конфигурации из `.env`
- **pydantic** — валидация данных
- **openpyxl** — работа с Excel-файлами

## Структура проекта

```
.
├── __main__.py              # точка входа
├── __init__.py
├── config/
│   └── config.py            # загрузка конфигурации (Config, load_config)
├── log_setup/
│   └── logging.py           # настройка логирования (setup_logging)
├── db/
│   └── database.py          # инициализация БД (init_db)
├── keyboards/
│   └── menu_commands.py     # меню команд бота (set_main_menu)
├── handlers/
│   ├── user.py               # хендлеры для пользователей (router)
│   └── admin.py               # хендлеры для админов (router)
├── Procfile                 # команда запуска для деплоя (Heroku и т.п.)
├── requirements.txt          # зависимости проекта
└── TODO.md                  # список задач
```

> Примечание: часть модулей (`config`, `log_setup`, `db`, `keyboards`, `handlers`) импортируется в `__main__.py`, но сами файлы не были загружены в этот чат — добавьте их в проект перед запуском, если их ещё нет.

## Установка

1. Клонируйте репозиторий и перейдите в его директорию.
2. Создайте и активируйте виртуальное окружение:

   ```bash
   python -m venv venv
   source venv/bin/activate   # Linux/macOS
   venv\Scripts\activate      # Windows
   ```

3. Установите зависимости:

   ```bash
   pip install -r requirements.txt
   ```

4. Создайте файл `.env` в корне проекта и укажите необходимые переменные окружения, например:

   ```env
   BOT_TOKEN=your_telegram_bot_token
   ADMIN_IDS=123456789,987654321
   CHANNEL_ID=-1001234567890
   CHANNEL_URL=https://t.me/your_channel
   ```

   Конкретный набор переменных зависит от реализации `config/config.py`.

## Запуск

Локально:

```bash
python bot
```

(команда соответствует содержимому `Procfile`: `worker: python bot`)

Через модуль:

```bash
python -m .
```

или, если пакет называется иначе, соответствующей командой запуска пакета, содержащего `__main__.py`.

## Деплой

Проект содержит `Procfile`, что говорит о готовности к деплою на платформы вроде **Heroku**:

```
worker: python bot
```

Это означает, что бот работает как **worker**-процесс (long polling), а не как web-сервис.

## Функциональность

При запуске бот:

1. Настраивает логирование.
2. Загружает конфигурацию из `.env`.
3. Создаёт экземпляр `Bot` и `Dispatcher`.
4. Инициализирует базу данных.
5. Устанавливает главное меню команд.
6. Подключает роутеры для админских и пользовательских хендлеров.
7. Запускает polling, передавая в контекст `admin_ids`, `CHANNEL_ID` и `CHANNEL_URL`.

## Лицензия

Не указана.
