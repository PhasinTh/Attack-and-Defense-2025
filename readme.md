# Lab for Attack and Defense

ลบคอมเม้น services ที่ต้องการเล่นในไฟล์ `cs.conf`

```
    services => [
        {id =>  4, name => 'passmgr',     tcp_port =>  80,   timeout => 120,  path => 'checkers/passmgr/checker.py'},
    ]
```

ลบคอมเม้น docker compose include ในไฟล์ `docker-compose.yml` สำหรับ service ที่ตรงกัน

```
include:
  - ./checkersystem/docker-compose.yml
  - ./services/passmgr/docker-compose.yml
```

รัน
```sh
docker compose down -v
docker compose up -d
```



***
ใช้ token เรียกดู flag_ids จะไม่เห็นของตัวเอง
