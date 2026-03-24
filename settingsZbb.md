# Руководство по настройке Zabbix
## Общая информация

IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Установка и настройка агентов (2SRV)
1.  Установите репозиторий Zabbix
-  wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian12_all.deb
-  dpkg -i zabbix-release_latest_7.4+debian12_all.deb
-  apt update
2.  Установите Zabbix агент
-  apt install zabbix-agent
3.  Запустите процесс Zabbix агента
-  systemctl restart zabbix-agent
-  systemctl enable zabbix-agent
