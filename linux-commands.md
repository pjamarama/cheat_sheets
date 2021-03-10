# Полезные командочки

<h2>Занят ли порт?</h2>
sudo lsof -i -P -n | grep LISTEN<br>
sudo netstat -tulpn | grep LISTEN<br>
sudo lsof -i:22 ## see a specific port such as 22 ##<br>
top -p `pgrep -d "," postgres` // поиск по имени процесса. Если не найден, будет ошибка "требуется аргумент"

<h2>Занят ли порт на удаленной машине?</h2>
nc -zvw10 172.18.0.3 8080

<h2>Объем определенной директории</h2>
du -sh directoryName
