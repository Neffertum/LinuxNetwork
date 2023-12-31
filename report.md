## Part 1. Инструмент ipcalc

Поднимаем виртуальную машину ws1

#### 1.1. Сети и маски

1) Определяем адрес сети 192.167.38.54/13

![1.1-1](screen/1.1-1.png)

2) Перевод маски 255.255.255.0 в префиксную и двоичную запись

![1.1-2.1](screen/1.1-2.1.png)

Перевод /15 в обычную и двоичную запись

![1.1-2.2](screen/1.1-2.2.png)

Перевод 11111111.11111111.11111111.11110000 в обычную и префиксную запись: 

Обычная - 255.255.255.240

Префиксная - /28

3) Минимальный и максимальный хост в сети 12.167.38.4 при масках: 

/8

![1.1-3.1](screen/1.1-3.1.png)

11111111.11111111.00000000.00000000

![1.1-3.2](screen/1.1-3.2.png)

255.255.254.0

![1.1-3.3](screen/1.1-3.3.png)

/4

![1.1-3.4](screen/1.1-3.4.png)

#### 1.2. localhost

Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP:

194.34.23.100 - нельзя

![1.2-1](screen/1.2-1.png)

127.0.0.2 - можно

![1.2-2](screen/1.2-2.png)

127.1.0.1 - можно

![1.2-3](screen/1.2-3.png)

128.0.0.1 - нельзя

![1.2-4](screen/1.2-4.png)

#### 1.3. Диапазоны и сегменты сетей

Определить и записать в отчёт:

1) какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 

10.0.0.45 - частный

![1.3-1.1](screen/1.3-1.1.png)

134.43.0.2 - публичный

![1.3-1.2](screen/1.3-1.2.png)

192.168.4.2 - частный

![1.3-1.3](screen/1.3-1.3.png)

172.20.250.4 - частный

![1.3-1.4](screen/1.3-1.4.png)

172.0.2.1 - публичный

![1.3-1.5](screen/1.3-1.5.png)

192.172.0.1 - публичный

![1.3-1.6](screen/1.3-1.6.png)

172.68.0.2 - публичный

![1.3-1.7](screen/1.3-1.7.png)

172.16.255.255 - частный

![1.3-1.8](screen/1.3-1.8.png)

10.10.10.10 - частный

![1.3-1.9](screen/1.3-1.9.png)

192.169.168.1 - публичный

![1.3-1.10](screen/1.3-1.10.png)

2) какие из перечисленных IP адресов шлюза возможны у сети 10.10.0.0/18:

10.0.0.1 - невозможен

10.10.0.2 - возможен

10.10.10.10 - возможен

10.10.100.1 - невозможен

10.10.1.255 - возможен

![1.3-2](screen/1.3-2.png)

## Part 2. Статическая маршрутизация между двумя машинами

Поднять две виртуальные машины (далее -- ws1 и ws2)

С помощью команды ip a посмотреть существующие сетевые интерфейсы

![2.0-1](screen/2.0-1.png)

Описать сетевой интерфейс, соответствующий внутренней сети, на обеих машинах и задать следующие адреса и маски: ws1 - 192.168.100.10, маска /16, ws2 - 172.24.116.8, маска /12

![2.0-2](screen/2.0-2.png)

Выполнить команду netplan apply для перезапуска сервиса сети

![2.0-3](screen/2.0-3.png)

#### 2.1. Добавление статического маршрута вручную

Добавить статический маршрут от одной машины до другой и обратно при помощи команды вида ip r add

![2.1-1](screen/2.1-1.png)

Пропинговать соединение между машинами

![2.1-2](screen/2.1-2.png)

## 2.2. Добавление статического маршрута с сохранением

Перезапустить машины

Добавить статический маршрут от одной машины до другой с помощью файла etc/netplan/00-installer-config.yaml

![2.2-1](screen/2.2-1.png)

Пропинговать соединение между машинами

![2.2-2](screen/2.2-2.png)

