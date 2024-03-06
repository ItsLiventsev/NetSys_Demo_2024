## Стенд ПК № 8 (09.02.06 Сетевое и системное администрирование) 2023-2024 год

Базовый стенд для  09.02.06 Сетеве и системное администрирование представлен по сссылке - https://disk.yandex.ru/d/wVsAJMZ-fcZi3w (Стенд для добавления в VMware Player, вложенная виртуализация через ESXi).

# Топология сети

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/af62755f-eb59-4efa-91f2-da89b200ccb7)

# Таблица сети (ожидает доработки и проверки на стенде) (Вариант 1)

| Имя устройства | Интерфейсы |  IPv4/IPv6    | Маска/Префикс |   Шлюз         |
| :------------: | :--------: | :----------:  | :----------:  | :----------:   |
|                | ens192     | 10.12.25.10   | /24           | 10.12.13.254   |
| ISP            | ens224     | 192.168.0.170 | /30           |                |
|                | ens256     | 192.168.0.162 | /30           |                |
|                | ens161     | 1.1.1.3       | /30           |                |
| HQ-R           | ens192     | 192.168.0.2   | /25           |                |
|                | ens224     | 192.168.0.169 | /30           | 192.168.0.164  | 
| BR-R           | ens192     | 192.168.0.130 | /27           |                |
|                | ens224     | 192.168.0.161 | /30           | 192.168.0.164  |
| HQ-SRV         | ens192     | 192.168.0.1   | /25           | 192.168.0.2    |
| BR-SRV         | ens192     | 192.168.0.129 | /27           | 192.168.0.130  |
| CLI            | ens192     | 1.1.1.2       | /30           | 1.1.1.3        |

# Таблица сети (ожидает доработки и проверки на стенде) (Вариант 2)

Таблица сетей

![address_lan](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ae42aeb9-bbb7-497d-9c5c-911936b41289)

Таблица для виртуальных машин

![address_vm](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e826d80a-7460-46e0-aacb-6a50bbeae295)

#### Базовая работа с сетями (выход в глобальную сеть, проброс DHCP)

Обратите внимание, что для настройки, необходимо работать с сетью VM Network (выходит на физический адаптер по NAT).

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e892be81-5bd6-4847-b4d9-47550cf2a223)

#### Проброс виртуальной машины в сеть VM Network

Открываем настройки виртуальной машины, выбираем сеть VM Network

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c784d598-1526-44d4-b142-0bd7386829c7)

#### Установка программ в соответствии с заданием для каждой виртулаьной машины:

| Имя устройства |  Программы  |  
| :------------: | :---------: | 
| ISP            | firewalld   | 
|                | frr         | 
|                | iperf3      | 
|                |urbackup-server|
|                |             | 
| HQ-R           | firewalld   | 
|                | frr         |
|                | dhcp-server |
|                | iperf3      |
|                | chrony      |
|                |             |
| BR-R           | firewalld   | 
|                | frr         | 
|                |          | 
| HQ-SRV         | openssh-server      |
|                | bind      |
|                | task-samba-dc admc|
|                | mdadm            |
|                | nfs-server            |
|                | nfs-clients            |
|                | nfs-utils            |
|                |             |
| BR-SRV         | task-auth-ad-sssd       | 
| CLI            | task-auth-ad-sssd       |

# Задание 1 модуля 1

## 1. Выполните базовую настройку всех устройств:
a. Присвоить имена в соответствии с топологией
b. Рассчитайте IP-адресацию IPv4 и IPv6. 
Необходимо заполнить таблицу №1, чтобы 
эксперты могли проверить ваше рабочее место.
c. Пул адресов для сети офиса BRANCH - не более 16
d. Пул адресов для сети офиса HQ - не более 64

Для работы с сетевыми адаптерами через текстовый редактор используются следующие файлы:

* options - выбор dhcp или static
* ipv4address - адрес
* ipv4route - маршрут, шлюз
* /etc/resolv.conf - DNS-сервер

### Настройка статического подключения

Настройка файла options

mcedit /etc/net/ifaces/ens192/options

В файле проверем, чтобы значения стояли на статику (static)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/66503d13-82bd-411d-bff0-5fba1348fe44)

