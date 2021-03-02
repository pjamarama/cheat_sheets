<h2>Полезные командочки</h2>
sudo lsof -i -P -n | grep LISTEN<br>
sudo netstat -tulpn | grep LISTEN<br>
sudo lsof -i:22 ## see a specific port such as 22 ##<br>
top -p `pgrep -d "," postgres` // поиск по имени процесса. Если не найден, будет ошибка "требуется аргумент"
