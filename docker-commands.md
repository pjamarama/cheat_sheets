<h2>Образы</h2>

**docker images** // Список образов, уже скачанных на компьютер<br>
**docker stats** // Инфа о потребляемых докером ресурсах<br>
**docker pull** // Для гарантированного обновления образа<br>
**docker build -t image-name .** // Создание образа


<h2>Контейнеры</h2>
Информация о запущенных контейнерах. -a все контейнеры (включая остановленные)
<ul>
<li>

**docker container ls**</li>
<li>

**docker ps (old style)**</li>
</ul>

Логи
<ul>

<li>

**docker logs container_id** // вывод логов запущенного контейнера</li>
<li>

**docker logs -f container_id** // обновляемый вывод логов</li>
</ul>

Запуск и останов контейнера:<br>
**docker container start/stop [container name1] [container name2] [...]**

Остановка контейнера:<br>
**docker kill container_id/alias**

Удалить образ из докера. -f удалит образ и все контейнеры, с ним связанные:<br>
**docker rmi <imagename>**

Удалить все "неиспользуемые" ресурсы:<br>
**docker system prune**

Удалить ВСЕ существующие ресурсы:<br>
**docker system prune -a**


Посмотреть адреса контейнера:<br>
**ip addr show docker0**<br>
**docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' myContainerID**

<h2>Работа внутри контейнера</h2>

**sudo docker run -it nginx bash** // Внутри контейнера nginx <br>
**sudo docker run nginx cat /etc/nginx/nginx.conf** // возвращает содержимое nginx.conf из контейнера<br>

**docker run -d -p 8080:80 -p 443:443 nginx** // -d - работа в фоне. Выводит идентификатор контейнера и возвращает управление. -p 8080:80 пробрасывает порты 8080 и 443 извне на порты 80 и 443 внутри контейнера<br>

**docker run -it -v ~/.bash_history:/root/.bash_history ubuntu bash** // пробрасывает часть файловой системы. Путь до файла во внешней системе должен быть абсолютным. Файлы во внутренней ФС создаются, существующие - затираются.<br>

Переменные окружения:<br>
Флаг -e. Используется он так:<br>
**docker run -it -e "HOME=/tmp" ubuntu bash**

Специальный файл, содержащий определения переменных окружения, который пробрасывается внутрь контейнера опцией --env-file.



<h2>Volumes</h2>

Создать именованный том:<br>
**docker volume create volume-name**<br>

Параметр для запуска контейнера с томом:<br>
<ul>

**<li>-v volume-name:/etc/volumename</li>**
**<li>-v /path/to/data:/usr/local/data**<li>
</ul>

**-w /app** - sets the “working directory” or the current directory that the command will run from<br>
**-v "$(pwd):/app"** - bind mount the current directory from the host in the container into the /app directory<br>

<h2>Network</h2>
<ol>
<li>Create the network:<br>
**docker network create network-name**</li>

<li>Start a MySQL container and attach it to the network.<br>
**docker run -d --network network-name --network-alias postgres -v mydb:/var/lib/mydb -e POSTGRES_PASSWORD=mysecretpassword postgres**</li>

<li>Сonnect to the database and verify it connects.<br>

**docker exec -it <mysql-container-id> mysql -p</li>**
</ol>

Получить информацию о сетевых параметрах с помощью Netshoot:<br>
<ul>
<li>

**docker run -it --network todo-app nicolaka/netshoot**</li>

<li>

**dig mysql** // mysql здесь, это параметр --network-alias</li>

<li>

или проще: **docker inspect flamboyant_bardeen | grep IPAddress**</li>
</ul>


<h2>Docker Hub</h2>

Войти в учетку докера:
**docker login -u YOUR-USER-NAME**

Пометить образ:
**docker tag getting-started pjamarama/getting-started**

Запушить образ на хаб:
**docker push pjamarama/getting-started**


