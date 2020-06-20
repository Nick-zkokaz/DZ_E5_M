# Задание E5_10
Для работы с клоном 
1) Распакуйте проект в папку C:\DZ_E5_M
2) Откройте командную строку и зайдите в директорию проекта:
   - cd C:\DZ_E5_M
3) Создайте виртуальное окружение:
   - python -m venv django
4) Активируйте виртуальное окружение:
   - django\scripts\activate.bat
5) Установите все необходимые пакеты:
   - pip install -r requirements.txt
6) Выполните следующую команду:
   - python manage.py runserver

- Логин: pws_admin
- Пароль: sf_password

Деплой django-проекта на виртуальную машину:
1.  sudo git clone https://github.com/Nick-zkokaz/DZ_E5_M 
2. После клонирования проекта запускаем nginx:
   - sudo service nginx start
3. Создаем конфигурационный файл nginx для нашего сайта: 
   - sudo nano /etc/nginx/sites-available/DZ_E5_M 
4. Содержание конфигурационного файла:
server {
    listen 80;
    server_name IP; # где IP - публичный IP нашей ВМ
    location = /favicon.ico { access_log off; log_not_found off; }
    location / {
        proxy_pass http://127.0.0.1:8000;
    }
    location /static/ {
        root /home/user/projects/DZ_E5_M; # путь до папки статических файлов
    }
}
5. Проверим наш конфигурационный файл на корректность:
    sudo nginx -t
    Должно быть:
      nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
      nginx: configuration file /etc/nginx/nginx.conf test is successful
База данных 
1.) Теперь создадим базу данных (БД) в PostgreSQL:
    sudo -u postgres psql
2.) Выполняем следующие команды:
   CREATE DATABASE database_name; (где database_name - имя нашей БД)
   CREATE USER database_user WITH PASSWORD 'database_password'; (где database_user - имя пользователя БД, database_password - пароль для БД)
   GRANT ALL PRIVILEGES ON DATABASE database_name TO database_user;
   \q (выход из консоли postgres)
  ЗЫ. Если база будет пустой (по каким либо причинам не загрузится, то)
  INSERT INTO app_car (id,manufacturer,model,release_year,gearbox,color,photo) VALUES ('26','Коза','Коза Драная','2016','1','синий','photos/mazda-cx5_N2MXsGm.jpg');

Запуск проекта
i). В директории проекта:
    cd DZ_E5_M
ii). Создаем виртуальное окружение:
    python3.8 -m venv .venv
iii). Активируем виртуальное окружение:
    source .venv/bin/activate
iv). Создаем файл виртульного окружения (в той же папке , где находится settings.py)
   nano project/.env
    Впишем в него:
      - SECRET_KEY='very_strong_secret_key'
      - DB_NAME='database_name'
      - DB_USER='database_user'
      - DB_PASSWORD='database_password'
      - DB_HOST=127.0.0.1
      - DB_PORT=5432
   
   В settings.py добавляем или берем settings_Srv.py : 
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': os.getenv("DB_NAME"),
        'USER': os.getenv("DB_USER"),
        'PASSWORD': os.getenv("DB_PASSWORD"),
        'HOST': os.getenv("DB_HOST", "127.0.0.1"),
        'PORT': os.getenv("DB_PORT", 5432),
    }
}
v). Установим пакеты requirements.txt:
    pip3 install -r requirements.txt
vi). Готовим включение:
    python3.8 manage.py collectstatic
    python3.8 manage.py migrate
    python3.8 manage.py createsuperuser
    

запуск сервера командой:
   -python3.8 manage.py runserver

сервер отвечает по адресу http://84.201.174.145/ ПРОСЬБА проверяющему не удалять машины из списка
