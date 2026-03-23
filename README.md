# Методичка по устновки и настройке Zabbix в контейнере docker
Общая информация
IP:
-  (1SRV) - 192.168.10.2 [DEBIAN 12]
-  (2SRV) - 192.168.10.1 [DEBIAN 12]
-  (CLI) - 192.168.10.3 [DEBIAN 12]
-  (CLI2) - 192.168.10.4 [DEBIAN 12]
-  (CLI3) - 192.168.10.5 [DEBIAN 12]
# (1SRV)
## Установка docker
### Обновите зависимости 
-  apt update & apt upgrade
### Установите необходимые зависимости:
-  sudo apt install ca-certificates curl gnupg lsb-release
### Добавьте официальный GPG-ключ Docker:
-  sudo install -m 0755 -d /etc/apt/keyrings
-  sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
-  sudo chmod a+r /etc/apt/keyrings/docker.asc
### Добавьте репозиторий в Apt sources:
-  sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
-  Types: deb
-  URIs: https://download.docker.com/linux/debian
-  Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
-  Components: stable
-  Signed-By: /etc/apt/keyrings/docker.asc
-  EOF
-  apt update
### Установка и проверка пакетов Docker.
-  sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
-  sudo systemctl status docker
-  sudo systemctl start docker
-  sudo docker run hello-world
  <img width="759" height="642" alt="image" src="https://github.com/user-attachments/assets/5c6a7212-2bac-48f1-97d5-8754b181377a" />

Если все сделано правильно, то должно появиться такое сообщение
## Установка zabbix
### Создаем раздел, в котором будет наш zabbix
-  mkdir /apps/
-  mkdir /apps/zabbix
-  cd /apps/zabbix
-  nano docker-compose.yaml

  <img width="751" height="596" alt="image" src="https://github.com/user-attachments/assets/8caa7224-2a17-420c-bfb5-f5fdfd0a096c" />

  <img width="804" height="624" alt="image" src="https://github.com/user-attachments/assets/3729e506-4c76-4423-a1aa-1dbbf7ad180b" />

-  nano .dev (В той же директории)

  <img width="777" height="634" alt="image" src="https://github.com/user-attachments/assets/65d9b58f-7c31-49cc-b14a-376fc95bb973" />
