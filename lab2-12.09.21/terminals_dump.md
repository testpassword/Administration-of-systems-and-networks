### Шаг 1: Настройте основные параметры устройств.
#### Задайте имена устройствам.

```sh
<Huawei>system-view
[Huawei]sysname R1

<Huawei>system-view
[Huawei]sysname R2

<Huawei>system-view
[Huawei]sysname R3
```

### Шаг 2: Выведите IP-адрес текущего интерфейса и таблицу маршрутизации.
#### Выведите статус интерфейса.

```sh
[R1]display ip interface brief
*down: administratively down
^down: standby
(l): loopback
(s): spoofing
The number of interface that is UP in Physical is 3
The number of interface that is DOWN in Physical is 1
The number of interface that is UP in Protocol is 1
The number of interface that is DOWN in Protocol is 3

Interface                         IP Address/Mask      Physical   Protocol  
GigabitEthernet0/0/0              unassigned           up         down      
GigabitEthernet0/0/1              unassigned           up         down      
GigabitEthernet0/0/2              unassigned           down       down      
NULL0                             unassigned           up         up(s)     
```

#### Выведите таблицу маршрутизации.

```sh
[R1]display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 4        Routes : 4        

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

### Шаг 3: Настройте IP-адреса для физических интерфейсов.
#### Настройте IP-адреса для физических интерфейсов на основе таблицы.
| Маршрутизатор | Интерфейс            | IP-адрес/маска |
|:-------------:|----------------------|----------------|
| R1            | GigabitEthernet0/0/1 | 10.0.13.1/24   |
|               | GigabitEthernet0/0/3 | 10.0.12.1/24   |
| R2            | GigabitEthernet0/0/3 | 10.0.12.2/24   |
|               | GigabitEthernet0/0/4 | 10.0.23.2/24   |
| R3            | GigabitEthernet0/0/1 | 10.0.13.3/24   |
|               | GigabitEthernet0/0/3 | 10.0.23.3/24   |

```sh
[R1]interface GigabitEthernet0/0/0
[R1-GigabitEthernet0/0/0]ip address 10.0.13.1 24
[R1-GigabitEthernet0/0/0]quit
[R1]interface GigabitEthernet0/0/1
[R1-GigabitEthernet0/0/1]ip address 10.0.12.1 24
[R1-GigabitEthernet0/0/1]quit

[R2]interface GigabitEthernet0/0/1
[R2-GigabitEthernet0/0/1]ip address 10.0.12.2 24
[R2-GigabitEthernet0/0/1]quit
[R2]interface GigabitEthernet0/0/2
[R2-GigabitEthernet0/0/2]ip address 10.0.23.2 24
[R2-GigabitEthernet0/0/2]quit

[R3]interface GigabitEthernet0/0/0
[R3-GigabitEthernet0/0/0]ip address 10.0.13.3 24
[R3-GigabitEthernet0/0/0]quit
[R3]interface GigabitEthernet0/0/1
[R3-GigabitEthernet0/0/1]ip address 10.0.23.3 24
[R3-GigabitEthernet0/0/1]quit 
```

#### Проверьте наличие связи с помощью ping.

```sh
[R1]ping 10.0.12.2
  PING 10.0.12.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.12.2: bytes=56 Sequence=1 ttl=255 time=130 ms
    Reply from 10.0.12.2: bytes=56 Sequence=2 ttl=255 time=20 ms
    Reply from 10.0.12.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.12.2: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.12.2: bytes=56 Sequence=5 ttl=255 time=30 ms

  --- 10.0.12.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/44/130 ms

[R1]ping 10.0.13.3
  PING 10.0.13.3: 56  data bytes, press CTRL_C to break
    Reply from 10.0.13.3: bytes=56 Sequence=1 ttl=255 time=100 ms
    Reply from 10.0.13.3: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.13.3: bytes=56 Sequence=3 ttl=255 time=40 ms
    Reply from 10.0.13.3: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.13.3: bytes=56 Sequence=5 ttl=255 time=30 ms

  --- 10.0.13.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/44/100 ms
```

#### Выведите на экран таблицу маршрутизации R1.

```sh
[R1]display ip routing-table 
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 10       Routes : 10       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

### Шаг 4: Создайте loopback-интерфейс.
#### Настройте loopback-интерфейс в соответствии с таблицей.
| Маршрутизатор | Интерфейс | IP-адрес/маска |
|:-------------:|-----------|----------------|
| R1            | LoopBack0 | 10.0.1.1/32    |
| R2            | LoopBack0 | 10.0.1.2/32    |
| R3            | LoopBack0 | 10.0.1.3/32    |

```sh
[R1]interface LoopBack0
[R1-LoopBack0]ip address 10.0.1.1 32
quit

[R2]interface LoopBack0
[R2-LoopBack0]ip address 10.0.1.2 32
quit

[R3]interface LoopBack0
[R3-LoopBack0]ip address 10.0.1.3 32
quit
```
#### Выведите таблицу маршрутизации.

