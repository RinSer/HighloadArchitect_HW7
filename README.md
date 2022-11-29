## Инструкция для запуска:

1) Установить Docker и python 3.8
2.1) Выполнить в текущей директории CLI команду `docker build --build-arg cnf=node1.cnf -t ubuntu:galera-node1 ./` для создания образа мастер ноды кластера.
2.2) Выполнить в текущей директории CLI команду `docker build --build-arg cnf=node2.cnf -t ubuntu:galera-node2 ./slave` для создания образа слейв ноды кластера.
2.3) Выполнить в текущей директории CLI команду `docker build -t centos:tarantool ./tarantool` для создания образа ноды с tarantool.
3.1) Создаем сеть в докере: `docker network create --subnet=192.168.0.0/24 hw7`.
3.2) Запуск первой ноды кластера: `docker run -i -d --name Node1 --hostname node1 --network="hw7" --ip=192.168.0.101 -p 3301:3306 -v /var/container_data/mysql:/var/lib/mysql ubuntu:galera-node1`.
3.3) Добавляем пользователя приложения и БД:
`mysql -u root -p`
`GRANT ALL ON *.* to flask@'192.168.0.1' IDENTIFIED BY 'ksalf';`
`FLUSH PRIVILEGES;`
`CREATE DATABASE social_network;`
4) Запускаем tarantool: `docker run -i -d --name Tarantool --hostname tarantool --network="hw7" --ip=192.168.0.103 -p 3303:3301 centos:tarantool`.
5) Выполнить в текущей директории CLI команду `pip install -r requirements.txt` для установки зависимостей приложения.
6) Выполнить в текущей директории CLI команду `py -3 -m flask --app app run` для локального запуска приложения (порт 5000).

Обновить requirements.txt: `py -3 -m  pipreqs.pipreqs --force`