# README

## 事前準備

* AWS CloudWatch Log Groupを作成
* ElasticBeanstalkアプリケーション作成
  * Platform: Multi docker container
  * Single Instance 
* RDS for Postgres 9.5 
* デプロイにはawsebcliを利用

## デプロイ

* init (初回) 

```
$ eb init --profile sandbox-eigo_fujikawa test2
```

* deploy (test2アプリケーション上のTest2-env-1にデプロイ)

```
$ eb deploy --profile sandbox-eigo_fujikawa Test2-env-1
```

## 参考

* Redash docker-compose in local

```
# create base pg_dump (only github maintainer)
$ git clone github.com/getredash/redash.git redash-original
$ cd redash-original
$ docker-compose build # if no ports mapping for postgresql, add 15432:5432 before build.
$ docker-compose up
```

* Redash DDL取得 (local)

```
# drop schema (only if reset required.)

$ psql -h127.0.0.1  --port 15432 -Upostgres 
drop schema public cascade;
create schema public;

# create table in docker container(only at first contact)
$ docker exec -it {redash server container_id} /bin/bash
bin/run ./manage.py database create_tables
$ pg_dump -h127.0.0.1  --port 15432 -Upostgres postgres > redash-data.sql
```

* Redashテーブル作成(AWS)

```
# create tables
redash-db-ssh # connect to postgresql on aws
redash-db < redash-data.sql # restore
```

* 参考

```
https://redash.io/help/open-source/dev-guide/setup
https://stackoverflow.com/questions/32311366/alembic-util-command-error-cant-find-identifier
https://qiita.com/a-suenami/items/e231adc2e083ef9449f6
```



