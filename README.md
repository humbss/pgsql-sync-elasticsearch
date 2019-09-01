# pgsql-sync-elasticsearch
# Push PGSQL data to Elasticsearch via Logstash
This stack will synchronize Postgresql database with Elasticsearch using Logstash.

This project is the basic docker compose stack for test/validation purpose only, for a production environment make sure that you had enabled security, cluster nodes, etc.

This stack is really solid, what I mean about solid? This software has been developed by community for many years, and the ecosystem is really harmonious, for instance, Logstash and Elasticsearch is maintained by the same group and PGSQL is considered the best opensource DB for enterprise use.

## Software Releases

* postgres:9.6-alpine
* logstash:7.3.1
* elasticsearch-oss:7.0.1
* (optional) dejavu - Elasticsearch client


```ditaa

             select from DB
       +--------------------+
       |                    |
       |                    |
       |                    v
+------+------+         +---+---------+          +-------------+
|             |         |             |          |             |
|  PostgreSQL |         |  Logstash   |          |  Elastic    |
|  DB         |         |             |          |  Search     |
|             |         |             |          |             |
|             |         |             |          |             |
+-------------+         +--------+----+          +----+--------+
                                 |                    ^
                                 |                    |
                                 |                    |
                                 +--------------------+
                                   Write to ES


```

## How does it work?

Logstash works with input/output pipelines, this config installs logstash-input-jdbc plugin in order to configure a read SQL query and push to Elasticsearch index.


## Config files

### pusher.conf
config file that contains the PGSQL config also the query.

## How to test it?
0. Run this compose:
   ` docker-compose -f docker-compose-indexer.yml up --force-recreate `
1. Create a PGSQL database called: test
2. Insert into a table called customer
3. Make sure the full-stack is running
4. Check the result in Dejavu
   `http://localhost:1358/?appname=customer&url=http://localhost:9200&mode=view`