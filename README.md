# n8n with PostgreSQL

Starts n8n with PostgreSQL as database.

## Start

To start n8n with PostgreSQL simply start docker-compose by executing the following
command in the current folder.

**IMPORTANT:** But before you do that change the default users and passwords in the [`.env`](.env) file!

! for server use docker-compose.yml_server instead of docker-compose.yml
! if you have nginx installed on host add site configuration

root@goon:vi /etc/nginx/sites-available/n8n.genioussoft
```bash
server {
    server_name n8n.genioussoft.com;

    listen 443 ssl;

    ssl_certificate /etc/letsencrypt/live/n8n.genioussoft.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/n8n.genioussoft.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://127.0.0.1:5678;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_read_timeout 36000s;
        proxy_send_timeout 36000s;
    }
}

server {
    listen 80;
    server_name n8n.genioussoft.com;
    return 301 https://$host$request_uri;
}
```

```bash
docker-compose up -d
```

To stop it execute:

```bash
docker-compose stop
```

## Configuration

The default name of the database, user and password for PostgreSQL can be changed in the [`.env`](.env) file in the current directory.
