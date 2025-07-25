# Домашнее задание к занятию "Система мониторинга Zabbix" - Хамуро Илья

### Задание 1

Установите Zabbix Server с веб-интерфейсом.\
Команды:\
$ sudo -s \
$ wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb\
$ dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb\
$ apt update\
$ apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent\
$ sudo -u postgres createuser --pwprompt zabbix\
$ sudo -u postgres createdb -O zabbix zabbix\
$ zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix\
$ systemctl restart zabbix-server zabbix-agent apache2\
$ systemctl enable zabbix-server zabbix-agent apache2\

![Снимок экрана 2025-06-08 155100](https://github.com/user-attachments/assets/6fa7f33c-c100-4c9d-a88e-830a30450ff6)
![Снимок экрана 2025-06-08 160706](https://github.com/user-attachments/assets/d0ed0153-dd4f-455b-9f5e-3cc8931e285a)


### Задание 2

Установите Zabbix Agent на два хоста.\
Команды\
$ sudo -s
$ wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
$ dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
$ apt update
$ apt install zabbix-agent
$ systemctl restart zabbix-agent
$ systemctl enable zabbix-agent
![Снимок экрана 2025-06-08 173312](https://github.com/user-attachments/assets/52b201d8-6837-49ad-9cb0-3fd48fa1527e)

![Снимок экрана 2025-06-08 173716](https://github.com/user-attachments/assets/d002ad77-cd2c-445f-9c8d-67c797a679c5)

![Снимок экрана 2025-06-08 173820](https://github.com/user-attachments/assets/b231d319-7e0b-4038-9553-d7225462cb4f)

![Снимок экрана 2025-06-08 173905](https://github.com/user-attachments/assets/35d0a06f-3871-496e-ab6d-630930dd0702)

