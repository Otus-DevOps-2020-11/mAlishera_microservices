# mAlishera_microservices
mAlishera microservices repository

# Домашняя работа к лекции №18 (docker-4)

- Добавлен запуск сервисов через docker-compose
- Добавлен .env файл
- Базовое имя проекта задается при запуске через опцию "-p"

# Домашняя работа к лекции №17 (docker-3)

- Собраны все сервисы
docker pull mongo:latest
docker build -t malishera/post:2.0 ./post-py
docker build -t malishera/comment:2.0 ./comment
docker build -t malishera/ui:3.0 ./ui

- Создана bridge-сеть и все собранные сервисы запущены в этой сети
docker network create reddit
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post malishera/post:1.0
docker run -d --network=reddit --network-alias=comment malishera/comment:1.0
docker run -d --network=reddit -p 9292:9292 malishera/ui:1.0

- Проверена работа приложения - открыли приложение запущенное по ссылке выше и написали тестовый пост
http://84.252.128.141:9292/

- Оптимизирован и пересобран ui образ
docker build -t malishera/ui:2.0 ./ui

- Выключены старые копии контейнеров и запущены новые с пересобранным образом ui:2.0
docker kill $(docker ps -q)

- Создан Docker volume и подключен к контейнеру mongo, чтобы не терять записи при остановке контейнера
docker volume create reddit_db
docker run -d --network=reddit --network-alias=post_db \
 --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit \
 --network-alias=post malishera/post:2.0
docker run -d --network=reddit \
 --network-alias=comment malishera/comment:2.0
docker run -d --network=reddit \
 -p 9292:9292 malishera/ui:3.0

# Домашняя работа к лекции №16 (docker-2)
# Docker контейнеры. Docker под капотом

- настроен travis и pre-commit для работы с новым репозиторием

- создан инстанс в yacloud -
yc compute instance create \
  --name docker-host \
  --zone ru-central1-a \
  --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 \
  --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \
  --ssh-key ~/.ssh/id_rsa.pub


- локально установлен docker-machine, с помощью него инициализорован докер хост с инстанстансом в yacloud
docker-machine create \
  --driver generic \
  --generic-ip-address=84.252.128.141 \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/id_rsa \
  docker-host

eval $(docker-machine env docker-host)

- в Dockerfile описан контейнер
- на основе описанного контейнера собран образ
- образ запушен на docker hub
docker-machine rm docker-host
