# home

## Установка и настройка сервера Matrix Synapse

Установка и настройка сервера Matrix Synapse с PostgreSQL, Admin UI (Synapse  Admin) и Element Web на Debian требует нескольких этапов. Ниже приведена пошаговая инструкция.

## Подготовка сервера

**Требования:**

- Debian (рекомендуется Debian 11/12);
- минимум 2 ГБ RAM и 1 CPU;
- доменное имя, направленное на IP-адрес сервера;
- доступ к серверу с правами root. 

**Настройка фаервола:**

```
bash
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 8448
sudo ufw status
```

## Установка Matrix Synapse

1. Добавьте репозиторий Matrix.org:

```
bash
sudo wget -O /usr/share/keyrings/matrix-org-archive-keyring.gpg https://packages.matrix.org/debian/matrix-org-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/matrix-org-archive-keyring.gpg] https://packages.matrix.org/debian/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/matrix-org.list
```

1. Обновите список пакетов и установите Synapse:

```
bash
sudo apt update
sudo apt install matrix-synapse-py3
```

Во время установки укажите имя сервера (например, `matrix.example.com`). Позже его можно изменить в файле `/etc/matrix-synapse/conf.d/server_name.yaml`. docs.vultr.com +1

## Установка и настройка PostgreSQL

1. Установите PostgreSQL:

```
bash
sudo apt install postgresql
```

1. Войдите в PostgreSQL как пользователь `postgres`:

```
bash
sudo -su postgres
```

1. Создайте пользователя и базу данных для Synapse:

```
sql
createuser --pwprompt synapse
createdb --encoding=UTF8 --locale=C --template=template0 --owner=synapse synapse
```

1. Вернитесь к обычному пользователю:

```
bash
exit
```

## Настройка Synapse для работы с PostgreSQL

В файле `/etc/matrix-synapse/homeserver.yaml` или в отдельном файле в директории `conf.d` укажите параметры подключения к PostgreSQL:

```
yaml
database:
  name: psycopg2
  args:
    user: synapse
    password: [ваш_пароль]
    database: synapse
    host: 127.0.0.1
    cp_min: 5
    cp_max: 10
```

## Установка и настройка NGINX с TLS

1. Установите NGINX и Certbot:

```
bash
sudo apt install nginx certbot python3-certbot-nginx
```

1. Настройте обратный прокси в NGINX. Пример конфигурации:

```
nginx
server {
    listen 80;
    server_name matrix.example.com;
    location /.well-known/acme-challenge/ {
        root /var/www/html;
    }
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl http2;
    server_name matrix.example.com;
    ssl_certificate /etc/letsencrypt/live/matrix.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/matrix.example.com/privkey.pem;

    location ~ ^(/_matrix|/_synapse/client) {
        proxy_pass http://localhost:8008;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host $host;
        client_max_body_size 100M;
        proxy_http_version 1.1;
    }
}
```

1. Получите SSL-сертификат с помощью Certbot:

```
bash
sudo certbot --nginx -d matrix.example.com
```

## Установка Element Web

1. Создайте директорию для Element:

```
bash
sudo mkdir -p /var/www/element
cd /var/www/element
```

1. Скачайте последнюю версию Element с GitHub:

```
bash
latest="$(curl -s https://api.github.com/repos/vector-im/element-web/releases/latest | jq -r .tag_name)"
sudo wget https://github.com/vector-im/element-web/releases/download/${latest}/element-${latest}.tar.gz
```

1. Распакуйте архив и создайте символическую ссылку:

```
bash
sudo tar xf element-${latest}.tar.gz
sudo ln -s element-${latest} current
```

1. Настройте конфигурацию Element. Скопируйте `config.sample.json` в `config.json` и отредактируйте его:

```
bash
cd current
sudo cp config.sample.json config.json
sudo nano config.json
```

Укажите ваш домен в полях `base_url` и `server_name`:

```
json
"m.homeserver": {
  "base_url": "https://matrix.example.com",
  "server_name": "matrix.example.com"
}
```

## Установка Synapse Admin (Admin UI)

1. Клонируйте репозиторий Synapse Admin:

```
bash
git clone https://github.com/Awesome-Technologies/synapse-admin.git
```

1. Соберите проект (если требуется):

```
bash
npm install
npm run build
```

1. Настройте NGINX для доступа к админ-панели. Добавьте в конфигурацию NGINX блок:

```
nginx
location /admin {
    proxy_pass http://localhost:8080;
    proxy_set_header X-Forwarded-For $remote_addr;
}
```

1. Запустите Synapse Admin (например, через Docker или вручную).

## Создание первого администратора

Создайте первого пользователя с правами администратора:

```
bash
sudo register_new_matrix_user -c /etc/matrix-synapse/homeserver.yaml http://127.0.0.1:8008 -u ваш-логин -p ваш-пароль --admin
```

## Дополнительные настройки

- **Отключение публичной регистрации.** В файле `/etc/matrix-synapse/conf.d/90-custom.yaml` установите `enable_registration: false` и перезапустите Synapse. 

- **Настройка Coturn** (для поддержки звонков через WebRTC). Установите `coturn` и настройте его, указав `turn_uris` и `turn_shared_secret` в конфигурации Synapse. habr.com +1
- **Резервное копирование.** Регулярно создавайте резервные копии базы данных PostgreSQL и конфигурационных файлов.

## Проверка работы

- Зайдите в Element Web по адресу `https://matrix.example.com` и авторизуйтесь с созданными учётными данными.
- Проверьте доступ к админ-панели по адресу `https://matrix.example.com/admin`.

Если возникнут проблемы, проверьте логи Synapse (`/var/log/matrix-synapse/homeserver.log`) и NGINX.

**Важно:** убедитесь, что все компоненты обновлены до последних версий, и регулярно обновляйте систему.