### Настройка IP-адреса

mcedit /etc/net/ifaces/ens192/ipv4address

Обратите внимание, что мы создаем новые файлы, изначально в них пусто

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/d7713a50-4595-4815-bb7c-5ebd871edbb2)

Прописываем адрес в привычном формате и через "/" прописываем маску - пример для HQ-SRV - 192.168.0.1/25

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f03d5719-392a-4c9e-9806-d7930f3696b4)

Нажимаем на F10 И сохраняем изменения в файле.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c10079d2-cc90-415e-bd2b-22bbc48b3b21)

### Настройка шлюза

mcedit /etc/net/ifaces/ens192/ipv4route

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/d51608a7-5593-465a-a581-ac956c2af999)

Обратите внимание, что мы создаем новые файлы, изначально в них пусто

Прописываем адрес в привычном формате - пример для HQ-SRV - 192.168.0.1

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ab2b6cb5-ea04-4c4d-a725-2b14f8ae4575)

### Настройка IP-адреса DNS

mcedit /etc/net/ifaces/ens192/resolv.conf

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/bac49099-2a90-4e0a-9ca7-59bb6f72f7a4)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/a5a3fdb5-ee0e-48f5-959f-da2bf58a04e5)

Через команду ip route проверяем, что шлюз поставлен

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/9df072a1-4c3f-490a-b1db-bdc0ba3f25e8)

### Маршрутизация транзитных пакетов на роутерах

На устройствах ISP, HQ-R, BR-R необходимо включить пересылку пакетов между интерфейсами - forwarding. Чтобы включить пересылку пакетов между интерфейсами, необходимо отредактировать файл sysctl.conf.

mcedit /etc/sysctl.conf и добавить в файл следующие значения:

net.ipv4.ip_forward=1

net.ipv6.conf.all.forwarding=1

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f274ab70-6db8-4ed3-ad0d-db41129a715c)

После необходимо применить изменения - sysctl -p (или перезагрузиться).

--------------------------

### Работа с NAT, открытие портов для передачи пакетов (если не уверены, что корректно работает, то выключаем firewalld и работаем дальше по заданию (systemctl stop firewalld)

Пример зон для Firewalld

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/98cc2863-4de2-4495-bfbe-79b64b0129e7)

#### Настройка на HQ-R

Перемещение сетевых адаптеров в зоны и включение NAT

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/58ee1bb7-e8f8-4cfa-af10-18210aa53c7f)

Правило на перессылку пакетов

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/155517b2-9d32-4829-9077-13dd34132ae7)

Разрешаем порт 89 для работы между firewalld (порт OSPF)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/2e10f6c5-b957-45b0-b624-976478cf99de)

Перезагрузка Firewalld для применения команд

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/64853438-fcf0-4164-80ac-dbdef6866fe2)

Проверяем применение команд через команду просмотра

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/b3e3f8ea-177f-4f76-900c-ba84607fd966)


#### Настройка на ISP

Перемещение сетевых адаптеров в зоны и включение NAT

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/02d96313-f14a-4ac0-8bfd-673f8d5e005a)

Правило на перессылку пакетов

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/dc2dbc92-86e1-440b-bb58-4cefb558605a)

Разрешаем порт 89 для работы между firewalld (порт OSPF)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e6433989-a29c-46f1-a816-14489294faa4)

Перезагрузка Firewalld для применения команд

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/64853438-fcf0-4164-80ac-dbdef6866fe2)

#### Настройка на BR-R

Перемещение сетевых адаптеров в зоны и включение NAT

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/4b24b074-b4f4-4fb1-82f9-8aade560e29a)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/d075a3e9-4166-4f9f-af81-60e0bbea8311)

Правило на перессылку пакетов

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/0dc6e12a-3838-487b-80b3-cfc9193ee751)

Разрешаем порт 89 для работы между firewalld (порт OSPF)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/9e2316ee-93ac-4048-9295-6d25bd8a7584)

Перезагрузка Firewalld для применения команд

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/df934a57-17bb-4dec-997f-77626c5090ea)

Проверить можно через команду firewall-cmd —list-all -zone="Зона"

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/de75daf2-3f53-4238-bbb1-cf6d6c5ac3f6)

