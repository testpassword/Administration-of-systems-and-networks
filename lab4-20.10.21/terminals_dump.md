### Шаг 1: Настройте IP-адреса.
#### Настройте IP-адреса для маршрутизаторов R1, R2, R3.

```sh
system-view
sysname R1
system-view
sysname R2
system-view
sysname R3

[R1]interface GigabitEthernet0/0/0
[R1-GigabitEthernet0/0/0]ip address 10.1.2.1 24
Oct 20 2021 21:52:51-08:00 R1 %%01IFNET/4/LINK_STATE(l)[1]:The line protocol IP 
on the interface GigabitEthernet0/0/0 has entered the UP state. 
[R1-GigabitEthernet0/0/0]quit
[R1]interface LoopBack 0
[R1-LoopBack0]ip address 10.1.1.1 24
[R1-LoopBack0]quit	
[R1]interface LoopBack 1
[R1-LoopBack1]ip address 10.1.4.1 24
[R1-LoopBack1]quit

[R2]interface GigabitEthernet 0/0/0
[R2-GigabitEthernet0/0/0]ip address 10.1.2.2 24
Oct 20 2021 21:55:25-08:00 R2 %%01IFNET/4/LINK_STATE(l)[2]:The line protocol IP 
on the interface GigabitEthernet0/0/0 has entered the UP state. 
[R2-GigabitEthernet0/0/0]quit	
[R2]interface GigabitEthernet 0/0/1
[R2-GigabitEthernet0/0/1]ip address 10.1.3.2 24
Oct 20 2021 21:55:59-08:00 R2 %%01IFNET/4/LINK_STATE(l)[3]:The line protocol IP 
on the interface GigabitEthernet0/0/1 has entered the UP state. 
[R2-GigabitEthernet0/0/1]quit 

[R3]interface GigabitEthernet 0/0/0
[R3-GigabitEthernet0/0/0]ip address 10.1.3.1 24
Oct 20 2021 21:57:14-08:00 R3 %%01IFNET/4/LINK_STATE(l)[0]:The line protocol IP 
on the interface GigabitEthernet0/0/0 has entered the UP state. 
[R3-GigabitEthernet0/0/0]quit 
```

### Шаг 2: Настройте OSPF для обеспечения возможности сетевого подключения.
#### Настройте OSPF на маршрутизаторах R1, R2 и R3 и назначьте их область 0, чтобы обеспечить возможность подключения.

```sh
[R1]ospf
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 10.1.1.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]network 10.1.2.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]network 10.1.4.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]return

[R2]ospf
[R2-ospf-1]area 0
[R2-ospf-1-area-0.0.0.0]network 10.1.2.2 0.0.0.0
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[4]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=HelloReceived, NeighborPreviousState=Down, NeighborCurrentState=Init) 
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[5]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=2WayReceived, NeighborPreviousState=Init, NeighborCurrentState=2Way) 
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[6]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=AdjOk?, NeighborPreviousState=2Way, NeighborCurrentState=ExStart) 
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[7]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=NegotiationDone, NeighborPreviousState=ExStart, NeighborCurrentState=Exchan
ge) 
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[8]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=ExchangeDone, NeighborPreviousState=Exchange, NeighborCurrentState=Loading)
 
[R2-ospf-1-area-0.0.0.0]network 
Oct 20 2021 21:59:34-08:00 R2 %%01OSPF/4/NBR_CHANGE_E(l)[9]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=1.2.1.10, NeighborE
vent=LoadingDone, NeighborPreviousState=Loading, NeighborCurrentState=Full) 
[R2-ospf-1-area-0.0.0.0]network 10.1.3.2 0.0.0.0
[R2-ospf-1-area-0.0.0.0]return

[R3]ospf
[R3-ospf-1]area 0
[R3-ospf-1-area-0.0.0.0]network 10.1.3.1 0.0.0.0
[R3-ospf-1-area-0.0.0.0]return 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[1]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=HelloReceived, NeighborPreviousState=Down, NeighborCurrentState=Init) 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[2]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=2WayReceived, NeighborPreviousState=Init, NeighborCurrentState=2Way) 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[3]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=AdjOk?, NeighborPreviousState=2Way, NeighborCurrentState=ExStart) 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[4]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=NegotiationDone, NeighborPreviousState=ExStart, NeighborCurrentState=Exchan
ge) 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[5]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=ExchangeDone, NeighborPreviousState=Exchange, NeighborCurrentState=Loading)
 
<R3>
Oct 20 2021 22:01:09-08:00 R3 %%01OSPF/4/NBR_CHANGE_E(l)[6]:Neighbor changes eve
nt: neighbor status changed. (ProcessId=256, NeighborAddress=2.3.1.10, NeighborE
vent=LoadingDone, NeighborPreviousState=Loading, NeighborCurrentState=Full) 
```

### Выполните команду ping на маршрутизаторе R3 чтобы проверить возможность подключения к сети.

