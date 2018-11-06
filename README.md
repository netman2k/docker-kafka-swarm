## About
Apache Kafka 서비스를 Docker swarm cluster에서 기동할 수 있는 설정을 가진 저장소

## Requirements
1. Node label 추가
다음은 3대의 노드에 각각 kafka label과 해당되는 번호를 부여한 예이다.

```
docker node update --label-add kafka=1 infa-swarm-t1904
docker node update --label-add kafka=2 infa-swarm-t1905 
docker node update --label-add kafka=3 infa-swarm-t1906 
```
### 장애대비 설정
Kafka 컨테이너는 설정된 레이블이 있어야 컨테이너를 기동하게된다. 하지만 만일 해당 호스트가 다운되면
Kafka 컨테이너를 실행할 호스트가 사라지게된다.  

이를 해결하기위해 다른 호스트에 동일 레이블을 각각 설정해 놓게될 경우, kafka 서비스에 할당된 
replicas: 1 설정에의해 해당 레이블이 있는 다른 호스트에 자동으로 컨테이너를 띄울 수 있게 된다.

따라서 동일 순간 한대의 장애가 있을 경우를 대비할 수 있게 된다.

```
docker node update --label-add kafka=1 infa-swarm-t1901 
docker node update --label-add kafka=2 infa-swarm-t1902 
docker node update --label-add kafka=3 infa-swarm-t1903
```

2. NETWORK 설정 및 환경변수 저장

```
docker network create -d overlay --attachable kafka
export NETWORK_BACKEND=kafka
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