--------------------------

### 2. Настройте внутреннюю динамическую маршрутизацию по  средствам FRR (требует доработки, возможно проблема в пакете на стенде)
Выберите и обоснуйте выбор протокола динамической маршрутизации из расчёта, что в дальнейшем сеть будет масштабироваться.

Установка пакета frr:

apt-get install frr

Настройка утилиты frr:

Для настройки OSPF необходимо включить соответствующий демон в конфигурации /etc/frr/daemons

mcedit /etc/frr/daemons

В конфигурационном файле /etc/frr/daemons необходимо активировать выбранный протокол для дальнейшей реализации его настройки

eigrpd = yes - для EIGRP (IPv4)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/95be0aff-5457-46a2-8393-4d7b10a1c5d1)

Включаем и добавляем в автозагрузку службу frr:

systemctl enable --now frr

Открываем утилиту frr для настройки маршрутизации:

vtysh

#### Настройки EIGRP на HQ-R

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/b6eabe1f-193e-4a05-85c2-9752eecee1c5)

conf t

router eigrp 1

network 1.1.1.0/30

network 172.16.100.0/26

do wr

exit

#### Настройки EIGRP на BR-R

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/76413a4e-a2a7-42a8-84e6-e9f433b273c2)

conf t

router eigrp 1

network 2.2.2.0/30

network 192.168.100.0/28

do wr

exit

#### Настройки EIGRP на ISP

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/05f55bf5-3217-4fd6-860b-7b826cc47686)

conf t

router eigrp 1

network 1.1.1.0/30

network 2.2.2.0/30

network 3.3.3.0/30

do wr

exit

##### Помните, что базы обновляются через промежуток времени

#### a. Составьте топологию сети L3

Пример схемы

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f0107ffa-c5bd-45bb-9846-2e1aea9cd3ff)

Создать схему можно в https://www.drawio.com/ ,там есть иконки сетевых устройств можно подгрузить

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/cef64cb5-46d8-40cc-8990-251f3b963f77)

----------------------
### 3. Настройте автоматическое распределение IP-адресов на роутере HQ-R
a. Учтите, что у сервера должен быть зарезервирован адрес.

#### Установка и настройка DHCP ДЛЯ IPv4

apt-get install dhcp-server

#### Добавление утилиты в атозагрузку

systemctl enable --now dhcpd

#### Проверяем, какой интерфейс смотрит в сторону локальной сети HQ, в которой должен работать DHCP сервер

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/20a195ad-6784-4395-b2f4-0af33b41225e)

#### Указываем интерфейс внутренней сети, где будет работать DHCP

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/6226e85e-d995-427b-884c-ae1b64c2f7e4)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/b9b774e8-d929-4da0-981c-c81d76e5181e)

Проверяем, что DHCP-сервер установился корректно. Видим, что в директории сервера есть файл "Sample"

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/3bc84b6a-ad23-458c-bb10-262d0146dd70)

Копируем файл с изменением имени (ставм имя dhcpd.conf) (копию будем использовать в работе)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/bc144f45-4a62-4add-b8d0-5eece90fae88)

Открываем файл и ставим настройки в зависимости от задания

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/7e4d1192-f54e-4a17-abca-1ff3eb9ad202)

#### a. Учтите, что у сервера должен быть зарезервирован адрес

Смотрим и/или копируем MAC-адрес сетевого адаптера из настроек виртуальной машины HQ-SRV

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/4d37b818-b16d-412e-b1c0-adabfab44ff2)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/125e9b72-743c-4714-b192-ecf36de0f8be)

Прописываем значение MAC-адреса в конфигурацию DHCP-сервера, создав подпункт host HQ-SRV. Фиксируем адрес, через команду fixed-address

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/767d74df-baa7-4d92-84e2-14113cdc7da7)

Добавляем сервис в автозагрузку

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/59ce5e03-06e3-4fc0-bab1-e9cbf8a302cd)

Запускаем сервис

systemctl start dhcpd

#### Проверяем на HQ-SRV, после перезагрузки демона сети systemctl restart network, через ip a, видим, что адрес выставился

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ffd2a725-a015-4619-a42b-91f37aadcabc)

------------------

