# [ py ] Django Framework: Шпаргалка по быстрому старту

Замечание. Предполагается, что у Вас уже установлен python.

## 1. Инициализацию проекта фреймворка Django начнем с создания папки проекта и виртуального окружения:

> $ mkdir app && cd app
> $ python3 -m venv env
> $ touch .env .gitignore 
> $
> $ cat > .gitignore
> __pycache__/
> *.py[cod]
> /env
> *.log
> .env
> .idea
> .DS_Store 

* создали папку проекта, затем перешли в нее
* создали виртуальное окружение в папке env
* создали файл .env для хранения переменных окружения и файл .gitignore для описания исключяющихся файлов и папок из git репозитория, добавили исключения.

## 2 . Создаем проект django

> $ source env/bin/activate
> (env)$ pip install django==3.2.6
> (env)$ django-admin.py startproject django_project_name .
> (env)$ python manage.py migrate
> (env)$ python manage.py runserver

прим. для выхода из виртуального окружения в терминале пишем deactivate.

## 3. Проверяем работоспасобность Django framework

Командой $ python manage.py runserver мы запустили тестовую страничку Django фреймворка. Далее переходим в браузер http://127.0.0.1:8000/ или на http://localhost:8000/. Если видем страницу ниже, то все ок и можно продолжить работу дальше.



## 4. Памятка по необходимым командам
> ### Cоздание Django приложения с именем app_name
> $ python manage.py startapp app_name
>
> ### Синхронизация с текущей моделью данных
> $ python manage.py makemigrations app_name
>
> ### Просмотр запросов на перемещение(миграцию) модели данных в БД
> $ python manage.py sqlmigrate app_name 0001
>
> ### Cинхронизация базы данных с новой моделью
> $ python manage.py migrate
>
> прим. Если вы изменяете model.py файл (добавление, удаление, изменение существующих полей, 
> или добавление новой модели), вы должны создать новую миграцию модели использовав команду
> makemigrations. Миграция позволяет Django сохранить путь изменения модели. Тогда вы сможете
> применить изменения с помощью команды migrate
> 
> ### Создание супер-пользователя для администрирования сайта
> $ python manage.py createsuperuser
>
> ### Сборка статики на продакшене  
> $ python manage.py collectstatic