```sh
[R1]display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 11       Routes : 11       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Проверьте наличие связи между loopback-интерфейсами.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Request time out
    Request time out
    Request time out
    Request time out
    Request time out

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    0 packet(s) received
    100.00% packet loss
```

### Шаг 5: Настройте статические маршруты.
#### На маршрутизаторе R1 настройте маршрут к интерфейсам LoopBack0 маршрутизаторов R2 и R3.

```sh
[R1]ip route-static 10.0.1.2 32 10.0.12.2
[R1]ip route-static 10.0.1.3 32 10.0.13.3
```

#### Выведите таблицу маршрутизации R1.

```sh
[R1]display ip routing-table 
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 13       Routes : 13       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.2/32  Static  60   0          RD   10.0.12.2       GigabitEthernet
0/0/1
       10.0.1.3/32  Static  60   0          RD   10.0.13.3       GigabitEthernet
0/0/0
      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Проверьте возможность установления связи.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Request time out
    Request time out
    Request time out
    Request time out
    Request time out

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    0 packet(s) received
    100.00% packet loss
```

#### На R2 добавьте маршрут к интерфейсу LoopBack0 маршрутизатора Р1.

```sh
[R2]ip route-static 10.0.1.1 32 10.0.12.1
```

#### Проверьте возможность установления связи.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=255 time=10 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=255 time=30 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 10/22/30 ms

