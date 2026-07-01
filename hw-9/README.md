# Домашнее задание к занятию «Уязвимости и атаки на информационные системы»

------

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
##### Ответ:
```bash
sfairath@DESKTOP-SQRCHMQ:/ubuntu/e/Обучение/homeworks/sysdb$ sudo nmap -sV 192.168.3.59
Starting Nmap 7.98 ( https://nmap.org ) at 2026-07-01 09:05 +0300
Stats: 0:00:27 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 95.65% done; ETC: 09:05 (0:00:01 remaining)
Nmap scan report for 192.168.3.59
Host is up (0.015s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE      VERSION
21/tcp   open  ftp          vsftpd 2.3.4
22/tcp   open  ssh          OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet       Linux telnetd
25/tcp   open  smtp         Postfix smtpd
53/tcp   open  domain       ISC BIND 9.4.2
80/tcp   open  http         Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind      2 (RPC #100000)
139/tcp  open  netbios-ssn  Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn  Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec         netkit-rsh rexecd
513/tcp  open  login
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi     GNU Classpath grmiregistry
1524/tcp open  bindshell    Metasploitable root shell
2049/tcp open  nfs          2-4 (RPC #100003)
2121/tcp open  ccproxy-ftp?
3306/tcp open  mysql        MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql   PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc          VNC (protocol 3.3)
6000/tcp open  X11          (access denied)
6667/tcp open  irc          UnrealIRCd
8009/tcp open  ajp13        Apache Jserv (Protocol v1.3)
8180/tcp open  http         Apache Tomcat/Coyote JSP engine 1.1
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 158.56 seconds
```

- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)
##### Ответ:
- открытый telnet порт 23
- [vsftpd 2.3.4](https://www.exploit-db.com/exploits/17491)
- [Samba 3.4.7/3.5.1 - Denial of Service](https://www.exploit-db.com/exploits/12588)

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

##### Ответ:
- [SYN](./wireshark/nmap%20-sS.pcapng) режим отправляет syn пакет и принимает ack или rst от открытого/закрытого порта. Запрос начала соединения
- [FIN](./wireshark/nmap%20-sF.pcapng) режим - отправляет запрос разрыва соединения. Если порт закрыт, то получим rst пакет, если открыт, то ответа не будет
- [Xmas](./wireshark/nmap%20-sX.pcapng) режим - отправляет пакеты с флагами FIN + PSH + URG. Если порт закрыт, то получим rst пакет, если открыт, то ответа не будет
- [UDP](./wireshark/nmap%20-sU.pcapng) режим - отправляет пакеты без флагов. Если порт открыт, ответа нет, если закрыт, то Port unreachable