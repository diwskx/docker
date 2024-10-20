FROM  debian:12.7
LABEL version="0.75"
LABEL description="Site blue-whale.com for test docker command"

# Копирование зеркала
COPY fi.list /etc/apt/sources.list.d/

# Обновление кеша и обновление пакетов
RUN apt-get update && apt-get upgrade -y

# Устанавливаем nginx, sudo
RUN apt-get install -y nginx sudo

# Очищаем кеш
RUN apt-get clean

# Удаляем содержимое директории /var/www/
RUN rm -rf /var/www/*

# Создаем директорию с именем сайта и папку с картинками
RUN mkdir -p /var/www/blue-whale.com/img

# Копируем index.html в /var/www/blue-whale.com/
COPY index.html /var/www/blue-whale.com/

# Копируем crib.jpg в /var/www/blue-whale.com/img/
COPY ./img/crib.jpg /var/www/blue-whale.com/img/

# Задаем рекурсивно на /var/www/blue-whale.com права "владельцу - читать, писать, исполнять; группе - читать, исполнять, остальным - только читать"
RUN chmod -R 755 /var/www/blue-whale.com

# Создаем пользователя
RUN adduser di

# Создаем группу
RUN groupadd wskx_group

# В группу помещаем пользователя
RUN usermod -aG wskx_group di

# Рекурсивно присвоить созданных пользователя и группу на папку /var/www/blue-whale.com
RUN chown -R di:wskx_group /var/www/blue-whale.com /var/lib/nginx/ /var/log/nginx/ /run/ /etc/nginx/

# Заменяем наименование сайта
RUN sed -i 's|/var/www/html|/var/www/blue-whale.com|g' /etc/nginx/sites-available/default

# Заменяем пользователя
RUN sed -i 's|www-data|di|g' /etc/nginx/nginx.conf

# Указываем порт для работы приложения в контейнере
EXPOSE 80

# Запуск от пользователя
USER di

# Запускаем nginx
CMD ["nginx", "-c", "/etc/nginx/nginx.conf", "-g", "daemon off;"]

