### 클러스터 마스터 수동 생성
##### --cluster-replicas 0 -> 수동으로 slacve 추가 (1의 경우 슬레이브를 뒤에 추가하면 마스터 노드에 자동으로 슬레이브 추가)
```
docker exec -it redis-master-1 redis-cli --cluster create redis-master-1 [ip 주소]:[포트번호] redis-master-2 [ip 주소]:[포트번호] redis-master-3 [ip 주소]:[포트번호] --cluster-replicas 0
```
### 클러스터 마스터에 슬레이브 추가 
- 로컬 환경인 경우 127.0.0.1 or localhost or host.docker.internal 삽입
- 외부 클라우드에 배포 (AWS, 개인 서버등등) 해당 서버의 외부 ip 주소를 삽입
```
docker exec -it redis-cli --cluster add-node [ip 주소]:[슬레이브 포트번호] [ip 주소]:[마스터 포트번호] --cluster-slave
```

### 클러스터 상태 확인
```
docker exec -it redis-master-1 redis-cli -p [포트번호] cluster info
```

```
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
```

### 클러스터 노드 상태 확인
```
docker exec -it redis-master-1 redis-cli -p [포트번호] cluster nodes
```

### 외부 접근 테스트
```
redis-cli -c -h [외부 ip 주소] -p [포트번호]
```

### 클러스터 생성 중 오류 발생 시 노드들을 초기화 한 상태로 처음부터 다시 시작
#### 마스터 노드 초기화
```
docker exec -it redis-master-1 redis-cli -p [포트번호] cluster reset hard
```
### 슬레이브 노드 초기화
```
docker exec -it redis-slave-1 redis-cli -p [포트번호] cluster reset hard
```

클러스터 마스터 수동 생성 -> 클러스터 마스터에 슬레이브 추가 -> 클러스터 상태 확인
