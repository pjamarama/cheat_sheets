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

<h2>Коды для смены прав файла</h2>
The sums of these numbers give combinations of these permissions:<br>
0 = no permissions whatsoever; this person cannot read, write, or execute the file.<br>
1 = execute only.<br>
2 = write only.<br>
3 = write and execute (1+2)<br>
4 = read only<br>
5 = read and execute (4+1)<br>
6 = read and write (4+2)<br>
7 = read and write and execute (4+2+1)<br>

<h2>Поиск и замена в nano</h2>
Press Ctrl + \<br>
Enter your search string and hit return.<br>
Enter your replacement string and hit return.<br>
Press A to replace all instances.<br>
