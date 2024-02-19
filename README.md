## Стенд ПК № 8 (09.02.06 Сетевое и системное администрирование) 2023-2024 год

Базовый стенд для  09.02.06 Сетеве и системное администрирование представлен по сссылке - https://disk.yandex.ru/d/wVsAJMZ-fcZi3w (Стенд для добавления в VMware Player, вложенная виртуализация через ESXi).

# Топология сети

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/af62755f-eb59-4efa-91f2-da89b200ccb7)

# Таблица сети

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


## Базовая работа с сетями (выход в глобальную сеть, проброс DHCP, установка nmtui)

Обратите внимание, что для настройки, необходимо работать с сетью VM Network (выходит на физический адаптер по NAT).

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/e892be81-5bd6-4847-b4d9-47550cf2a223)

### Проброс виртуальной машины в сеть VM Network

Открываем настройки виртуальной машины, выбираем сеть VM Network

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/c784d598-1526-44d4-b142-0bd7386829c7)

### Настройка ALT в терминальном режиме для получения DHCP

Используем текстовый редактор mcedit или vim (mcedit имеет графический интерфейс).

Файлы для настройки сетевых адаптеров расположены по пути - /etc/net/iface/

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/a94cb3b7-9e81-4717-8b47-00552aab4cf8)

Обратите внимание, что для каждого сетевого адаптера создается отдельная директория с настройками. Для выполнения базовых настроек (static или DHCP) используется уже созданный файл options.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/47d43a12-fd4c-45dc-affb-0f10e666d1e0)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/a4a43d3a-bc40-4596-b7a5-cf4d097ceb54)

Для перевода сетевого адаптера в работу по DHCP необходимо изменить параметр BOOTPROTO на DHCP.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/0eb649ed-d55b-4386-90a3-9789608d75a5)

Для сохранения нажимаем F2 или F10, где выбираем Yes.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/7ace5911-b319-4ae9-a07d-784838a7c614)

Перезагружаем "сеть" командой - systemctl restart network

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/22d9e8df-3e62-4860-a650-ad7237c8325f)

Проверяем насройки сетевого адаптера командой - ip a

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/9f847eaa-55aa-4e84-b715-cfc6c7f4af76)

Проверяем подключение к глобальной сети командой - ping

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/6250241a-8247-4938-af13-c22af309daa4)

### Установка NetworkManager-tui

apt-get update - обновляем репозитории (Обратите внимание, что работает только apt-get, команда apt отключена).

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/782d1dcd-c21b-4ceb-9c20-ab68d7c586d2)

apt-get install NetworkManager-tui

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/fa8252f8-6c92-477e-9d18-c08ffe1be4db)

Соглашаемся с установкой заимствований для работы пакета (Y).

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/bf33af6d-d15c-4459-9c49-618fe2645caf)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/43bbfc0b-2204-474f-bc9f-011dffefec3c)

Далее запускаем NetworkManager через команду - nmtui, если выводит ошибку перезапускаем службу пакета.

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/2feedfe6-dc9e-4ebd-911a-746258370b21)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/17ab9a68-e43d-4e39-8f0a-6e604db5da65)

![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/22e0a8ba-5576-4e98-980f-04b315cc75f9)

# Установка программ в соответствии с заданием для каждой виртулаьной машины:

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

## Базовая работа с сетями (работа через тектовый редактор и файлы системы, настройка статики, dns)

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

--------------------------

# Данные для авториации в виртуальных машинах стенда

| Тип операционной системы | Логин      |  Пароль         |
| :---------------------:  | :--------: | :-------------: | 
| Eltex                    | admin      | P@ssw0rd        | 
| ALT_SRV                  | admin      | P@ssw0rd        | 
| ALT_CLI                  | admin      | P@ssw0rd        | 
| ASTRA_CLI                | root       | P@ssw0rd        |
