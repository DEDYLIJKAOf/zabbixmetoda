# Руководство по возможностям Zabbix
Общая информация IP:

(1SRV) - 192.168.10.2 [DEBIAN 12]

(2SRV) - 192.168.10.1 [DEBIAN 12]

(CLI) - 192.168.10.3 [DEBIAN 12]

(CLI2) - 192.168.10.4 [DEBIAN 12]

(CLI3) - 192.168.10.5 [DEBIAN 12]

## Мониторинг простой веб-страницы

Мы будем мониторить веб-страницу: poo.tomedu.ru

Проследуйте в Templates потом Create template, заполните такие значения:

   <img width="936" height="384" alt="image" src="https://github.com/user-attachments/assets/426e17fd-2648-4239-a7be-97ae9541ec73" />

Ищем этот же Templates, жмем на web, потом на create web scenario, далее заполняем

   <img width="1682" height="869" alt="image" src="https://github.com/user-attachments/assets/79add00d-4794-48be-889f-e7933a0b54c7" />

Переходим в steps, нажимаем add на steps и заполняем

   <img width="1675" height="854" alt="image" src="https://github.com/user-attachments/assets/9dc42184-e845-44e8-9a85-82e2ac81cee3" />

После заполнения всех параметров жмем Add, чтобы добавить шаг и далее еще раз Add, чтобы добавить сам сценарий проверки. Должна получиться вот такая картина.

   <img width="1535" height="395" alt="image" src="https://github.com/user-attachments/assets/cdfee86a-810a-4a0b-9427-47619f5f1454" />

   
Простейшая проверка доступности сайта сделана. Дальше нам надо прикрепить этот шаблон к какому-нибудь хосту, чтобы начались реальные проверки. Я прикреплю шаблон к SRV2. Для этого идем в Data collection -> Hosts, выбираем Zabbix Server и прикрепляем к нему созданный ранее шаблон.

<img width="929" height="599" alt="image" src="https://github.com/user-attachments/assets/37b83ece-f7d1-4955-a94e-aee96487a659" />

Вот и готово, теперь можно зайти в Monitoring --> Hosts --> SRV2 --> WEB --> poo.tomedu.ru. Тут можно будет увидеть данные графики 

<img width="1490" height="569" alt="image" src="https://github.com/user-attachments/assets/3b6281cc-35f3-4a2b-96ec-c0ad9e408a6b" />
