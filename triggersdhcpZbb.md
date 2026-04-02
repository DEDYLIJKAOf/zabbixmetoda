# Триггеры для DHCP
Общая информация IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Предварительная настройка
Для начала нам нужен UserParameter для сбора статистики DHCP

Сначала заходим в /etc/zabbix/zabbix_agentd.d/dhcp.conf (Если нету такого файла, то создаем)

<img width="845" height="619" alt="image" src="https://github.com/user-attachments/assets/1ff899ed-49a0-4232-bde0-8e81693268e1" />

```
# Проверка статуса DHCP конфигурации
UserParameter=dhcp.config.status,sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf >/dev/null 2>&1; echo $?

# Сбор статистики пула (требует omapi или парсинг логов)
UserParameter=dhcp.pool.free,sudo /usr/local/bin/dhcp-stats.sh free
UserParameter=dhcp.pool.used,sudo /usr/local/bin/dhcp-stats.sh used
UserParameter=dhcp.pool.total,sudo /usr/local/bin/dhcp-stats.sh total

# Сбор статистики аренд из файла
UserParameter=dhcp.leases.active,sudo grep "^lease" /var/lib/dhcp/dhcpd.leases | wc -l
UserParameter=dhcp.leases.expiring_1h,sudo /usr/local/bin/dhcp-expiring.sh 3600

# Сбор метрик из логов за последние 5 минут
UserParameter=dhcp.discovers,sudo grep "DHCPDISCOVER" /var/log/syslog | tail -300 | wc -l
UserParameter=dhcp.offers,sudo grep "DHCPOFFER" /var/log/syslog | tail -300 | wc -l
UserParameter=dhcp.declines,sudo grep "DHCPDECLINE" /var/log/syslog | tail -300 | wc -l

# Время ответа DHCP (среднее)
UserParameter=dhcp.response.time,sudo /usr/local/bin/dhcp-response-time.sh
```
Далее напишем скрипт для парсинга данных из логов

Создайте файл /usr/local/bin/dhcpstat.sh и напишите в нем этот конфиг

<img width="818" height="621" alt="image" src="https://github.com/user-attachments/assets/46c36baf-2dc8-4a9a-a65c-690b36f8d28f" />

```
bash
#!/bin/bash
# Простой скрипт для парсинга статистики из лога DHCP
# Требует включения statistics в dhcpd.conf

case $1 in
    free)
        sudo grep "address range" /var/log/dhcpd-stats.log | awk '{sum+=$NF} END {print sum}'
        ;;
    used)
        sudo grep "address range" /var/log/dhcpd-stats.log | awk '{sum+=$(NF-2)} END {print sum}'
        ;;
    total)
        sudo grep "address range" /var/log/dhcpd-stats.log | awk '{sum+=$(NF-1)} END {print sum}'
        ;;
esac
```

Сделайте скрипт исполняемым:

chmod +x /usr/local/bin/dhcpstat.sh

## Триггер сервис недоступен

Для начала нужно создать item

<img width="1045" height="701" alt="image" src="https://github.com/user-attachments/assets/b041f140-2c8c-4696-b0ac-9031158ca622" />

<img width="1047" height="693" alt="image" src="https://github.com/user-attachments/assets/3d41115b-da56-4cef-bc6c-61a9fd2d4d8d" />

Теперь можно создать триггер

<img width="1046" height="707" alt="image" src="https://github.com/user-attachments/assets/1b83be54-df81-4110-9155-beec04225491" />

## Триггер Пул адресов исчерпан

Item:

<img width="1048" height="708" alt="image" src="https://github.com/user-attachments/assets/fb4938f1-b21b-4d08-a497-0aca7aea01ec" />

trigger:

<img width="1046" height="695" alt="image" src="https://github.com/user-attachments/assets/4ba4ef09-f61c-4c6f-ad71-3e4cc5b01ea4" />

