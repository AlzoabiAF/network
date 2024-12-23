# DO2_LinuxNetwork

## Part 1. Инструмент ipcalc
**1. Сети и маски** 
- **Адрес сети 192.167.38.54/13:**  
```ipcalc 192.167.38.54/13```  
![images](images/part1/1.png)  
- **Перевод маски 255.255.255.0 в префиксную и двоичную запись, /15 в обычную и двоичную, 11111111.11111111.11111111.11110000 в обычную и префиксную:**  
```ipcalc 192.168.1.1 255.255.255.0```  
![images](images/part1/2.png)  
```ipcalc 192.168.1.1/15```  
![images](images/part1/3.png)  
- **Минимальный и максимальный хост в сети 12.167.38.4 при масках: /8, 11111111.11111111.00000000.00000000, 255.255.254.0 и /4:**  
```ipcalc 12.167.38.4/8```  
![images](images/part1/4.png)  
```ipcalc 12.167.38.4/16```   
16 = 11111111.11111111.00000000.00000000   
![images](images/part1/5.png)  
![images](images/part1/6.png)  
```ipcalc 12.167.38.4/255.255.254.0```  
![images](images/part1/7.png)  
```ipcalc 12.167.38.4/4```   
![images](images/part1/8.png)  

**2. localhost**  

- Локальные сети IPv4:  
10.0.0.0/8  
![images](images/part1/9.png)  
172.16.0.0/12  
![images](images/part1/10.png)  
192.168.0.0/16  
![images](images/part1/11.png)  
- Loopback:  
127.0.0.0/8  
![images](images/part1/12.png) 
- Проверим IP на localhost:  
    1. **194.34.23.100**  - не является, не входит ни в одну зону  
    2. **127.0.0.2**  - является, входит в зону в 127.0.0.0/8  
    3. **127.1.0.1**  - является, входит в зону в 127.0.0.0/8  
    4. **128.0.0.1**  - не является, не входит ни в одну зону  

**3. Диапазоны и сегменты сетей**  
- Минимальный и максимальный хост у сети **10.10.0.0/18**:  
![images](images/part1/13.png)  
- Проверим каждый возможный ip на шлюз сети **10.10.0.0/18**:  
    1. **10.0.0.1** - не является, первый октет подходит, а второй нет 10 != 0  
    2. **10.10.0.2** - является
    3. **10.10.10.10** - является
    4. **10.10.100.1** - не является, первые два октета подходят, а третий не входит в сеть, 0 < 63 < 100
    5. 10.10.1.255 - является

## Part 2. Статическая маршрутизация между двумя машинами
**Посмотрим существующие сетевые интерфейсы у ws1 и ws2:**  
- **ws1**  
![images](images/part2/1.png)  
- **ws2**  
![images](images/part2/2.png)  
**Изменим интерфейсы на обеих машинах:**  
- **ws1**  
![images](images/part2/3.png)  
- ws2  
![images](images/part2/4.png)  
**Выполним команду ```netplan apply``` для перезапуска сервиса сети:**  
- **ws1**  
![images](images/part2/5.png)  
- **ws2**  
![images](images/part2/6.png)   

**1. Добавление статического маршрута вручную и пингование между машинами:**  
- **ws1**  
![images](images/part2/7.png)  
- **ws2**  
![images](images/part2/8.png)  

**2. Добавление статического маршрута с сохранением**  
1. **Изменяем конфигурационный файл:**  
- **ws1**  
![images](images/part2/9.png)  
- **ws2**  
![images](images/part2/10.png)  
**1. Перезагружаем машины с помощью ```reboot``` и пингуем:**  
- **ws1**  
![images](images/part2/11.png)  
- **ws2**  
![images](images/part2/12.png)   


## Part 3. Утилита iperf3  
**1. Перевод скорости:**  
- 8 Mbps в MB/s: 8 / 8 = 1 MB/s  
- 100 MB/s в Kbps: 100 * 8 * 1000 = 800000 Kbps  
- 1 Gbps в Mbps: 1 * 1000 = 1000 Mbps

**2. Утилита iperf3:**
- Запуск сервера на ws2:  
![images](images/part3/1.png)    
- Запуск тестового соединения с помощью клиента на ws1:  
![images](images/part3/2.png)  

## Part 4. Сетевой экран
### **1. Утилита iptables**  
1. **Пропишем правила в файл, иметирующий фаервол**:
- ws1  
![images](images/part4/1.png)  
- ws2  
![images](images/part4/2.png)  
2. **Делаем файл исполняемым и запускаем его:**
- ws1  
![images](images/part4/3.png)  
- ws2  
![images](images/part4/4.png)  

### **2. Утилита nmap**  
1. **Проверка на пинг:**
- **с ws1 на ws2**  
![images](images/part4/5.png)  
- **с ws2 на ws1**  
![images](images/part4/6.png)  
2. **Проверка, что хост ws1 запущен:**  
![images](images/part4/7.png)  

## Part 5. Статическая маршрутизация сети

### 1. Настройка адресов машин
- **r1**  
![images](images/part5/r1.png)  
**interfaces:**  
![images](images/part5/r1_int.png)  

- **r2**  
![images](images/part5/r2.png)  
**interfaces:**  
![images](images/part5/r2_int.png)  

- **ws11**  
![images](images/part5/ws11.png)  
**interfaces:**  
![images](images/part5/ws11_int.png)  

- **ws21**  
![images](images/part5/ws21.png)  
**interfaces:**  
![images](images/part5/ws21_int.png)  

- **ws22**  
![images](images/part5/ws22.png)  
**interfaces:**  
![images](images/part5/ws22_int.png)  

- **ping ws22 с ws21**  
![images](images/part5/ping_ws22_with_ws21.png)  

