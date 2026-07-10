# django-app-ci

Базовий Django-застосунок.

## Запуск локально

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

cp .env.example .env

python manage.py migrate
python manage.py runserver
```

Застосунок буде доступний на http://127.0.0.1:8000/

## Структура

- `config/` — налаштування та кореневі URL-маршрути проєкту
- `core/` — базовий застосунок з домашньою сторінкою
