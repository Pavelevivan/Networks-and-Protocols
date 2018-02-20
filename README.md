# Запуск протокола динамической маршрутизации RIP(англ. Routing Information Protocol) в Сisco Pocket Tracer

## Павельев Иван, КН-202, №20

### Последовательность действий:

1. Создание оборудывания(роутеры, свитч, компьютеры, сервера) и соединения между ними, а также добавления физического модуля NM-1FE-TX или любого другого подходящего.

2. Выставление ip адресов для серверов, роутеров и пк
  Выставление ip для роутеров
  ```
  Router>enable
  Router#conf t
  Router(config)#int fan/k /*где n/k - идентификатор необходимого интерфейса.*/
  Router(config-if)#ip address adr mask /* вместо adr и mask устанавливается нужный адрес и маска в зависимости от подсети */ 
  Router(config-if)#no shutdown
  Router(config-if)#do wr mem
  Router(config-if)#end
  
  ```
  Для серверов и пк ip устанавливаются через Desktop/IP Configurationt, также для пк устанавливается нужный **gateaway**
  
3. Запуск RIP протокола
  ```
  Router#conf t
  Router(config)#router rip 
  Router(config-router)#network n1
  Router(config-router)#network n2
  Router(config-router)#network n3 /* вместо n указывается подстеть
  Router(config-router)#exit
  Router(config)#do wr mem
  Router(config)#end
  ```
4. Смотрим таблицу роутера с помощью команд **show ip interface brief** и **show ip route**
  ```
  Router#show ip interface brief 
  Interface              IP-Address      OK? Method Status                Protocol 
  FastEthernet0/0        10.20.1.2       YES manual up                    up 
  FastEthernet0/1        192.168.10.5    YES manual up                    up 
  FastEthernet1/0        192.168.3.1     YES manual up                    up 
  Vlan1                  unassigned      YES unset  administratively down down
  ```
  ```
  Router#show ip rip database
10.0.0.0/8    auto-summary
10.0.0.0/8
    [2] via 192.168.10.6, 00:01:14, FastEthernet0/1
10.20.1.0/24    auto-summary
10.20.1.0/24    directly connected, FastEthernet0/0
192.168.1.0/24    auto-summary
192.168.1.0/24
    [1] via 10.20.1.1, 00:00:23, FastEthernet0/0
192.168.2.0/24    auto-summary
192.168.2.0/24
    [1] via 192.168.10.6, 00:00:16, FastEthernet0/1
192.168.3.0/24    auto-summary
192.168.3.0/24    directly connected, FastEthernet1/0
192.168.10.0/30    auto-summary
192.168.10.0/30
    [1] via 192.168.10.6, 00:00:16, FastEthernet0/1
192.168.10.0/24    is possibly down
192.168.10.0/24    is possibly down
192.168.10.4/30    auto-summary
192.168.10.4/30    directly connected, FastEthernet0/1
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
R       10.0.0.0/8 [120/2] via 192.168.10.6, 00:01:38, FastEthernet0/1
C       10.20.1.0/24 is directly connected, FastEthernet0/0
R    192.168.1.0/24 [120/1] via 10.20.1.1, 00:00:19, FastEthernet0/0
R    192.168.2.0/24 [120/1] via 192.168.10.6, 00:00:13, FastEthernet0/1
C    192.168.3.0/24 is directly connected, FastEthernet1/0
     192.168.10.0/24 is variably subnetted, 3 subnets, 2 masks
R       192.168.10.0/24 is possibly down, routing via 10.20.1.1, FastEthernet0/0
R       192.168.10.0/30 [120/1] via 192.168.10.6, 00:00:13, FastEthernet0/1
C       192.168.10.4/30 is directly connected, FastEthernet0/1
```
  
  
  
