Start Redis & Postgres.

Setup nginx for Sentry first see nginx & ssl directores:
```
git clone git@bitbucket.org:mptnt1988/docker_containers.git
```
Generate secret key:
```
docker run --rm sentry config generate-secret-key
# -> c=5jlv1pxr)#b&ymwd+%@w%b7aa7=07ap-p782&ldb7gq=s&(w
```
Setup Postgres user & DB:
```
docker exec -it postgres bash
su - postgres
psql
postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
postgres=# alter user sentry with Superuser;
```
Run sentry upgrade to init database:
```
docker run -it --rm -e SENTRY_SECRET_KEY='c=5jlv1pxr)#b&ymwd+%@w%b7aa7=07ap-p782&ldb7gq=s&(w' -e SENTRY_POSTGRES_HOST=postgres -e SENTRY_POSTGRES_PORT=5432 -e SENTRY_DB_NAME=sentry -e SENTRY_DB_USER=sentry -e SENTRY_DB_PASSWORD=asdfasdf --link postgres:postgres --link redis:redis --restart always sentry upgrade
```
Add user account: **mptnt1988@gmail.com/asdfasdf**

Run Sentry server:
```
docker run -d --hostname sentry.local.tech --name sentry -e SENTRY_SECRET_KEY='c=5jlv1pxr)#b&ymwd+%@w%b7aa7=07ap-p782&ldb7gq=s&(w' -e SENTRY_POSTGRES_HOST=postgres -e SENTRY_POSTGRES_PORT=5432 -e SENTRY_DB_NAME=sentry -e SENTRY_DB_USER=sentry -e SENTRY_DB_PASSWORD=asdfasdf -e TZ=Asia/Ho_Chi_Minh --link redis:redis --link postgres:postgres -p 127.0.0.1:9000:9000 --restart always sentry
```
with root URL: **https://sentry.local.tech** and Admin Email **mptnt1988@gmail.com**

Run Sentry beat:
```
docker run -d --hostname sentry-cron.local.tech --name sentry-cron -e SENTRY_SECRET_KEY='c=5jlv1pxr)#b&ymwd+%@w%b7aa7=07ap-p782&ldb7gq=s&(w' -e SENTRY_POSTGRES_HOST=postgres -e SENTRY_POSTGRES_PORT=5432 -e SENTRY_DB_NAME=sentry -e SENTRY_DB_USER=sentry -e SENTRY_DB_PASSWORD=asdfasdf -e TZ=Asia/Ho_Chi_Minh --link postgres:postgres --link redis:redis --restart always sentry run cron
```
and Sentry worker:
```
docker run -d --hostname sentry-worker-1.local.tech --name sentry-worker-1 -e SENTRY_SECRET_KEY='c=5jlv1pxr)#b&ymwd+%@w%b7aa7=07ap-p782&ldb7gq=s&(w' -e SENTRY_POSTGRES_HOST=postgres -e SENTRY_POSTGRES_PORT=5432 -e SENTRY_DB_NAME=sentry -e SENTRY_DB_USER=sentry -e SENTRY_DB_PASSWORD=asdfasdf -e TZ=Asia/Ho_Chi_Minh --link postgres:postgres --link redis:redis --restart always sentry run worker
```