## Part 3. Утилита iperf3

#### 3.1. Скорость соединения

Перевести и записать в отчёт: 8 Mbps в MB/s, 100 MB/s в Kbps, 1 Gbps в Mbps

8Mbps = 1MBps

100MBps = 819200Kbps

1Gbps = 1024Mbps

#### 3.2. Утилита iperf3

Измерить скорость соединения между ws1 и ws2

![3.2](screen/3.2.png)

## Part 4. Сетевой экран

#### 4.1. Утилита iptables

Создать файл /etc/firewall.sh, имитирующий фаерволл, на ws1 и ws2

![4.1-1](screen/4.1-1.png)

Запустить файлы на обеих машинах командами chmod +x /etc/firewall.sh и /etc/firewall.sh

![4.1-2](screen/4.1-2.png)

    Код для фильтрации пакетов организован в виде набора таблиц, каждая из которых предназначена для конкретной цели. 
    Таблица состоит из группы предопределённых цепочек содержащих правила, которые проверяются по очереди.

    Каждое правило состоит из условий и действия. 
    Действие применяется к пакету,n если все условия выполнены.

    Правила iptables в firewall.sh добавляются последовательно. 
    Если условие подходит, то сразу выполняется действиe (ACCEPT или DROP). 
    Срабатывает ПЕРВОЕ правило.

    ws1 первым правилом отклоняет пакеты imcp. Пакеты от ws1 не возвращаются.

    ws2 первым правилом разрешает пакеты imcp. Пакеты от ws2 приходят.


#### 4.2. Утилита nmap


Командой ping найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен
Проверка: в выводе nmap должно быть сказано: Host is up

![4.2](screen/4.2.png)

## Part 5. Статическая маршрутизация сети

Сеть:

![5.0](screen/5.0.png)

Поднять пять виртуальных машин (3 рабочие станции (ws11, ws21, ws22) и 2 роутера (r1, r2))

#### 5.1. Настройка адресов машин

Настроить конфигурации машин в etc/netplan/00-installer-config.yaml согласно сети на рисунке.

![5.1-1-11](screen/5.1-1-11.png)

![5.1-1-21](screen/5.1-1-21.png)

![5.1-1-22](screen/5.1-1-22.png)

![5.1-1-1](screen/5.1-1-1.png)

![5.1-1-2](screen/5.1-1-2.png)

Перезапустить сервис сети. Если ошибок нет, то командой ip -4 a проверить, что адрес машины задан верно. 

![5.1-2-11](screen/5.1-2-11.png)

![5.1-2-21](screen/5.1-2-21.png)

![5.1-2-22](screen/5.1-2-22.png)

![5.1-2-1](screen/5.1-2-1.png)

![5.1-2-2](screen/5.1-2-2.png)

Также пропинговать ws22 с ws21. Аналогично пропинговать r1 с ws11.

![5.1-3-21](screen/5.1-3-21.png)

![5.1-3-11](screen/5.1-3-11.png)

#### 5.2. Включение переадресации IP-адресов.

Для включения переадресации IP, выполните команду на роутерах:
sysctl -w net.ipv4.ip_forward=1
При таком подходе переадресация не будет работать после перезагрузки системы.

![5.2-1-1](screen/5.2-1-1.png)

![5.2-1-2](screen/5.2-1-2.png)

Откройте файл /etc/sysctl.conf и добавьте в него следующую строку:
net.ipv4.ip_forward = 1
При использовании этого подхода, IP-переадресация включена на постоянной основе.

![5.2-2-1](screen/5.2-2-1.png)

![5.2-2-2](screen/5.2-2-2.png)

#### 5.3. Установка маршрута по-умолчанию

Пример вывода команды ip r после добавления шлюза:

    default via 10.10.0.1 dev eth0
    10.10.0.0/18 dev eth0 proto kernel scope link src 10.10.0.2

Настроить маршрут по-умолчанию (шлюз) для рабочих станций. Для этого добавить default перед IP роутера в файле конфигураций