## 4. Настройте локальные учётные записи на всех устройствах в соответствии с таблицей 2

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/8363c96f-d648-4c10-ba40-ac773ce2d8e3)

Пример ввода пользователя (остальные делаем по аналогии)

adduser admin

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/369506ed-324a-45b8-b17b-7669920a68bc)

Изменение пароля для пользователя

passwd admin

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/a0b8e1bf-043b-439e-b0df-6e0558e3e227)

Пользователи, котоыре должны быть на виртуальных машинах 

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/8ae9927d-7a64-4624-af13-ef07626cc85b)

#### Создание пользователя в графическом интерфейсе

Заходим в центр управления системой от root, идем во вкладку локальные учетные записи, создаем новую учетную запись, заходим в настройки новой учетной записи (admin), прописываем пароль и подтверждение пароля, затем нажимаем применить


![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/579ae49a-1ca2-40a7-b92d-7ad60bf8e202)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/2e6cd605-5d14-4b54-a072-0ccb88b74700)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/9d81b94e-6eef-402f-b0ad-adfb94dbcdd6)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ce29bb3b-ef9d-45ec-b927-94ae777ae564)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/5896071a-a2b4-4708-852e-efda628e1a2e)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/81400672-e4a6-4dab-9fa3-9f4f02125400)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ea21b767-a782-4280-8f69-af96b7b70a36)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/0edae2da-a4c6-48fd-a595-25470bd495d5)

------------------------

## 5. Измерьте пропускную способность сети между двумя узлами HQ-R-ISP по средствам утилиты iperf3. Предоставьте описание пропускной способности канала со скриншотами

Установка утилиты iperf3 на HQ-R и ISP

apt-get install iperf3

#### Отключаем firewalld, если работает - systemctl stop firewalld

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f46e7c53-a7e6-483d-8aea-058e989cecb1)

### Перевод работы iperf3 в режим "сервера" (делаем на ISP)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/7adada17-76e1-4999-be68-9dba42e7841f)

### Отправка пакетов iperf3 на ISP с клиента HQ-R

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e8ff09ac-a852-4922-a768-f13443ad093c)

После чего на клиенте произойдет обработка таблицы передачи данных

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/4800a64b-7627-4ad8-8c0c-bfb77162fa76)

Делаем скриншот ответа команды (таблицы) и сохраняем в "лист ответа".

## 6. Составьте backup скрипты для сохранения конфигурации сетевых устройств, а именно HQ-R BR-R. Продемонстрируйте их работу.

Для работы с Backup будем использовать утилиту UrBackup

#### ISP является сервером UrBackup

apt-get isntall urbackup-server

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/431e7b90-c960-48d0-b9af-18cfcc9c415b)

Добавляем в автозагрузку

systemctl enable --now urbackup-server

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/db0da1f7-27db-445c-95f0-1f507857d50b)

Создаем директорию для хранения копий на сервере

mkdir /mnt/backups

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c85cb48f-183b-484f-b5af-a6008d9c1fe3)

Даем права -R пользователю и утилете UrBackup на папку /mnt/backups

chown -R urbackup:urbackup /mnt/backups/

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/43073a26-2769-4b34-a818-8094434054ae)

Даем полные права на папку всем группам пользователей

chmod 777 /mnt/backups/ 

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/459a84ec-9320-4a70-a65b-496787911f71)

#### HQ-R и BR-R являются клиентом UrBackup

apt-get install urbackup-client

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/53a1f716-5634-4fff-9838-be4a10823c11)

Добавляем в автозагрузку

systemctl enable --now urbackup-client

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c8692819-86d7-42b8-87f2-c745f51d6a2e)

#### Настройка UrBackup из браузера на графическом клиенте CLI

Открываем браузер

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/1bef580f-2ac7-48c9-9262-bac250dfde86)

Заходим на IP-адрес ISP с портом UrBackup (55414)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/884ac672-4dc8-4c73-8216-c9f0d1efe1e0)

Видим в статусе работы ошибку

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/dc0f81c3-be0c-4523-9aa5-bf7e8e51ccf9)

Переходим в вкладку настройки

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/35f9596b-dc07-4342-a9f2-6b1b2fd45e64)

