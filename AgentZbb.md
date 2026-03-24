# Руководство по настройке Zabbix agent
## Общая информация

IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

# Агент zabbix

## Установка агента (2SRV)
1.  Установите репозиторий Zabbix
-  wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian12_all.deb
-  dpkg -i zabbix-release_latest_7.4+debian12_all.deb
-  apt update
2.  Установите Zabbix агент
-  apt install zabbix-agent
3.  Запустите процесс Zabbix агента
-  systemctl restart zabbix-agent
-  systemctl enable zabbix-agent
## Настройка агента (2SRV)
Для начало нужно зайти в конфиг агента
- nano /etc/zabbix/zabbix_agentd.conf

Нужно отредактировать 3 записи
1.  Server = 192.168.10.2 (Указываем сервер с zabbix)
2.  ServerActive = 102.168.10.2
3.  Hostname = SRV2 (Указываем имя, которое будет отображаться в web интерфейсе)

## Добавление устройства как узел сети в web
Вводим данные устройства и агента

<img width="1691" height="863" alt="image" src="https://github.com/user-attachments/assets/03c32143-2c47-4136-a9f3-6c5f89016e00" />

Если все было введено правильно, то значок zbx загорится зеленым, это значит агент подключен

<img width="1484" height="29" alt="image" src="https://github.com/user-attachments/assets/fbb13742-06d0-46cd-b830-f1daef23bf9a" />


