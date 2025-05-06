# Docker compose

## Nedir?

Docker'ın sağladığı işlevleri daha da kolaylaştıran bir araçtır.
Bu araç bir yml dosyasını referans alarak servislerinizi yönetmenizi sağlar.

```yml

# Bu dosyaların syntaxları versiyona göre değişebiliyor. ancak siz belirtmeyebilirsiniz çünkü deprecated oldu
version: "3.5"

services:

  # Servis ismi
  web:
    # Kullandığı imaj
    image: kompaney-web
    # Port ayarıdır. Burada ilk bilgisayarınızda açılacak port yazılır, ardından iki nokta üst üste eklenip konteynırınızın, ve yahut uygulamanızın kabul ettiği port yazılır. 
    # Örn: Bilgisayarınızda 80 ancak uygulamanız 666 portunda ayağa kalkıyorsa "80:666" olarak yazılır 
    # Eğer ikisi aynıysa (örn 80 ise) "80" diye de geçebilirsiniz 
    ports:
      - "80:80"
      - "443:443"

    # Aynı klasördeki dosyaları konteynırınızın kullanması için volume ayarını yapabilirsiniz. Sırasıyla aralarda iki nokta üst üste olacak şekilde
    # - bilgisayardaki aynı klasörden başlayacak şekilde path
    # - konteynırınızda denk gelecek olan path
    # - eğer üstüne yazıp değiştirsin istemiyorsanız "ro" 
    volumes:
      - ./config/nginx/kompaney-web.conf:/etc/nginx/nginx.conf:ro
      - ./certs:/certs
      - ./html-files:/usr/share/nginx/html

  api:
    image: "kompaney-api"
    # Eğer gerekliyse environment variable verebilirsiniz.
    # Buradaki örnekte veritabanı tipi vs. vereceğiz
    environment:
      - NX_MICROSERVICE_TYPE=KAFKA
      # Fun fact: konteynırlar bilgisayarınıza 172.17.0.1 ipsi ile istek atabilir 
      - NX_KAFKA_URL=172.17.0.1:9092
      # Fun fact: konteynırlar "servis isimleri" ile birbirlerine istek atabilir
      - NX_MONGO_URL=my-mongodb:27017
      - NX_MONGO_DBNAME=kompaney
      # Fun fact: docker-compose.yml dosyasının yanında .env dosyası açıp "${}" içindekileri doldurabilirsiniz. yoksa default olarak boş değer verecektir, ya da :"DEĞER" bişi eklendiyse bu gelecektir
      - NX_MONGO_USERNAME=${NX_MONGO_USERNAME}
      - NX_MONGO_PASSWORD=${NX_MONGO_PASSWORD}
      - NX_SECRET_JWT=${NX_SECRET_JWT}
      - NX_SECRET_PW=${NX_SECRET_PW}

 
  my-mongodb:
    image: mongo:4.2.3-bionic
    container_name: sifali-veritabani
    environment:
      - MONGO_INITDB_DATABASE=test
      - MONGO_INITDB_ROOT_USERNAME=${NX_MONGO_USERNAME}
      - MONGO_INITDB_ROOT_PASSWORD=${NX_MONGO_PASSWORD}
    volumes:
      - ./mongo-entrypoint:/docker-entrypoint-initdb.d
      - ./mongo-data:/data/db
      - ./mongoconfig:/data/configdb


```

Bu dosya docker-compose.yml olarak kaydedilir ve bu şekilde çalıştırılır.

```
docker compose up
```

- Bu andan itibaren servislerinizin çıktılarını döndürecektir.
Kapatmak için CTRL+C yapabilirsiniz.

Arkaplanda çalışsın isterseniz sonuna `-d` eklemelisiniz

```
docker compose up -d
```

Böylece arkaplanda çalışacaktır ve terminalinizi kapatabileceksiniz. Arkaplanda çalışmasını istemediğiniz anda stop komutunu çalıştırabilirsiniz

```
docker compose stop
```

Sadece spesifik bir servisi kapatıp açacaksanız:

```
docker compose stop api
docker compose start api
```

Logları görüntülemek için ("-f" kapanana kadar bekletir):

```
docker compose logs -f
docker compose logs -f api # sadece bir servis
docker compose logs -f api web # sadece belirli servisler
```