```sh
<R3>ping 10.1.1.1
  PING 10.1.1.1: 56  data bytes, press CTRL_C to break
    Reply from 10.1.1.1: bytes=56 Sequence=1 ttl=254 time=50 ms
    Reply from 10.1.1.1: bytes=56 Sequence=2 ttl=254 time=40 ms
    Reply from 10.1.1.1: bytes=56 Sequence=3 ttl=254 time=30 ms
    Reply from 10.1.1.1: bytes=56 Sequence=4 ttl=254 time=30 ms
    Reply from 10.1.1.1: bytes=56 Sequence=5 ttl=254 time=30 ms

  --- 10.1.1.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 30/36/50 ms

<R3>ping 10.1.2.1
  PING 10.1.2.1: 56  data bytes, press CTRL_C to break
    Reply from 10.1.2.1: bytes=56 Sequence=1 ttl=254 time=40 ms
    Reply from 10.1.2.1: bytes=56 Sequence=2 ttl=254 time=30 ms
    Reply from 10.1.2.1: bytes=56 Sequence=3 ttl=254 time=30 ms
    Reply from 10.1.2.1: bytes=56 Sequence=4 ttl=254 time=30 ms
    Reply from 10.1.2.1: bytes=56 Sequence=5 ttl=254 time=20 ms

  --- 10.1.2.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/30/40 ms

<R3>ping 10.1.4.1
  PING 10.1.4.1: 56  data bytes, press CTRL_C to break
    Reply from 10.1.4.1: bytes=56 Sequence=1 ttl=254 time=20 ms
    Reply from 10.1.4.1: bytes=56 Sequence=2 ttl=254 time=30 ms
    Reply from 10.1.4.1: bytes=56 Sequence=3 ttl=254 time=30 ms
    Reply from 10.1.4.1: bytes=56 Sequence=4 ttl=254 time=40 ms
    Reply from 10.1.4.1: bytes=56 Sequence=5 ttl=254 time=30 ms

  --- 10.1.4.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/30/40 ms
```

### Шаг 3: Сконфигурируйте R3 в качестве сервера.
#### Включите функцию Telnet на R3, установите для уровня пользователя значение 3 и задайте для входа пароль - Huawei@123.

```sh
[R3]telnet server enable
 Error: TELNET server has been enabled

[R3]user-interface vty 0 4
[R3-ui-vty0-4]user privilege level 3
[R3-ui-vty0-4]set authentication password cipher Huawei@123
[R3-ui-vty0-4]quit
```

### Шаг 4: Настройте ACL на основе неободимого трафика.
#### Настройте ACL на R3.

```sh
[R3]acl 3000
[R3-acl-adv-3000]rule 5 permit tcp source 10.1.4.1 0.0.0.0 destination 10.1.3.1 
0.0.0.0 destination-port eq 23
[R3-acl-adv-3000]rule 10 deny tcp source any
[R3-acl-adv-3000]quit 
```

#### Выполните фильтрацию трафика на интерфейсе VTY маршрутизатора R3.

```sh
[R3]user-interface vty 0 4
[R3-ui-vty0-4]acl 3000 inb	
[R3-ui-vty0-4]acl 3000 inbound
[R3-ui-vty0-4]quit
```

#### Выведите на экран конфигурацию ACL на R3.

```sh
[R3]display acl 3000
Advanced ACL 3000, 2 rules
Acl's step is 5
 rule 5 permit tcp source 10.1.4.1 0 destination 10.1.3.1 0 destination-port eq 
telnet 
 rule 10 deny tcp
```

#### Настройте ACL на R2.

```sh
[R2]acl 3001
[R2-acl-adv-3001]rule 5 permit tcp source 10.1.4.1 0.0.0.0 destination-port eq 23
[R2-acl-adv-3001]rule 10 deny tcp source any
[R2-acl-adv-3001]quit
```

#### Выполните фильтрацию трафика на интерфейсе GE0/0/0/3 маршрутизатора R3.

```sh
[R2]interface GigabitEthernet 0/0/0
[R2-GigabitEthernet0/0/0]traffic-filter inbound acl 3001
[R2-GigabitEthernet0/0/0]quit
```

#### Выведите на экран конфигурацию ACL на R2.

```sh
[R2]display acl 3001
Advanced ACL 3001, 2 rules
Acl's step is 5
 rule 5 permit tcp source 10.1.4.1 0 destination-port eq telnet 
 rule 10 deny tcp 
```

### Проверка: протестируйте доступ через Telnet и проверьте конфигурацию ACL.

#### На маршрутизаторе R1 подключитесь через Telnet к серверу, используя указанный IP-адрес источника 10.1.1.1.

```sh
<R1>telnet -a 10.1.1.1 10.1.3.1
  Press CTRL_] to quit telnet mode
  Trying 10.1.3.1 ...
  Error: Can't connect to the remote host
```

#### На маршрутизаторе R1 подключитесь через Telnet к серверу, используя указанный IP-адрес источника 10.1.4.1.

```sh
<R1>telnet -a 10.1.1.1 10.1.3.1
  Press CTRL_] to quit telnet mode
  Trying 10.1.3.1 ...
  Error: Can't connect to the remote host
<R1>telnet -a 10.1.4.1 10.1.3.1
  Press CTRL_] to quit telnet mode
  Trying 10.1.3.1 ...
  Connected to 10.1.3.1 ...

Login authentication


Password:
<R3>quit

  Configuration console exit, please retry to log on

  The connection was closed by the remote host
<R1>
```