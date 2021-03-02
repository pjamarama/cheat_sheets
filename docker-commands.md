<h2>Полезные командочки</h2>
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
sudo lsof -i:22 ## see a specific port such as 22 ##
top -p `pgrep -d "," postgres` // поиск по имени процесса. Если не найден, будет ошибка "требуется аргумент"


sudo docker run -it nginx bash // Внутри контейнера nginx:
sudo docker run nginx cat /etc/nginx/nginx.conf // возвращает содержимое nginx.conf из контейнера


Registry. Хранилище образов.
Репозиторий пакетов любого пакетного менеджера. https://store.docker.com/ (либо по старому адресу https://hub.docker.com/), кликнув по ссылке Containers.


docker pull // Для гарантированного обновления образа

docker logs container_id // вывод логов запущенного контейнера
docker logs -f container_id // обновляемый вывод логов

<h2>Образы</h2>
docker images // Список образов, уже скачанных на компьютер
docker stats // Инфа о потребляемых докером ресурсах


<h2>Контейнеры</h2>
Информация о запущенных контейнерах. -a все контейнеры (включая остановленные)
<ul>
<li>docker container ls</li>
<li>docker ps (old style)</li>
</ul>

Запуск и останов контейнера
docker container start/stop [container name1] [container name2] [...]

Остановка контейнера
docker kill container_id/alias

Удалить образ из докера. -f удалит образ и все контейнеры, с ним связанные.
docker rmi <imagename>

Удалить все "неиспользуемые" ресурсы:
docker system prune

Удалить ВСЕ существующие ресурсы:
docker system prune -a


Посмотреть адреса контейнера
ip addr show docker0
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' myContainerID

<h2>Работа внутри контейнера</h2>
sudo docker run -it nginx bash // Внутри контейнера nginx:
sudo docker run nginx cat /etc/nginx/nginx.conf // возвращает содержимое nginx.conf из контейнера


docker run -d -p 8080:80 -p 443:443 nginx // -d - работа в фоне. Выводит идентификатор контейнера и возвращает управление. -p 8080:80 пробрасывает порты 8080 и 443 извне на порты 80 и 443 внутри контейнера
docker run -it -v ~/.bash_history:/root/.bash_history ubuntu bash // пробрасывает часть файловой системы. Путь до файла во внешней системе должен быть абсолютным. Файлы во внутренней ФС создаются, существующие - затираются.
Переменные окружения:
Флаг -e. Используется он так: docker run -it -e "HOME=/tmp" ubuntu bash
Специальный файл, содержащий определения переменных окружения, который пробрасывается внутрь контейнера опцией --env-file.

<h2>Docker Hub</h2>
Войти в учетку докера:
docker login -u YOUR-USER-NAME

Пометить образ:
docker tag getting-started pjamarama/getting-started

Запушить образ на хаб:
docker push pjamarama/getting-started


Создание Dockerfile - файла с инструкциями для создания контейнера
FROM - указываем образ, от которого наследуемся. Образ лежит на докер хабе.
RUN - выпполнение команд shell. Команды должны быть неинтерактивными, -y подавляет дополнительные вопросы. Каждый вызов RUN формирует новый слой (набор изменяемых файлов). Кэширование слоёв позволяет улучшить производитльность.
COPY - копирует файл из основной системы внутрь образа. Файл должен лежать в той же папке, что и Dockerfile
ADD - позволяет больше, чем COPY. Если магия ADD не требуется, нужно использовать COPY.
WORKDIR - устанавливаем рабочую директорию. Все последующие инструкции выполняются внутри неё. При запуске контейнера он стартует из неё же.
CMD - определяет действие по умолчанию, если контейнер запущен без указания команды. Если при запуске контейнера указывались команды, инструкция CMD игнорируется.
ENTRYPOINT - выполняет команду при старте контейнера


Docker Compose - инструмент для запуска многоконтейнерных докер-приложений. Использует YAML-файл для конфигурирования сервисов.



Чуть подробнее:
- создать Dockerfile
- создaть файл для docker-compose
- запуск 
	запаковать с помощью gradle
	извлечь jar-ки
	запустить docker-compose build && docker-compose up

Из medium.com
1. create a volume for store data.
docker create -v /var/lib/postgresql/data --name PostgresData alpine
2. create the database.
docker run -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=admin -d --volumes-from PostgresData postgres
3. connect to db by containerid
docker exec -it <containerid> bash
psql -U postgres
create, insert, all that stuff

Запуск по отдельности:
docker run -d -p 5432:5432 postgres


From Sysout.ru
1. Запускаем контейнер с базой
docker run --name some-postgres --volume db-data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=qqq -e POSTGRES_DB=vv -p 5434:5432 postgres:12-alpine

2. создаем docker-compose.yml
docker-compose.yml лежит в корне иерархии рядом с папкой services, в которой перечислены сервисы – то есть компоненты, из который состоит наш проект.

3. запускаем docker-compose
docker-compose up




# Dockerfile for AWS
FROM openjdk:8-jdk-alpine
RUN addgroup -S timqergroup && adduser -S timeruser -G timergroup
USER timeruser:timqergroup




Еще подход:
sudo docker run -d --name some-postgres --volume db-data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=timer -e POSTGRES_DB=timer_db -p 5434:5432 postgres:12-alpine

sudo docker run -d --name postgresTimer2 --volume /var/lib/postgresql/12:/var/lib/postgresql/data -e POSTGRES_PASSWORD=timer -e POSTGRES_DB=timer_db -p 5434:5432 postgres:12-alpine

docker create -v /var/lib/postgresql/12 --name postgresTimer2 alpine
docker run -p 5432:5432 -e POSTGRES_PASSWORD=timer -d --volumes-from postgresTimer2 postgres

Восстановить базу:
C:\Program Files\PostgreSQL\11\bin\pg_restore.exe --host "ec2-3-16-55-108.us-east-2.compute.amazonaws.com" --port "5432" --username "timer"  --dbname "new_timer_db" --verbose "C:\\Users\\ALEKSE~1.GRO\\DOCUME~1\\timerman\\BACKUP~1.BAC"


DB stuff:
\c - connect to database
\d - list relations


To create a new role:
CREATE ROLE rolename LOGIN;

Then make the new role a superuser:
ALTER ROLE rolename WITH SUPERUSER;

Change a role's password:
ALTER ROLE davide WITH PASSWORD 'hu8jmn3';
