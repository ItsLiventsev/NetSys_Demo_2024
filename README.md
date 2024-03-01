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

### Базовая работа с сетями (выход в глобальную сеть, проброс DHCP)

Обратите внимание, что для настройки, необходимо работать с сетью VM Network (выходит на физический адаптер по NAT).

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e892be81-5bd6-4847-b4d9-47550cf2a223)

### Проброс виртуальной машины в сеть VM Network

Открываем настройки виртуальной машины, выбираем сеть VM Network

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c784d598-1526-44d4-b142-0bd7386829c7)

### Установка программ в соответствии с заданием для каждой виртулаьной машины:

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

## Задание 1 модуля 1

### 1. Выполните базовую настройку всех устройств:
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

#### Настройка статического подключения

Настройка файла options

mcedit /etc/net/ifaces/ens192/options

В файле проверем, чтобы значения стояли на статику (static)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/66503d13-82bd-411d-bff0-5fba1348fe44)

#### Настройка IP-адреса

mcedit /etc/net/ifaces/ens192/ipv4address

Обратите внимание, что мы создаем новые файлы, изначально в них пусто

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/d7713a50-4595-4815-bb7c-5ebd871edbb2)

Прописываем адрес в привычном формате и через "/" прописываем маску - пример для HQ-SRV - 192.168.0.1/25

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f03d5719-392a-4c9e-9806-d7930f3696b4)

Нажимаем на F10 И сохраняем изменения в файле.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c10079d2-cc90-415e-bd2b-22bbc48b3b21)

#### Настройка шлюза

mcedit /etc/net/ifaces/ens192/ipv4route

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/d51608a7-5593-465a-a581-ac956c2af999)

Обратите внимание, что мы создаем новые файлы, изначально в них пусто

Прописываем адрес в привычном формате - пример для HQ-SRV - 192.168.0.1

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/ab2b6cb5-ea04-4c4d-a725-2b14f8ae4575)

#### Настройка IP-адреса DNS

mcedit /etc/net/ifaces/ens192/resolv.conf

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/bac49099-2a90-4e0a-9ca7-59bb6f72f7a4)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/a5a3fdb5-ee0e-48f5-959f-da2bf58a04e5)

Через команду ip route проверяем, что шлюз поставлен

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/9df072a1-4c3f-490a-b1db-bdc0ba3f25e8)

#### Маршрутизация транзитных пакетов на роутерах

На устройствах ISP, HQ-R, BR-R необходимо включить пересылку пакетов между интерфейсами - forwarding. Чтобы включить пересылку пакетов между интерфейсами, необходимо отредактировать файл sysctl.conf.

mcedit /etc/sysctl.conf и добавить в файл следующие значения:

net.ipv4.ip_forward=1

net.ipv6.conf.all.forwarding=1

После необходимо применить изменения - sysctl -p (или перезагрузиться).

--------------------------

### 2. Настройте внутреннюю динамическую маршрутизацию по  средствам FRR. 
Выберите и обоснуйте выбор протокола 
динамической маршрутизации из расчёта, что в 
дальнейшем сеть будет масштабироваться.

Установка пакета frr:

apt-get install frr

Настройка утилиты frr:

Обратите внимание, что для настройки OSPF для IPv4 используется протокол OSPF второй версии для IPv6 OSPF третьей версии.

Для настройки OSPF необходимо включить соответствующий демон в конфигурации /etc/frr/daemons

mcedit /etc/frr/daemons

В конфигурационном файле /etc/frr/daemons необходимо активировать выбранный протокол для дальнейшей реализации его настройки

ospfd = yes - для OSPFv2 (IPv4)

ospf6d = yes - для OSPFv3 (IPv6)

Включаем и добавляем в автозагрузку службу frr:

systemctl enable --now frr

Открываем утилиту frr для настройки маршрутизации:

vtysh

#### Настройки OSPFv2 и OSPFv3 на HQ-R

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/f8eda396-323d-4053-862d-c06ecc007495)

#### Настройки OSPFv2 и OSPFv3 на BR-R

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c975593f-b36c-4f4d-b72d-23dc73e6757b)

###### Сноска по командам в frr

configure terminal - вход в режим глобальной конфигурации

router ospf - переход в режим конфигурации OSPFv2

passive-interface default - перевод всех интерфейсов в пассивный режим

network - объявляем локальную сеть офиса HQ и сеть (GRE-туннеля)

exit - выход и режима конфигурации OSPFv2

туннельный интерфейс "tun1" делаем активным, для устанавления соседства с BR-R и обмена внутренними маршрутами

no ip ospf passive - перевод интерфейса tun1 в активный режим

write - сохраняем текущую конфигурацию

router ospf6 - переход в режим конфигурации OSPFv3

ospf6 router-id - назначение номера router-id

сети интерфейсов tun1 и enp0s3 добавляем в конфигурацию OSPFv3

write - сохраняем текущую конфигурацию

#### a. Составьте топологию сети L3 (ожидает схемы)

----------------------
### 3. Настройте автоматическое распределение IP-адресов на роутере HQ-R (ожидает картинок)
a. Учтите, что у сервера должен быть зарезервирован адрес.

#### Установка и настройка DHCP ДЛЯ IPv4

apt-get install isc-dhcp-server

#### Добавление утилиты в атозагрузку

systemctl enable --now dhcpd

#### Производим конфигурацию сервера через файл

mcedit /etc/dhcp/dhcpd.conf

##### Добавление нового пула для раздачи сетевых настроек по DHCP (надо доработать скриншоты)

subnet 172.16.100.0 netmask 255.255.255.192 {
  range 172.16.100.2 172.16.100.12;
  option domain-name-servers 172.16.100.2;
  option routers 172.16.100.1;
  default-lease-time 600;
  max-lease-time 7200;
}

#### Резервирование адреса для сервера

host hq-srv {
        hardware ethernet ab:cd:ef:ab:aa:aa; - (прописать MAC-адрес сетевого адаптера из настроек виртуальной машины)
        fixed-address 172.16.100.2;
}

#### Настройка DHCP ДЛЯ IPv6



------------------

# Данные для авториации в виртуальных машинах стенда

| Тип операционной системы | Логин      |  Пароль         |
| :---------------------:  | :--------: | :-------------: | 
| Eltex                    | admin      | P@ssw0rd        | 
| ALT_SRV                  | root       | P@ssw0rd        | 
| ALT_CLI                  | user       | P@ssw0rd        | 
| ASTRA_CLI                | root       | P@ssw0rd        |
