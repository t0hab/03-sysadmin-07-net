# Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
### Ответ:

Я привык по сторинке `ifconfig`, сейчас же все использую `ip`. Ниже представлены примеры.
```bash
vagrant@vagrant:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:b1:28:5d brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
       valid_lft 86170sec preferred_lft 86170sec
    inet6 fe80::a00:27ff:feb1:285d/64 scope link 
       valid_lft forever preferred_lft forever
vagrant@vagrant:~$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:feb1:285d  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:b1:28:5d  txqueuelen 1000  (Ethernet)
        RX packets 590  bytes 70064 (70.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 482  bytes 79021 (79.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vagrant@vagrant:~$ 
```
---

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?
### Ответ:
Link Layer Discovery Protocol (LLDP) используется для распознавания соседа по сетевому интерфейсу. 
LLDP это стандартный протокол, который описан в IEEE 802.1AB

LLDP в Linux поддерживает не только LLDP, но также и CDP, EDP, SONMP и AgentX SNMP.
Активация соответствующих протоколов выполняется ключами:
```
-x — AgentX SNMP
-s — SONMP
-c — CDP
-f — FDP
-e — EDP 
```

Команды 
```
lldp enable - Включить LLDP глобально или на порту. Команда no отключает эту функцию
lldp disable - Включить LLDP глобально или на порту. Команда no отключает эту функцию
lldp mode (send|receive|both|disable) - Настроить режим LLDP на порту, send - только отправка, receive - только прием, both - оба направления (по умолчанию)

lldp tx-interval <integer> - Настроить интервал отправки LLDP сообщений в секундах. Команда no восстанавливает конфигурацию по-умолчанию - 30 секунд.
lldp msgTxHold <value> - Настроить количество интервалов tx-interval - время жизни информации о соседе LLDP с момента последнего обновления. Команда no восстанавливает конфигурацию по-умолчанию - 4.
lldp transmit delay <seconds> - Задать время в течении которого коммутатор не будет принимать новые LLDP сообщения на порту после получения последнего. Команда no восстанавливает конфигурацию по-умолчанию - 2 секунды.

lldp trap <enable|disable> - Включить LLDP trap для порта. Команда no отключает эту функцию.
lldp notification interval <seconds> - Задать время отправки trap после изменения LLDP таблицы. Команда no восстанавливает конфигурацию по-умолчанию - 5 секунд.

show lldp - Вывести суммарную информацию о конфигурации LLDP на коммутаторе.
show lldp interface ethernet <IFNAME> - Вывести информацию по конфигурации LLDP на порту коммутатора.
show lldp traffic - Вывести суммарную информацию об отправленных и полученных пакетах LLDP.

```
Полную информацию можно изучить на https://nag.wiki/display/DOC/05.+LLDP

---

3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.


4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.


5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.


6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.


7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?
