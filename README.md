# docker-compose Example for Apple M1 Local

## Folder Structure

```
project─┬─env─┬─config─┬─app1-nginx─┬─nginx.conf
        │     │        │            │
        │     │        │            └─default.conf
        │     │        │
        │     │        ├─app2-nginx─┬─nginx.conf
        │     │        │            │
        │     │        │            └─default.conf
        │     │        │
        │     │        ├─certs
        │     │        │
        │     │        └─mysql──my.conf
        │     │
        │     └─docker-compose.yml
        │
        ├─app─┬─app1(Replace Symbolic Link)
        │     │
        │     ├─app2(Replace Symbolic Link)
        │     │
        │     └─develop(Replace Symbolic Link)
        │
        ├─nginx-log─┬─app1
        │           │
        │           ├─app2
        │           │
        │           └─proxy
        │
        └─db─┬─data
             │
             ├─init-sql
             │
             └─log
```

- `env` : Container Build Settings Folder
- `app` : Application Folder
- `nginx-log` : Server Log Folder
- `db` : DB Folder (Log and Data)

init-sql example

```
CREATE DATABASE IF NOT EXISTS `database_name` CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
GRANT ALL ON database_name.* TO user_name
```

## Replace Sample Application with Symbolic Link

app1

```
ln -s <your-application-directory> app/app1
```

app2

```
ln -s <your-application-directory> app/app2
```

develop (for build frontend application)

```
ln -s <your-application-directory> develop
```

## Create Locally Trusted SSL Certificates with mkcert

```

mkcert -cert-file env/config/certs/shared.crt -key-file env/config/certs/shared.key localhost 127.0.0.1 ::1 example.com "*.example.com"

```

## Add to /etc/hosts file

```

127.0.0.1 app1.example.com
127.0.0.1 app2.example.com

```

## Connect with your browser

```

https://app1.example.com/
https://app2.example.com/

```

## Example Commands

### Build

```

docker-compose -f env/docker-compose.yml up -d

```

### Rebuild

```

docker-compose -f env/docker-compose.yml build --no-cache

```

### Delete Containers

```

docker-compose -f env/docker-compose.yml down

```

### Delete Containers and Images

```

docker-compose -f env/docker-compose.yml down --rmi all

```

### Stop Containers

```

docker-compose -f env/docker-compose.yml stop

```

### Into a Container

app1 server

```

docker exec -it nginx-1 /bin/sh --login

```

app2 server

```

docker exec -it nginx-2 /bin/sh --login

```

mysql connect

```

docker exec -it mysql8.0 /bin/bash --login -c "mysql -u docker -pdocker"

```

develop container (for build frontend application)

```
docker exec -it node /bin/sh --login
```
