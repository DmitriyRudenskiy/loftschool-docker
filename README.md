### Удаляем старые контенеры
docker rm -f $(docker ps -a -q)

### Собираем окружение
docker-compose up -d --build

### Запускаем
Docker Quickstart Terminal

```bash
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/

docker is configured to use the default machine with IP 192.168.99.100 <-- этот IP нужно запомнить (У каждого он будет свой)
For help getting started, check out the docs at https://docs.docker.com
```

### Настройка хостов
Окрываем в блокноте С:/Windows/System32/drivers/etc/hosts

```bash
192.168.99.100       homework.local
192.168.99.100       phpmyadmin.local
192.168.99.100       wordpress.local
192.168.99.100       laravel.local
192.168.99.100       mail.local
```

Linux
sudo nano /etc/hosts
```bash
127.0.0.1       homework.local
127.0.0.1       phpmyadmin.local
127.0.0.1       wordpress.local
127.0.0.1       laravel.local
127.0.0.1       mail.local
```

phpMyAmin
===
mkdir -p /var/www/phpmyadmin.local/tmp/
mv config.sample.inc.php config.inc.php

nano config.inc.php
$cfg['Servers'][$i]['host'] = 'mysql57';
$cfg['blowfish_secret'] = 'gIybc37SNJ8';


# Проверка
wp --allow-root --info

# Установка
wp --allow-root core download --locale=ru_RU --path=wordpress.local
wp --allow-root core config --dbname=wp.local --dbuser=root --dbpass=root --dbhost=db
wp --allow-root db create (можно создать вручную)
wp --allow-root core install --url='wordpress.local' --title='Тестовый сайт' --admin_user='admin' --admin_password='123' --admin_email='test@example.com'

# Очистка
wp --allow-root plugin delete --all
wp --allow-root theme install wp-bootstrap-starter --activate
wp --allow-root theme delete twentynineteen
wp --allow-root theme delete twentyseventeen
wp --allow-root theme delete twentysixteen

# Наполняем
wp --allow-root user generate --count=5 --role=editor
wp --allow-root user generate --count=10 --role=author
wp --allow-root term generate --count=12
wp --allow-root post generate --count=3000
wp --allow-root comment generate --count=10000

# Устанавливаем
composer create-project laravel/laravel laravel.local
