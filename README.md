# mAlishera_microservices
mAlishera microservices repository

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
  --generic-ip-address=130.193.38.79 \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/id_rsa \
  docker-host
  
- в Dockerfile описан контейнер
- на основе описанного контейнера собран образ
- образ запушен на docker hub