How to run:
===
docker-compose up -d

Настройка
===
nano nano /etc/hosts

127.0.0.1       homework.local
127.0.0.1       phpmyadmin.local
127.0.0.1       wordpress.local
127.0.0.1       laravel.local
127.0.0.1       mail.local


phpMyAmin
===
$cfg['blowfish_secret'] = 'gIybc37SNJ8';
mkdir -p /var/www/phpmyadmin.local/tmp/

# Проверка
wp --allow-root --info

# Установка
wp --allow-root core download --locale=ru_RU --path=wordpress.local
wp --allow-root core config --dbname=wp.local --dbuser=root --dbpass=root --dbhost=db
wp --allow-root db create
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