![5.3-1-1](screen/5.3-1-1.png)

![5.3-1-2](screen/5.3-1-2.png)

Вызвать ip r и показать, что добавился маршрут в таблицу маршрутизации

![5.3-2-11](screen/5.3-2-11.png)

![5.3-2-21](screen/5.3-2-21.png)

![5.3-2-22](screen/5.3-2-22.png)

![5.3-2-1](screen/5.3-2-1.png)

![5.3-2-2](screen/5.3-2-2.png)

Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. Для этого использовать команду:
tcpdump -tn -i eth1

![5.3-3-11](screen/5.3-3-11.png)

![5.3-3-2](screen/5.3-3-2.png)

#### 5.4. Добавление статических маршрутов

Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций

![5.4-1-1](screen/5.4-1-1.png)

![5.4-1-2](screen/5.4-1-2.png)

Вызвать ip r и показать таблицы с маршрутами на обоих роутерах

![5.4-2-1](screen/5.4-2-1.png)

![5.4-2-2](screen/5.4-2-2.png)

Запустить команды на ws11:
ip r list 10.10.0.0/18 и ip r list 0.0.0.0/0

![5.4-3-11](screen/5.4-3-11.png)

В отчёте объяснить, почему для адреса 10.10.0.0/18 был выбран маршрут, отличный от 0.0.0.0/0, хотя он попадает под маршрут по-умолчанию:

    Маршрут по умолчанию выполняется в случае отсутствия других подходящих в netplan.
    Для адреса 10.10.0.0 был выбран маршрут, отличный от 0.0.0.0/0, 
    потому что маска /18 описывает маршрут к сети.

#### 5.5. Построение списка маршрутизаторов

Запустить на r1 команду дампа:
tcpdump -tnv -i eth0

![5.5-1](screen/5.5-1.png)

При помощи утилиты traceroute построить список маршрутизаторов на пути от ws11 до ws21

![5.5-11](screen/5.5-11.png)

    Команда traceroute  отправляет пакет с TTL (time to live - время жизни,
    указывает максимальное количество маршрутизаторов, которое может быть пройдено пакетом.)
    и показывает адрес порта ответившего об исчерпании TTL, на каждом следующем шаге
    TTL увеличивается на единицу пока не достигнет целевого адреса.

    Первый пакет отправляется с TTL, равным 1, и поэтому первый же маршрутизатор
    возвращает обратно сообщение ICMP, указывающее на невозможность доставки данных.

    Затем traceroute повторяет отправку пакета, но уже с TTL, равным 2,
    что позволяет первому маршрутизатору пропустить пакет дальше.

    Процесс повторяется до тех пор, пока при определённом
    значении TTL пакет не достигнет целевого узла.

    При получении ответа от этого узла процесс трассировки считается завершённым.

#### 5.6. Использование протокола ICMP при маршрутизации

Запустить на r1 перехват сетевого трафика, проходящего через eth0 с помощью команды:
tcpdump -n -i eth0 icmp

![5.6-1](screen/5.6-1.png)

Пропинговать с ws11 несуществующий IP (например, 10.30.0.111) с помощью команды:
ping -c 1 10.30.0.111

![5.6-11](screen/5.6-11.png)

## Part 6. Динамическая настройка IP с помощью DHCP

Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP:

1) указать адрес маршрутизатора по-умолчанию, DNS-сервер и адрес внутренней сети. 

![6-1-2](screen/6-1-2.png)

2) в файле resolv.conf прописать nameserver 8.8.8.8.

![6-2-2](screen/6-2-2.png)

Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server. 

![6-3-2](screen/6-3-2.png)

Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес.

![6-4-21](screen/6-4-21.png)

Также пропинговать ws22 с ws21.

![6-5-21](screen/6-5-21.png)

Указать MAC адрес у ws11, для этого в etc/netplan/00-installer-config.yaml надо добавить строки: macaddress: 10:10:10:10:10:BA, dhcp4: true