Изменяем путь для хранения бэкапов на /mnt/backups/

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/badd39bb-46e9-4b8f-a83f-e36cb0b68586)

Снизу нажимаем "Сохранить"

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/2aa2b55a-505d-4776-af2a-c1db38d84fb8)

Возвращаемся на вкладку "Статус", видим, что всё хорошо - сообщение с ошибкой пропало

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f5928da5-44da-4604-922a-855193a3c64e)

#### Создаем скрипты для сохранения конфигурации сетевых устройств (файлы frr)

Заходим в настройки, далее "Файловые бэкапы", пролистываем вниз до "Катологи по умолчанию для бэкаба" - прописываем путь до папки frr

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/02ffb9aa-c83e-4878-8c70-d1a82a3168a0)

Нажимаем "Сохранить".

#### Прирудительно создаем первый бэкап

Открываем меню у имени компьютера

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/fc98a57b-54a4-4ed5-b378-6037d252ffce)

Нажимаем на функцию "Полный файловый бэкап"

Во вкладке "В работе", можем посмотреть выполнение задания

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/0e132a51-1abc-494c-8ab4-9ca8e461550c)

После несколких минут, бэкап будет успешно создан.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/3a5170a7-667d-4d9f-9a45-c9426432b14a)

# 7. Настройте подключение по SSH для удалённого конфигурирования устройства HQ-SRV по порту 2222. Учтите, что вам необходимо перенаправить трафик на этот порт по средствам контролирования трафика.

Устанавливаем на HQ-SRV утилиту openssh-server

apt-get install openssh-server

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/67eb8919-7519-42d4-a2db-a9f2dac17254)

Добавляем openssh-server в автозагрузку

systemctl enable --now sshd

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/61d20066-f65c-4e4e-85c8-5d88d1c1ac68)

#### Конфигурация openssh-server

Файл конфигурации - sshd_config

mcedit /etc/openssh/sshd_config

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/6a271f03-d2b2-4518-9cfd-b4b9f0445df8)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/8e30daf4-11cd-4c72-ab7c-45d58cbcacbc)

В соотвествии с заданием необходимо ssh-серверу использовать порт 2222, для проверки работоспособности самого соединения можно оставить 22, если есть проблемы с firewall, но затем поменять по заданию, на 2222

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/0ecfe865-1362-45d8-b50b-a4b482d2a288)

Перезагружаем сервис для применения настроек

systemctl restart sshd.service

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e25632b2-7f7b-41aa-93f0-b38b53abe149)

#### Подключение производится через клиента (утилита ssh)

ssh имяпользователя@172.16.100.2 -p 2222

-p - изменение стандартного для подключения порта

# 8. Настройте контроль доступа до HQ-SRV по SSH со всех устройств, кроме CLI.

P.S. В задании не указан конкретный пользователь, который должен заходить через SSH на HQ-SRV со всех машин кроме CLI, т.е. мы можем завести НОВОГО пользователя, либо использовать пользователей из задания № 4 (где таблица пользоваталей).

Для работы контроля доступа, необходимо в файле sshd_config добавить данные в параметр AllowUsers

Изначально в файле конфигурации параметра нет, его необходимость прописать с нуля.

mcedit /etc/openssh/sshd_config

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/6cbd9ebc-c587-45d0-ab40-1b13cf193ea0)

Пример команды, если DNS точно работает: #AllowUsers admin@HQ-SRV admin@HQ-R branch_admin@BR-SRV branch_admin@BR-R network_admin@BR-R network_admin@BR-SRV

Пример команды, если для IP (ЛУЧШЕ ПИСАТЬ ЕЁ): #AllowUsers admin@172.16.100.2 admin@172.16.100.1 branch_admin@192.168.100.10 branch_admin@192.168.100.1 network_admin@192.168.100.10

# Данные для авториации в виртуальных машинах стенда

| Тип операционной системы | Логин      |  Пароль         |
| :---------------------:  | :--------: | :-------------: | 
| Eltex                    | admin      | P@ssw0rd        | 
| ALT_SRV                  | root       | P@ssw0rd        | 
| ALT_CLI                  | user       | P@ssw0rd        | 
| ASTRA_CLI                | root       | P@ssw0rd        |