- **ping r1 с ws11**  
![images](images/part5/ping_r1_with_ws11.png)  

### 2. Включение переадресации IP-адресов
- **r1**  
**Применение команды ```sysctl -w net.ipv4.ip_forward=1```:**  
![images](images/part5/r1_sysctl_ip_forward.png)  
**Содержимое файла /etc/sysctl.conf:**  
![images](images/part5/r1_sysctl_file.png)  

- **r2**  
**Применение команды ```sysctl -w net.ipv4.ip_forward=1```:**  
![images](images/part5/r2_sysctl_ip_forward.png)  
**Содержимое файла /etс/sysctl.conf:**  
![images](images/part5/r2_sysctl_file.png)  

### 3. Установка маршрута по умолчанию  
- **ws11**  
![images](images/part5/ws11_ip_r.png)  
- **ws21**  
![images](images/part5/ws21_ip_r.png)  
- **ws22**  
![images](images/part5/ws22_ip_r.png)  

- **ping ws11 с r2**  
![images](images/part5/ping_r2_with_ws11.png)  
- **Проверка, что пинг доходит**  
![images](images/part5/ping_r2_with_ws11_tcpdump.png)  

### 4. Добавление статических маршрутов
**Проверяем наличие статических маршрутов**  
- **r1**   
![images](images/part5/r1_ip_r.png)  
- **r2**  
![images](images/part5/r2_ip_r.png)  

### 5. Построение списка маршрутизаторов  
**Запустим на r1 команду ```tcpdump -tnv -i eth0```:**  
![images](images/part5/r1_tcpdump1.png)  
**При помощи утилиты traceroute построим список маршрутизаторов на пути от ws11 до ws21**  
![images](images/part5/traceroute_ws21_with_ws11.png)  

**traceroute** работает по принципу от udp пакетов с увеличением внутри них ttl после каждого хопа до тех пор, пока не дойдет до места назначения. Если не может достигнуть своей цели, он ttl будет 0 и закончит отправку пакетов, возвращая в ответ ***  

### 6. Использование протокола ICMP при маршрутизации  
**Перехватываем сетевой трафик на r1 c помощью команды ```tcpdump -n -i eth0 icmp```:**  
![images](images/part5/r1_tcpdump2.png)  
**Пингуем несуществующий ip с ws11:**  
![images](images/part5/ping_unknown_ip_with_ws11.png)  


## Part 6. Динамическая настройка IP с помощью DHCP

### 1. **Настройка r2**  
**Пропишим правила выдачи ip в dhcpd.conf:**   
![images](images/part6/r2_dhcpd_config.png)  

**resolv.conf пропиши nameserver 8.8.8.8:**  
![images](images/part6/r2_resolv_config.png)  

**Перезагрузим сервис dhcp-server:**  
![images](images/part6/r2_restart_dhcp_service.png)  

**Проверим выдачу ip для ws21:**  
![images](images/part6/ws21_ip_a.png)  

**Пропингуем ws22 c ws21**  
![images](images/part6/ping_ws22_with_ws21.png)

### 2. **Настройка r1**
**Изменим mac-адрес ws11:**  
![images](images/part6/ws11_config_file.png)  

**Пропишим правила выдачи ip в dhcpd.conf:**   
![images](images/part6/r1_dhcpd_config.png)   

**resolv.conf пропиши nameserver 8.8.8.8:**  
![images](images/part6/r1_resolv_config.png)  

**Перезагрузим сервис dhcp-server:**  
![images](images/part6/r1_restart_dhcp_service.png)

**Проверим выдачу ip в ws11:**  
![images](images/part6/ws11_ip_a.png)

## Part 7. NAT  

1. **Cделаем сервер Apache2 общедоступным на ws22 и r1, изменив файл /etc/apache2/ports.conf:**  
- r1  
![images](images/part7/r1_apache2_ports_config.png)  
- ws22  
![images](images/part7/ws22_apache2_ports_config.png)  

2. **Запустим веб-сервер Apache командой ```service apache2 start``` на ws22 и r1:**  
- r1  
![images](images/part7/r1_apache2_start.png)  
- ws22  
![images](images/part7/ws22_apache2_start.png)  

3. **Добавим в фаервол правила в r2:**  
![images](images/part7/r2_firewall.png)  

4. **Проверим все правила**  
**PS:** надо было добавить только первые три правила и проверить, что r1 не пингуется с ws22:  
![images](images/part7/ping_r1_with_ws22_1.png)  
**После этого мы разрешили проходить пакеты icmp с помощью команды ```iptables -A FORWARD -p icmp -j ACCEPT``` и проверили, что пинг проходит:**  
![images](images/part7/ping_r1_with_ws22_2.png)  
**Проверим работу SNAT и DNAT с помощью telnet:**  
    - r1 с ws22  
    ![images](images/part7/telnet_r1_80.png)  
    - r2 с r1  
    ![images](images/part7/telnet_r2_8080.png)  
    - ws22 с r1  
    ![images](images/part7/telnet_ws22_80.png)  

## Part 8. Дополнительно. Знакомство с SSH Tunnels  

1. **Запустим на r2 фаервол с правилами:**  
![images](images/part8/r2_run_firewall.png)  

2. **Запусти веб-сервер Apache на ws22 только на localhost:**  
![images](images/part8/ws22_apache2_ports.png)  
![images](images/part8/ws22_apache2_restart.png)

3. **Подключимся по ssh c ws11:**  
![images](images/part8/ssh_ws22_with_ws11.png)  

4. **Подключимся по ssh c ws21:**  
![images](images/part8/ssh_ws22_with_ws21.png)  

5. **Для проверки, сработало ли подключение в обоих предыдущих пунктах, перейдем во второй терминал (например, клавишами Alt + F2) и выполнем команду:**  
![images](images/part8/check.png)  
