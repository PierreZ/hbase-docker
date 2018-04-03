# hbase-docker
Dockerfiles to have images for HBase

## Why ?

Because sometimes, you need some standalone HBase to use into some docker-compose.

## But why packaging the distributed versions?

Because sometimes, you need to run the distributed version of HBase into Kubernetes using ConfigMap.

## What is available?

* HBase Standalone:
    * 1.3.1 with embedded ZK
    * 1.3.1 without ZK
* Hbase distributed
    * 1.3.1 with Hadoop 2.7.3

## How can I use it?

#### Standalone with embedded ZK

```bash
docker run -it \
        -p 8080:8080 \
        -p 9090:9090 \
        -p 2181:2181 \
        -p 16000:16000 \
        -p 16010:16010 \
        -p 16020:16020 \
        -p 16030:16030 \
      pierrezemb/hbase-docker:standalone-1.3.1-embedded-zk
```

### Standalone without ZK (docker-compose example)

```bash 
cat docker-compose.yml

# ...
  zookeeper:
    image: wurstmeister/zookeeper
    expose:
      - "2181"
    logging:
        driver: "none"

  hbase:
    image: pierrezemb/hbase-docker:standalone-1.3.1
    links:
      - zookeeper
    logging:
      driver: "none"
    ports:
      - "16010:16010"
    expose:
      - "8080"
      - "9090"
      - "16000"
      - "16010"
      - "16020"
      - "16030"
    command: ["/wait-for-it.sh", "zookeeper:2181", "-t", "10", "--", "/usr/bin/supervisord"]
# ...
```

#### HBase Master server interface
http://localhost:16010/master-status

#### Verifying HBase REST interface
```
# HBase Version
[user@host ~]$ curl http://localhost:8080/version/cluster

# Cluster Status
[user@host ~]$ curl http://localhost:8080/status/cluster

# Table List
[user@host ~]$ curl http://localhost:8080/
```
### Accessing the HBase shell
```
[user@host ~]$ docker exec -it hbase bash
user@host:/opt# hbase shell
```