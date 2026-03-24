# Руководство по возможностям Zabbix
Общая информация IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Настройка оповещений через Email

Заходим в Alerts --> Media types, после нажимаем на Email и заполняем там свои данные 

<img width="1686" height="870" alt="image" src="https://github.com/user-attachments/assets/3baebb6c-a526-4a5a-b6f4-9c65f6349660" />

Теперь мы должны зайти в Users --> Users, нажать на нужного вам пользователя, потом Media, нажать добавить и занести данные, в поле (Send to) вводим на какую почту отправлять, так как мне лень регестрировать новую почту я отправлю на туже самую

<img width="1699" height="866" alt="image" src="https://github.com/user-attachments/assets/ab366114-1c46-4068-8982-4a433bd7df7c" />

Нужно зайти в Alerts --> Actions --> Trigger actions,тут нам нужно включить оповещение о проблемах

<img width="1503" height="191" alt="image" src="https://github.com/user-attachments/assets/2bfcca58-d47b-41af-ad4d-65c0251437a4" />