Создание Dockerfile - файла с инструкциями для создания контейнера
FROM - указываем образ, от которого наследуемся. Образ лежит на докер хабе.
RUN - выпполнение команд shell. Команды должны быть неинтерактивными, -y подавляет дополнительные вопросы. Каждый вызов RUN формирует новый слой (набор изменяемых файлов). Кэширование слоёв позволяет улучшить производитльность.
COPY - копирует файл из основной системы внутрь образа. Файл должен лежать в той же папке, что и Dockerfile
ADD - позволяет больше, чем COPY. Если магия ADD не требуется, нужно использовать COPY.
WORKDIR - устанавливаем рабочую директорию. Все последующие инструкции выполняются внутри неё. При запуске контейнера он стартует из неё же.
CMD - определяет действие по умолчанию, если контейнер запущен без указания команды. Если при запуске контейнера указывались команды, инструкция CMD игнорируется.
ENTRYPOINT - выполняет команду при старте контейнера


Docker Compose - инструмент для запуска многоконтейнерных докер-приложений. Использует YAML-файл для конфигурирования сервисов.



# Dockerfile for AWS
FROM openjdk:8-jdk-alpine
RUN addgroup -S timqergroup && adduser -S timeruser -G timergroup
USER timeruser:timqergroup

 docker run --rm -dp 3000:3000 -w /app -v "$(pwd):/app" node:12-alpine sh -c "yarn install && yarn run dev"


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


# Official documentation 'getting started'
1. Создать сеть:
docker network create todo-app

2. Запустить контейнер с дб
docker run -d \
     --network todo-app --network-alias mysql \
     -v todo-mysql-data:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=secret \
     -e MYSQL_DATABASE=todos \
     mysql:5.7

3. Запустить контейнер с приложением
docker run --rm -dp 3000:3000 \
   -w /app -v "$(pwd):/app" \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:12-alpine \
   sh -c "yarn install && yarn run dev"

4. проверить подключение к бд
docker logs <container-id>


# AWS config

<h2>I. Database</h2>

docker network create timer-network
*("Subnet": "172.18.0.0/16", "Gateway": "172.18.0.1")*
*;; ANSWER SECTION:*
*for-postgres.           600     IN      A       172.18.0.2*

docker volume create timer-volume
*("Mountpoint": "/var/lib/docker/volumes/timer-volume/_data",)*

docker run -dp 5432:5432 --network timer-network --network-alias for-postgres \
 -v timer-volume:/var/lib/postgresql/data
 -e POSTGRES_PASSWORD=timer \
 -e POSTGRES_USER=timer \
 -e POSTGRES_DB=timer_db \
 postgres:11

 *elastic_margulis*

Открыть tcp/5432 снаружи

Залить данные
C:\Program Files\PostgreSQL\11\bin\pg_restore.exe --host ec2-3-21-28-177.us-east-2.compute.amazonaws.com	 --port "5432" --username "timer" --dbname "timer_db" --verbose "C:\\Users\\aleksey.grokhotov\\Documents\\timerman\\timer_db.sql"

Закрыть tcp/5432 снаружи

<h2>II. Application</h2>


    1. Создать Dockerfile
        FROM openjdk:8-jdk-alpine
        RUN addgroup -S spring && adduser -S spring -G spring
        USER spring:spring
        ARG JAR_FILE=*.jar
        COPY ${JAR_FILE} app.jar
        ENTRYPOINT ["java","-jar","/app.jar"]

    2. Создать образ. Запускаем из папки, где лежит Dockerfile и *.jar (путь к джарнику указан в докерфайле):
        docker build -t timer-app --network host.  (на стенде: --network host)

        Рабочий вариант:
        docker build -t timer-app --network host .

    3. Запустить контейнер
        docker run -dp 8080:8080 -v /var/www/html/photos/:/var/www/html/photos timer-app (на стенде: --add-host timer_db:172.18.0.2)

        Не выбрасывал исключение:
        docker run -dp 8080:8080 -v /var/www/html/photos/:/var/www/html/photos --net="host" timer-app

    4. Открыть tcp/8080

    5. Проверить подключение через браузер.
    (org.postgresql.util.PSQLException: Connection to localhost:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections.)

    6. если попытка неудачна:
        - удалить образ и создать заново с парамтером --network host
        - запустить контейнер с --add-host)