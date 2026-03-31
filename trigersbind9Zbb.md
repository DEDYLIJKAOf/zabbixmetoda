# Создание тригеров для bind9
Общая информация

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Триггер на доступность bind9
### Шаг 1: Включение статистики в BIND9
Для того чтобы Zabbix мог собирать данные, необходимо включить встроенный HTTP-сервер статистики BIND.

Откройте файл конфигурации BIND (/etc/named.conf или /etc/bind/named.conf) и добавьте следующий блок :

<img width="565" height="80" alt="image" src="https://github.com/user-attachments/assets/becd8f8a-0c6e-48db-aea9-09412bc94376" />

statistics-channels {
    inet 127.0.0.1 port 8053 allow { 192.168.10.2; };
};

-  inet 127.0.0.1 — сервер будет слушать только локальный интерфейс (рекомендуется с точки зрения безопасности).

-  port 8053 — порт, на котором будет доступна статистика.

-  allow { 192.168.10.2; }; — разрешаем доступ только с этого IP (для Zabbix Agent, который работает локально). Если Zabbix Server опрашивает хост удаленно, укажите его IP здесь или через запятую.

  Далее нужно проверить конфигурацию конфигурацию и перезапустить BIND:

-  named-checkconf
-  systemctl restart named

После это мы можем убедиться в том, что статистика отдается:
-  curl http://127.0.0.1:8053/

### Шаг 2: Настройка агента Zabbix
На сервере, где работает BIND, необходимо настроить Zabbix Agent (версии 6.0, 7.0+), чтобы он мог парсить XML и передавать данные серверу.

Агенту понадобятся curl для получения данных и утилита для парсинга XML (xmllint или xmlstarlet) .

-  apt-get install -y curl libxml2-utils

Далее нужно создать скрипт, который будет извлекать нужные значения из XML.

Создайте файл /usr/local/bin/bindstat.sh и напишите в нем этот конфиг

<img width="760" height="580" alt="image" src="https://github.com/user-attachments/assets/15c669ca-77c5-46fa-889c-54302db2c188" />

Config

```
#!/bin/bash
set -euo pipefail

URL="http://127.0.0.1:8053/"
WHAT="${1:-queries-in}"
QTYPE="${2:-A}"

DATA=$(curl -fsS "$URL")

# Функция извлечения данных через xmllint
extract_rdtype() {
  local dir="$1" q="$2"
  if command -v xmllint >/dev/null; then
    xmllint --xpath "string(//${dir}/rdtype[@name='${q}'])" - 2>/dev/null || echo 0
  else
    echo 0
  fi
}

case "$WHAT" in
  queries-in)
    extract_rdtype "queries-in" "$QTYPE"
    ;;
  queries-out)
    extract_rdtype "queries-out" "$QTYPE"
    ;;
  *)
    echo 0
    ;;
esac
```

Сделайте скрипт исполняемым:

-  chmod +x /usr/local/bin/bindstat.sh

Добавление UserParameter:

Определите ключи, которые Zabbix Server будет запрашивать у агента.

Создайте файл конфигурации агента /etc/zabbix/zabbix_agentd.d/bind.conf

<img width="820" height="99" alt="image" src="https://github.com/user-attachments/assets/e26a9244-49aa-4266-abac-8b4196b2931c" />


-  UserParameter=bind.queries.in[*],/usr/local/bin/bindstat.sh queries-in "$1"
-  UserParameter=bind.queries.out[*],/usr/local/bin/bindstat.sh queries-out "$1"

Перезапустите агента Zabbix:

-  systemctl restart zabbix-agent
### Шаг 3: Hастройка шаблона в Zabbix
Перейдите в Data Collection → Templates, создайте новый шаблон (например, "Custom Bind9").

Создайте Item (Элемент данных):

<img width="1048" height="649" alt="image" src="https://github.com/user-attachments/assets/a05f5be3-6abc-4c06-8f40-046f701e8b95" />

Cоздание триггеров

<img width="1047" height="642" alt="image" src="https://github.com/user-attachments/assets/098f54d9-1ff5-4e38-a4a2-d09e1cfebb8c" />

### Проверка
Отключаем bind9 и смотрим

<img width="990" height="109" alt="image" src="https://github.com/user-attachments/assets/4f44e0ee-c7d1-43ba-a39a-5b0ed0a9cc36" />

Высветилась наша проблема
