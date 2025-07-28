# Lab for Attack and Defense
## รัน Checker System
เปิดโฟลเดอร์ checkersystem
ลบคอมเม้น services ที่ต้องการเล่นในไฟล์ `cs.conf`

```
    services => [
        {id =>  4, name => 'passmgr',     tcp_port =>  80,   timeout => 120,  path => 'checkers/passmgr/checker.py'},
    ]
```

แก้ไขเวลา ในไฟล์ `cs.conf`
```
    postgres_uri => {'postgresql://postgres:Secr3t@pg:5432/cs'},
    cs => {
        base_url => 'http://localhost:8080',
        time => [['2025-07-28 00:00:00', '2025-07-28 23:59:00']],
        ...
    }
```

รัน
```sh
docker compose down -v
docker compose up -d
```

## รัน Vulnerable Service
เปิดโฟลเดอร์ services/(lab ที่ต้องการ) เช่น services/notes

รัน
```sh
docker compose down -v
docker compose up -d
```


***
ใช้ token เรียกดู flag_ids จะไม่เห็นของตัวเอง
