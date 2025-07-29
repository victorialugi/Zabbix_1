# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Лугинина Виктория`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

```
sudo -s
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

Edit file /etc/zabbix/zabbix_server.conf
DBPassword=password

systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```


![task_1.png](https://github.com/victorialugi/Zabbix_1/blob/main/task_1.png)


---

### Задание 2

```
## Commands Used

# On zabbix-vm (Zabbix Server)
sudo -s
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
wget https://www.zabbix.com/downloads/7.4.0/zabbix-7.4.0.tar.gz
tar -xzf zabbix-7.4.0.tar.gz
cd zabbix-7.4.0/database/postgresql
scp -i /home/victoria_luginina/.ssh/id_rsa /home/victoria_luginina/Downloads/zabbix-7.4.0.tar.gz ubuntu@84.201.178.123
sudo -u zabbix psql -d zabbix -f schema.sql
sudo -u postgres psql
ALTER USER zabbix WITH PASSWORD ******;
\q
nano /etc/zabbix/zabbix_server.conf
# DBName=zabbix
# DBUser=zabbix
# DBPassword=*****
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
nano /etc/zabbix/zabbix_agentd.conf
# Server=84.201.178.123
# ServerActive=84.201.178.123
# Hostname=zabbix-vm
systemctl restart zabbix-agent

# On zabbix-agent-vm
ssh -i /home/victoria_luginina/.ssh/id_rsa ubuntu@89.169.190.116
sudo apt-get update && sudo apt-get upgrade -y
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo apt-get update
sudo apt-get install -y zabbix-agent
nano /etc/zabbix/zabbix_agentd.conf
# Server=84.201.178.123
# ServerActive=84.201.178.123
# Hostname=zabbix-agent-vm
sudo systemctl restart zabbix-agent
```

![configuration.png](https://github.com/victorialugi/Zabbix_1/blob/main/configuration.png)

![monitoring.png](https://github.com/victorialugi/Zabbix_1/blob/main/monitoring.png)

![screenshot_log_1.png](https://github.com/victorialugi/Zabbix_1/blob/main/screenshot_log_1.png)

![screenshot_log_2.png](https://github.com/victorialugi/Zabbix_1/blob/main/screenshot_log_2.png)
