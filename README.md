# Стенд ПК № 8 (09.02.06 Сетеве и системное администрирование) 2023-2024 год

### Базовый стенд для  09.02.06 Сетеве и системное администрирование представлен по сссылке - https://disk.yandex.ru/d/wVsAJMZ-fcZi3w (Стенд для добавления в VMware Player, вложенная виртуализация через ESXi).

# Топология сети
![image](https://github.com/ItsLiventsev/NetSys_Demo_2024/assets/108996446/af62755f-eb59-4efa-91f2-da89b200ccb7)

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
