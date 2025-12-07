# Проект: API для социальной сети Yatube

## Описание

Данный проект представляет собой API для социальной сети Yatube, позволяющее пользователям взаимодействовать с контентом (публикациями, комментариями) и управлять подписками на других пользователей. Проект реализует RESTful API с использованием Django REST Framework и JWT-аутентификации.

## Установка

Для развертывания проекта на локальной машине выполните следующие шаги:

1. Клонируйте репозиторий:

   ```bash
   git clone <URL вашего репозитория>
   cd api_final_yatube
   ```

2. Создайте и активируйте виртуальное окружение:

   ```bash
   python -m venv venv
   # Для Windows:
   venv\Scripts\activate
   # Для Linux/macOS:
   # source venv/bin/activate
   ```

3. Установите зависимости:

   ```bash
   pip install -r requirements.txt
   ```

4. Перейдите в директорию с `manage.py`:

   ```bash
   cd yatube_api
   ```

5. Выполните миграции базы данных:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. Создайте суперпользователя (для доступа к Django Admin):

   ```bash
   python manage.py createsuperuser
   ```

7. Запустите сервер разработки:

   ```bash
   python manage.py runserver
   ```

Проект будет доступен по адресу `http://127.0.0.1:8000/`.

## Примеры запросов к API

### Получение JWT-токена

Для доступа к защищенным эндпоинтам необходимо получить JWT-токен. Отправьте POST-запрос на `/api/v1/jwt/create/` с вашими учетными данными:

**POST** `/api/v1/jwt/create/`

**Тело запроса**:

```json
{
    "username": "your_username",
    "password": "your_password"
}
```

**Пример ответа**:

```json
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

Используйте `access` токен в заголовке `Authorization: Bearer <access_token>` для последующих запросов.

### Обновление JWT-токена

**POST** `/api/v1/jwt/refresh/`

**Тело запроса**:

```json
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

### Получение списка подписок текущего пользователя

**GET** `/api/v1/follow/`

**Пример ответа**:

```json
[
    {
        "id": 1,
        "user": "current_user",
        "following": "followed_user_1"
    },
    {
        "id": 2,
        "user": "current_user",
        "following": "followed_user_2"
    }
]
```

### Подписка на пользователя

**POST** `/api/v1/follow/`

**Тело запроса**:

```json
{
    "following": "username_to_follow"
}
```

**Пример ответа**:

```json
{
    "id": 3,
    "user": "current_user",
    "following": "username_to_follow"
}
```

**Примечание**: При попытке подписаться на самого себя будет возвращено сообщение об ошибке (HTTP 400 Bad Request).

### Получение списка постов

**GET** `/api/v1/posts/`

### Создание нового поста

**POST** `/api/v1/posts/`

**Тело запроса**:

```json
{
    "text": "Мой новый пост!",
    "group": null, 
    "image": null
}
```

### Получение комментариев к посту

**GET** `/api/v1/posts/{post_id}/comments/`

### Добавление комментария к посту

**POST** `/api/v1/posts/{post_id}/comments/`

**Тело запроса**:

```json
{
    "text": "Отличный пост!"
}
```
