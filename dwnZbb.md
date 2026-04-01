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

  Types: deb

  URIs: https://download.docker.com/linux/debian

  
  Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")

  
  Components: stable

  Signed-By: /etc/apt/keyrings/docker.asc
  
  EOF
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

  Теперь нужно заполнить .yaml файл, примерный конфиг показан на рисунках ниже

https://github.com/DEDYLIJKAOf/remotedocker/tree/main

-  nano .env (В той же директории создаете файл). После заполения файла нужно прописать "docker compose up -d" чтобы запустить контейнеры

  После этого заходите на клиента, либо на любую машину с графическим интерфейсом и в строке браузера вводите ip сервера, где находится Zabbix и через двоеточие вводите порт (Пример: 192.168.10.2:8080). После всех действий вас перекидывает в интерфейс Zabbix

  <img width="1291" height="779" alt="image" src="https://github.com/user-attachments/assets/6751eda7-a51b-44e1-8424-927744ee55e5" />

  Стандартный пароль для входа: Admin/zabbix

  <img width="1680" height="899" alt="image" src="https://github.com/user-attachments/assets/84313e28-9b17-49f5-b7b6-48c318a35a79" />
  
  ## Примечания 
  Если у вас перестала работать вебка, нужно зайти в директорию и прописать команду "docker compose restart", это перезагрузит ваши контейнеры, если ошибка не ушла, то стоит обратиться к логам, иногда контейнеры запускаются не сразу
  
  Если по каким-то причина docker не хочет пулить, то уберите флаг -d, тогда весь процесс запуска контейнеров должен выводиться. Обычно ошибка в самих .yaml файлах. лишние пробелы, не те версии и тому подобное
