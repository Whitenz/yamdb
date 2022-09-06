![Django-app workflow](https://github.com/Whitenz/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)


# YaMDb - база отзывов пользователей о фильмах, книгах и музыке


## Особенности
Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий может быть расширен администратором. Произведению может быть присвоен жанр из списка предустановленных. Новые жанры может создавать только администратор. Пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

#### Ресурсы API YaMDb:
- auth: аутентификация
- users: пользователи
- titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка)
- categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»)
- genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам
- reviews: отзывы на произведения. Отзыв привязан к определённому произведению
- comments: комментарии к отзывам. Комментарий привязан к определённому отзыву

Каждый ресурс описан в <a href="http://yourip/redoc/">redoc</a> файле, доступный после запуска приложения: указаны эндпоинты (адреса, по которым можно сделать запрос), разрешённые типы запросов, права доступа и дополнительные параметры, если это необходимо.


## Как запустить приложение

#### Запустить командную строку:
<details>
    <summary>Windows</summary>
    Скачайте и установите Git Bash. Подробная инструкция и дистрибутив можно найти на <a href="https://gitforwindows.org/">сайте</a>. Затем в меню пуск найдите и запустите приложение Git Bash.
</details>
<details>
    <summary>macOS или Linux</summary>
    Откройте главное меню приложений и выберите приложение "Терминал".
</details>

#### Установить Docker:

Следуйте официальной [документации](https://docs.docker.com/get-docker/) Docker для Вашей ОС.


#### Клонировать репозиторий и перейти в каталог с файлом docker-compose.yaml:
```
git clone https://github.com/Whitenz/yamdb_final
cd yamdb_final/infra/
```

#### Создать файл .env с переменными окружения:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres # задайте свой пароль
DB_HOST=db
DB_PORT=5432
```

#### Создать образы и запустить контейнеры:
```
sudo docker-compose up -d
```

#### Выполнить миграции:
```
sudo docker-compose exec web python manage.py migrate
```

#### Собрать статику:
```
sudo docker-compose exec web python manage.py collectstatic --no-input
```

#### Создать суперпользователя:
```
sudo docker-compose exec web python manage.py createsuperuser

```

#### Заполнить базу данных тестовыми данными:
```
sudo docker-compose exec web python manage.py loaddata fixtures.json

```

✨ Поздравляю ✨ <br>
Приложение запущено и готово к работе.

## Эндпоинты API
- Регистрация пользователей и выдача токенов
```
/api/v1/auth/signup/ - POST
/api/v1/auth/token/ - POST
```
- Категории произведений
```
/api/v1/categories/- GET, POST
/api/v1/categories/{slug}/ - DEL
```
- Категории жанров
```
/api/v1/genres/ - GET, POST
/api/v1/genres/{slug}/ - DELETE
```
- Произведения, к которым пишут отзывы
```
/api/v1/titles/ - GET, POST
/api/v1/titles/{titles_id}/ - GET, PATCH, DELETE
```
- Отзывы к произведениям
```
/api/v1/titles/{title_id}/reviews/ - GET, POST
/api/v1/titles/{title_id}/reviews/{review_id}/ - GET, PATCH, DELETE
```
- Комментарии к отзывам
```
/api/v1/titles/{title_id}/reviews/{review_id}/comments/ - GET, POST
/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/ - GET, PATCH, DELETE
```
- Пользователи
```
/api/v1/users/ - GET, POST
/api/v1/users/{username}/- GET, PATCH, DELETE
/api/v1/users/me/ - GET, PATCH
```

Доступ для чтения возможен с моделью Post, Group, Comment. В ином случае доступ через JSON Web Token.

### Пример запроса к API

Список всех отзывов
```
curl -X GET 'http://localhost/api/v1/categories/'
```
Ответ
```
{
    "count": 3,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "Книга",
            "slug": "book"
        },
        {
            "name": "Музыка",
            "slug": "music"
        },
        {
            "name": "Фильм",
            "slug": "movie"
        }
    ]
}
```

## Стек технологий
- Django
- PyJWT
- djoser
- postgresql
- nginx
- gunicorn
- docker


## Лицензия
BSD 3-Clause License


## Автор
Ilya Kolesnikov