![6-6-11](screen/6-6-11.png)

Для r1 настроить аналогично r2, но сделать выдачу адресов с жесткой привязкой к MAC-адресу (ws11). 

![6-7-1](screen/6-7-1.png)

![6-8-1](screen/6-8-1.png)

Провести аналогичные тесты

![6-9-11](screen/6-9-11.png)

![6-10-11](screen/6-10-11.png)

Запросить с ws21 обновление ip адреса

![6-11-21](screen/6-11-21.png)

![6-12-21](screen/6-12-21.png)

## Part 7. NAT

В файле /etc/apache2/ports.conf на ws22 и r1 изменить строку Listen 80 на Listen 0.0.0.0:80, то есть сделать сервер Apache2 общедоступным

![7-1-22](screen/7-1-22.png)

![7-1-1](screen/7-1-1.png)

Запустить веб-сервер Apache командой service apache2 start на ws22 и r1

![7-2-22](screen/7-2-22.png)

![7-2-1](screen/7-2-1.png)

Добавить в фаервол, созданный по аналогии с фаерволом из Части 4, на r2 следующие правила:

    1) Удаление правил в таблице filter - iptables -F
    2) Удаление правил в таблице "NAT" - iptables -F -t nat
    3) Отбрасывать все маршрутизируемые пакеты - iptables --policy FORWARD DROP

![7-3-2](screen/7-3-2.png)

Запускать файл также, как в Части 4

![7-4-2](screen/7-4-2.png)

Проверить соединение между ws22 и r1 командой ping

При запуске файла с этими правилами, ws22 не должна "пинговаться" с r1

![7-5-1](screen/7-5-1.png)

Добавить в файл ещё одно правило:

    4) Разрешить маршрутизацию всех пакетов протокола ICMP

![7-6-2](screen/7-6-2.png)

Запускать файл также, как в Части 4

![7-7-2](screen/7-7-2.png)

Проверить соединение между ws22 и r1 командой ping

При запуске файла с этими правилами, ws22 должна "пинговаться" с r1

![7-8-1](screen/7-8-1.png)

Добавить в файл ещё два правила:

    5) Включить SNAT, а именно маскирование всех локальных ip из локальной сети, 
    находящейся за r2 (по обозначениям из Части 5 - сеть 10.20.0.0)

    6) Включить DNAT на 8080 порт машины r2 и добавить к веб-серверу Apache, 
    запущенному на ws22, доступ извне сети

![7-9-2](screen/7-9-2.png)

Запускать файл также, как в Части 4

![7-10-2](screen/7-10-2.png)

Проверить соединение по TCP для SNAT, для этого с ws22 подключиться к серверу Apache на r1 командой:
telnet [адрес] [порт]

![7-11-1](screen/7-11-1.png)

Проверить соединение по TCP для DNAT, для этого с r1 подключиться к серверу Apache на ws22 командой telnet (обращаться по адресу r2 и порту 8080)

![7-12-1](screen/7-12-1.png)

## Part 8. Дополнительно. Знакомство с SSH Tunnels

Запустить на r2 фаервол с правилами из Части 7

![8-1-2](screen/8-1-2.png)

![8-2-2](screen/8-2-2.png)

Запустить веб-сервер Apache на ws22 только на localhost

![8-3-22](screen/8-3-22.png)

Воспользоваться Local TCP forwarding с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21

    Чтобы создать локальную переадресацию портов, передайте -L параметр ssh клиенту:
    ssh -L [LOCAL_IP:]LOCAL_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER

![8-4-21](screen/8-4-21.png)

Проверка подключения

![8-6-21](screen/8-6-21.png)

Воспользоваться Remote TCP forwarding c ws11 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws11

    Для создания удаленной переадресации портов передайте -R параметр ssh клиенту:
    ssh -R [REMOTE:]REMOTE_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER

![8-5-22](screen/8-5-22.png)

Проверка подключения

![8-7-22](screen/8-7-22.png)
