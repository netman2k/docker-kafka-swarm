## About
Apache Kafka 서비스를 Docker swarm cluster에서 기동할 수 있는 설정을 가진 저장소

## Dependency
zookeeper 서비스 
- https://tools.toastoven.net/daehyung.lee/docker-zookeeper-swarm

## How to use
```
docker stack deploy -c docker-stack.yml kafka

```