[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=50 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/28/50 ms
```

#### Настройте другие необходимые маршруты.

```sh
[R2]ip route-static 10.0.1.3 32 10.0.23.3
[R3]ip route-static 10.0.1.1 32 10.0.13.1
[R3]ip route-static 10.0.1.2 32 10.0.23.2
```

#### Проверьте возможность установления связи между интерфейсами LoopBack маршрутизаторов, следуя приведённой процедуре.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=40 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=255 time=10 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=255 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=255 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 10/26/40 ms

[R1]ping -a 10.0.1.1 10.0.1.3
  PING 10.0.1.3: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.3: bytes=56 Sequence=1 ttl=255 time=40 ms
    Reply from 10.0.1.3: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.3: bytes=56 Sequence=3 ttl=255 time=40 ms
    Reply from 10.0.1.3: bytes=56 Sequence=4 ttl=255 time=30 ms
    Reply from 10.0.1.3: bytes=56 Sequence=5 ttl=255 time=10 ms

  --- 10.0.1.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 10/30/40 ms

[R2]ping -a 10.0.1.2 10.0.1.1
  PING 10.0.1.1: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.1: bytes=56 Sequence=1 ttl=255 time=30 ms
    Reply from 10.0.1.1: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.1: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.1: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.1.1: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 10.0.1.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/24/30 ms

[R2]ping -a 10.0.1.2 10.0.1.3
  PING 10.0.1.3: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.3: bytes=56 Sequence=1 ttl=255 time=60 ms
    Reply from 10.0.1.3: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.3: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.3: bytes=56 Sequence=4 ttl=255 time=30 ms
    Reply from 10.0.1.3: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 10.0.1.3 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/32/60 ms

[R3]ping -a 10.0.1.3 10.0.1.1
  PING 10.0.1.1: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.1: bytes=56 Sequence=1 ttl=255 time=20 ms
    Reply from 10.0.1.1: bytes=56 Sequence=2 ttl=255 time=20 ms
    Reply from 10.0.1.1: bytes=56 Sequence=3 ttl=255 time=40 ms
    Reply from 10.0.1.1: bytes=56 Sequence=4 ttl=255 time=30 ms
    Reply from 10.0.1.1: bytes=56 Sequence=5 ttl=255 time=20 ms

  --- 10.0.1.1 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/26/40 ms

[R3]ping -a 10.0.1.3 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=40 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=255 time=10 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=255 time=30 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 10/24/40 ms
```

### Шаг 6: Настройте маршрут от R1 к R2 через R3 в качестве резервного маршрута от LoopBack0 R1 к LoopBack0 R2.
#### Настройте статические маршруты на R1 и R2.

```sh
[R1]ip route-static 10.0.1.2 32 10.0.13.3 preference 100
[R2]ip route-static 10.0.1.1 32 10.0.23.3 preference 100
```

#### Выведите таблицы маршрутизации R1 и R2.

```sh
[R1]display ip routing-table 
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 13       Routes : 13       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.2/32  Static  60   0          RD   10.0.12.2       GigabitEthernet
0/0/1
       10.0.1.3/32  Static  60   0          RD   10.0.13.3       GigabitEthernet
0/0/0
      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0

[R2]display ip routing-table 
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 13       Routes : 13       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Static  60   0          RD   10.0.12.1       GigabitEthernet
0/0/1
       10.0.1.2/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.3/32  Static  60   0          RD   10.0.23.3       GigabitEthernet
0/0/2
      10.0.12.0/24  Direct  0    0           D   10.0.12.2       GigabitEthernet
0/0/1
      10.0.12.2/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.23.0/24  Direct  0    0           D   10.0.23.2       GigabitEthernet
0/0/2
      10.0.23.2/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/2
    10.0.23.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/2
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Отключите интерфейс GigabitEthernet0/0/3 на маршрутизаторах R1 и R2, чтобы сделать недействительным маршрут с наивысшим приоритетом.

```sh
[R1]interface GigabitEthernet0/0/1
[R1-GigabitEthernet0/0/1]shutdown 
Sep 13 2021 00:19:26-08:00 R1 %%01IFPDT/4/IF_STATE(l)[4]:Interface GigabitEthern
et0/0/1 has turned into DOWN state.
quit
```

#### Выведите на экран таблицы маршрутизации на R1 и R2. Из командного вывода видно, что маршруты с более низким приоритетом активируется, когда маршруты с более высоким приоритетом становится недействительными.

```sh
[R1]display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 10       Routes : 10       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.2/32  Static  100  0          RD   10.0.13.3       GigabitEthernet
0/0/0
       10.0.1.3/32  Static  60   0          RD   10.0.13.3       GigabitEthernet
0/0/0
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0

[R2]display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 10       Routes : 10       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Static  100  0          RD   10.0.23.3       GigabitEthernet
0/0/2
       10.0.1.2/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.3/32  Static  60   0          RD   10.0.23.3       GigabitEthernet
0/0/2
      10.0.23.0/24  Direct  0    0           D   10.0.23.2       GigabitEthernet
0/0/2
      10.0.23.2/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/2
    10.0.23.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/2
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Проверьте возможность установления связи.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=254 time=40 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=254 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=254 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=254 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=254 time=30 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/30/40 ms
```

#### Выполните трассировку маршрута, по которому передаются пакеты данных.

```sh
[R1]tracert -a 10.0.1.1 10.0.1.2

 traceroute to  10.0.1.2(10.0.1.2), max hops: 30 ,packet length: 40,press CTRL_C
 to break 

 1 10.0.13.3 20 ms  10 ms  10 ms 

 2 10.0.23.2 30 ms  20 ms  20 ms 
```

### Шаг 7: Настройте маршруты по умолчанию для установления связи между интерфейсом LoppBack0 маршрутизатора R1 и интерфейсом LoopBack0 маршрутизатора R2.
#### Включите интерфейса и удалите настроенные маршруты.

```sh
[R1]interface GigabitEthernet0/0/1
[R1-GigabitEthernet0/0/1]undo shutdown
Sep 13 2021 00:23:11-08:00 R1 %%01IFPDT/4/IF_STATE(l)[6]:Interface GigabitEthern
et0/0/1 has turned into UP state.
[R1]
Sep 13 2021 00:23:11-08:00 R1 %%01IFNET/4/LINK_STATE(l)[7]:The line protocol IP 
on the interface GigabitEthernet0/0/1 has entered the UP state. 
[R1-GigabitEthernet0/0/1]quit 
[R1]undo ip route-static 10.0.1.2 255.255.255.255 10.0.12.2
[R1]undo ip route-static 10.0.1.2 255.255.255.255 10.0.13.3 preference 100
```

#### Выведите таблицу маршрутизации R1.

```sh
[R1]display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 12       Routes : 12       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.3/32  Static  60   0          RD   10.0.13.3       GigabitEthernet
0/0/0
      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Настройте маршрут по умолчанию на R1.

```sh
[R1]ip route-static 0.0.0.0 0 10.0.12.2
```

#### Выведите таблицу маршрутизации R1.

```sh
[R1]display ip routing-table 
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 13       Routes : 13       

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

        0.0.0.0/0   Static  60   0          RD   10.0.12.2       GigabitEthernet
0/0/1
       10.0.1.1/32  Direct  0    0           D   127.0.0.1       LoopBack0
       10.0.1.3/32  Static  60   0          RD   10.0.13.3       GigabitEthernet
0/0/0
      10.0.12.0/24  Direct  0    0           D   10.0.12.1       GigabitEthernet
0/0/1
      10.0.12.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
    10.0.12.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/1
      10.0.13.0/24  Direct  0    0           D   10.0.13.1       GigabitEthernet
0/0/0
      10.0.13.1/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
    10.0.13.255/32  Direct  0    0           D   127.0.0.1       GigabitEthernet
0/0/0
      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
127.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
255.255.255.255/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

#### Проверьте наличие связи между LoppBack0 маршрутизатора R1 и LoppBack0 маршрутизатора R2.

```sh
[R1]ping -a 10.0.1.1 10.0.1.2
  PING 10.0.1.2: 56  data bytes, press CTRL_C to break
    Reply from 10.0.1.2: bytes=56 Sequence=1 ttl=255 time=40 ms
    Reply from 10.0.1.2: bytes=56 Sequence=2 ttl=255 time=30 ms
    Reply from 10.0.1.2: bytes=56 Sequence=3 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=4 ttl=255 time=20 ms
    Reply from 10.0.1.2: bytes=56 Sequence=5 ttl=255 time=30 ms

  --- 10.0.1.2 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 20/28/40 ms
```