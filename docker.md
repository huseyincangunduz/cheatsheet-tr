# Docker Hile Kağıdı

Docker bir konteynırlaştırma aracıdır. Bir uygulamayı sisteminizde ayrı bir işletim sistemi sanal olarak başlatmadan (genelde linux) aynı donanım üstünde izole şekilde çalıştırır. Sistemin environmentından ve distrodan da etkilenmez.

Eğer sisteminizde docker kurulu değilse bu rehberi okuyabilirsiniz:

[Windows'ta docker kurulumu](https://docs.docker.com/desktop/setup/install/windows-install/)

[Linux sistemlerinde docker kurulumu](https://docs.docker.com/engine/install/ubuntu/)


Linuxta kurulum sonrasında bunu çalıştırırsanız, her seferinde "sudo / parola" yazmak zorunda kalmazsınız. Etki etmesi için bir kereliğine logout olunmalıdır.
```
sudo usermod -aG docker $USER
```
## Temel komutlar
### docker ps
```
docker ps # Halihazırda çalışan docker imajları görüntülenir

CONTAINER ID   IMAGE                COMMAND                  CREATED        STATUS       PORTS                                                                                                                   NAMES
80e3af064351   rnwood/smtp4dev:v3   "dotnet /app/Rnwood.…"   7 weeks ago    Up 7 hours   0.0.0.0:25->25/tcp, [::]:25->25/tcp, 0.0.0.0:143->143/tcp, [::]:143->143/tcp, 0.0.0.0:5051->80/tcp, [::]:5051->80/tcp   infrastructure-smtp4dev-1
63ace7b62c9b   mysql                "docker-entrypoint.s…"   6 months ago   Up 7 hours   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp                                                                  mysql-local-db-1
➜  ~ 

```

Eğer üstteki komut karışık görünüyorsa ` docker ps --format '{{.ID}} {{.Names}}' ` id ve isimlerini alabilirsiniz. -q ile sadece idlerini de alabilirisniz
```
➜  ~ docker ps --format '{{.ID}} {{.Names}}'
80e3af064351 infrastructure-smtp4dev-1
63ace7b62c9b mysql-local-db-1
➜  ~ 

```

Ayrıca `docker ps -a` ile çalışan çalışmayan tüm konteynerları listeleyebilirsiniz.


### docker stop/start KONTEYNER_ID
Docker konteynırlarını bu komutlarda durdurup yeniden başlatabilirsiniz

```
    docker stop 63ace7b62c9b
    docker start 63ace7b62c9b
```

### docker run imaj[:etiket]

Eğer farklı bir imaj çalıştırılmak isteniyorsa bu komut ile imaj çalıştırılabilir.

```
## basit
docker run mysql
## daha fazla parametreli
docker run -p 8080:8080 --gpus all --name local-ai -ti localai/localai:latest-aio-gpu-nvidia-cuda-12
```

Ekstra parametreler 

> Bu parametreler imajdan önce gelmelidir. Yoksa verdiğiniz parametreler çalışacak uygulamaya gönderilir.

- `-p 8582:8080` konteyner portu bilgisayarınızda açık hale getirir. Bilgisayarınıza :8582 portuna istek geldiği zaman konteynırınızın 8080 portuna yönlendirilir.
- `-name isim` konteynıra özel isim
- `--entrypoint /bin/blabla` konteynırda özel bir komut çalıştırmak istiyorsanız bunu kullanabilirsiniz
- `--rm` kapatıldığı anda silinir
- `-it --entrypoint /bin/sh` konteynırda terminalde işlem yapmanıza olanak tanır
- `--network network` hali hazırda olan networklerden birine bağlanır. Eğer ağdaki konteynırlara erişme gibi ihtiyacınız varsa bu önemlidir. Bu network isimlerine ``docker ps --format '{{.ID}} {{.Names}} {{.Networks}}'
`` komutundaki 3. kelimeye karşılık gelen ismi kullanabilir, ya da `docker network ls` yapabilirsiniz.



### docker exec -it konteyner_id/ismi bin
Çalışan herhangi bir konteynır içinde bir şey çalıştırır.
```
➜  ~ docker exec mysql-local-db-1 mysqldump --databases testo
-- MySQL dump 10.13  Distrib 9.1.0, for Linux (x86_64)
--
-- Host: localhost    Database: testo
-- ------------------------------------------------------
-- Server version       9.1.0
...

```

Burada -it ile terminal işlemleri yapabilirsiniz

```
➜  ~ docker exec -it mysql-local-db-1 mysql       
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 9.1.0 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use appdb
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select * from users;
Empty set (0.00 sec)

mysql> 

```

Eğer docker'da çalışan bir veritabanınız varsa, (örnek olarak mysql), bu şekilde export alabilirsiniz
```bash
docker exec konteyner_idsi_yada_ismi /usr/bin/mysqldump -u root --password=root veritabanıismi > backup.sql
```

Dockerda çalışan mysql instancesında bu şekilde import edilebilir
```bash
docker exec -i konteyner_idsi_yada_ismi mysql -ukullanıcıismi -pşifre123456 veritabanıismi < backup.sql
```
