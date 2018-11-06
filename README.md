## About
Apache Kafka 서비스를 Docker swarm cluster에서 기동할 수 있는 설정을 가진 저장소

## Requirements
1. Node label 추가
다음은 3대의 노드에 각각 kafka label과 해당되는 번호를 부여한 예이다.

```
[root@infa-swarm-wa901 docker-kafka-swarm]# docker node update infa-swarm-t1903 --label-add kafka=1
infa-swarm-t1903
[root@infa-swarm-wa901 docker-kafka-swarm]# docker node update infa-swarm-t1904 --label-add kafka=2
infa-swarm-t1904
[root@infa-swarm-wa901 docker-kafka-swarm]# docker node update infa-swarm-t1905 --label-add kafka=3
```

2. NETWORK 설정 및 환경변수 저장

```
docker network create -d overlay --attachable kafka
export NETWORK_BACKEND=kafka
```

```
## Apache Zookeeper ensemble 실행
다음 커맨드는 Zookeeper ensemble 구성을 위한 3개의 Zookeeper, 그리고 zookeeper-exporter를 구동시킨다.

* zookeeper-export의 기본 포트는 8084이다.

```
docker stack deploy -c zookeeper.yml zookeeper
```

## Apache Kafka cluster 실행
다음 커맨드는 Apache Kafka cluster 구성을 위한 3개의 Kafka, Kafka Manager, 그리고 kafka-exporter를 구동시킨다.

* kafka manager의 서비스 포트는 9001이다.
* kafka-export의 서비스 포트는 9308이다.

```
docker stack deploy -c kafka.yml kafka
```

