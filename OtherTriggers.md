# Другие триггеры (В разработке)
Общая информация IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Триггеры для контроля попыток входа в систему

Макросы

<img width="1557" height="516" alt="image" src="https://github.com/user-attachments/assets/5022f9b4-9e54-4eb1-949a-72f7df9b7dce" />


Список items который нужно добавить для триггеров

```
                    Key	                                         Описание	                     Тип
log[/var/log/auth.log,"Failed password",300s]	     Подсчет неудачных паролей за 5 минут	Zabbix agent (log)

log[/var/log/auth.log,"Invalid user",300s]	        Подсчет несуществующих пользователей	Zabbix agent (log)
 
log[/var/log/auth.log,"Accepted password",300s]	    Подсчет успешных входов	              Zabbix agent (log)

log[/var/log/auth.log,"sudo.*FAILED",300s]	        Подсчет неудачных sudo	              Zabbix agent (log)

log[/var/log/auth.log,"Connection closed by",600s]	Подсчет обрывов соединений	          Zabbix agent (log)

log[/var/log/auth.log,"from",300s]	                Общая активность по IP	              Zabbix agent (log)

```
<img width="1506" height="265" alt="image" src="https://github.com/user-attachments/assets/82238c5d-dedf-498b-b673-d03fc0b47038" />

### 1. Множество неудачных попыток входа (возможная атака)

```
Поле          Значение
Name          Multiple failed login attempts on {HOST.NAME}
Expression    last(/Linux by Zabbix agent/log[/var/log/auth.log,"Failed password",300s]) > {$FAILED_LOGIN_THRESHOLD:"10"}
Severity      High
Tags	security : failed_login
```

<img width="1053" height="694" alt="image" src="https://github.com/user-attachments/assets/ee2904c8-a13c-4571-95db-1d1aa418e6c2" />

### 2. Множество неудачных попыток sudo

```
Поле          Значение
Name          Multiple failed sudo attempts on {HOST.NAME}
Expression    last(/Linux by Zabbix agent/log[/var/log/auth.log,"sudo.*FAILED",300s]) > {$FAILED_SUDO_THRESHOLD:"5"}
Severity       High
Tags	security : failed_sudo
```

<img width="1042" height="695" alt="image" src="https://github.com/user-attachments/assets/128bb005-c3c0-4126-adac-c905b32beec6" />

### 3. Неудачная попытка входа от root

```
Поле        Значение
Name        Failed root login attempt on {HOST.NAME}
Expression	last(/Linux by Zabbix agent/log[/var/log/auth.log,"Failed password for root",60s]) > 0
Severity	  High
Tags	security : root_login
```

<img width="1048" height="695" alt="image" src="https://github.com/user-attachments/assets/c02fabe9-f4cf-4530-ae2e-a20ec1db3d91" />

