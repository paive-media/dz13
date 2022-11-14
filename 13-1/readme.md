# Домашнее задание к занятию 13.1 "Уязвимости и атаки на информационные системы"

------

### Задание 1.

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/

Это типовая ОС для экспериментов в области информационно безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту ВМ, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена данная виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?
- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно 3х уязвимостей)


---

Команда итоговая, с версиями ПО
```sh
nmap -v -A -T5 -sV -Pn 192.168.1.70
# или без подробностей/ без "A - агрессивного"
nmap -v -T5 -sV -Pn 192.168.1.70
```

Разрешенные службы: 
```sh
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     2 (RPC #100000)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```


Найденные уязвимости из списка: 

[vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)](https://www.exploit-db.com/exploits/17491)

[Apache 1.4/2.2.x - APR 'apr_fnmatch()' Denial of Service](https://www.exploit-db.com/exploits/35738)

[Apache cocoon 2.14/2.2 - Directory Traversal](https://www.exploit-db.com/exploits/23282)

[ProFTPd IAC 1.3.x - Remote Command Execution](https://www.exploit-db.com/exploits/15449)

[PostgreSQL 8.2/8.3/8.4 - UDF for Command Execution](https://www.exploit-db.com/exploits/7855)

[PostgreSQL 8.3.6 - Low Cost Function Information Disclosure](https://www.exploit-db.com/exploits/32847)

[PostgreSQL 8.3.6 - Conversion Encoding Remote Denial of Service](https://www.exploit-db.com/exploits/32849)

[VNC Keyboard - Remote Code Execution (Metasploit)](https://www.exploit-db.com/exploits/37598)


---


### Задание 2.

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

---

Сканирование в разных режимах: 

[Источник: документация nmap](https://nmap.org/man/ru/man-port-scanning-techniques.html)


- **SYN**
  - `nmap -sS -sV -T4 -v --reason 192.168.1.70`
    - самый быстрый
  	- принцип полуоткрытых соединений, т.е. только ответ на первое "рукопожатие" (отправка SYN)
  	- сервер отвечает активно, полно и быстро
```sh
Nmap done: 1 IP address (1 host up) scanned in 11.70 seconds

Host is up, received arp-response (0.0014s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE     REASON         VERSION
21/tcp   open  ftp         syn-ack ttl 64 vsftpd 2.3.4
22/tcp   open  ssh         syn-ack ttl 64 OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      syn-ack ttl 64 Linux telnetd
25/tcp   open  smtp        syn-ack ttl 64 Postfix smtpd
53/tcp   open  domain      syn-ack ttl 64 ISC BIND 9.4.2
80/tcp   open  http        syn-ack ttl 64 Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp  open  rpcbind     syn-ack ttl 64 2 (RPC #100000)
139/tcp  open  netbios-ssn syn-ack ttl 64 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 64 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        syn-ack ttl 64 netkit-rsh rexecd
513/tcp  open  login       syn-ack ttl 64
514/tcp  open  tcpwrapped  syn-ack ttl 64
1099/tcp open  java-rmi    syn-ack ttl 64 GNU Classpath grmiregistry
1524/tcp open  bindshell   syn-ack ttl 64 Metasploitable root shell
2049/tcp open  nfs         syn-ack ttl 64 2-4 (RPC #100003)
2121/tcp open  ftp         syn-ack ttl 64 ProFTPD 1.3.1
3306/tcp open  mysql       syn-ack ttl 64 MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  syn-ack ttl 64 PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc         syn-ack ttl 64 VNC (protocol 3.3)
6000/tcp open  X11         syn-ack ttl 64 (access denied)
6667/tcp open  irc         syn-ack ttl 64 UnrealIRCd
8009/tcp open  ajp13       syn-ack ttl 64 Apache Jserv (Protocol v1.3)
8180/tcp open  http        syn-ack ttl 64 Apache Tomcat/Coyote JSP engine 1.1

```
- **FIN**
  - `nmap -sF -sV -T4 -v --reason 192.168.1.70`
    - как sS но с другими флагами TCP = только TCP FIN бит
    - сервер отвечает активно, полно и быстро
- **Xmas**
  - `nmap -sX -sV -T4 -v --reason 192.168.1.70`
    - как sS но с другими флагами TCP = FIN, PSH и URG флаги
    - сервер отвечает активно, полно и быстро
- **UDP**
  - `nmap -sU -sV -T4 -v --reason 192.168.1.70`
    - перебор портов UDP 
    - почти все порты закрыты (ICMP ответ)
    - редкие SMB / UDP (66,137,138 и др.) открытые порты, специфических служб (NetBIOS в основном)
    - медленная скорость работы
    - сервер отвечат медленно, в основном отбивками о недоступности порта
    - из 1000 портов, которые проверял более 20 минут, открытыми только 19 обнаружились
```sh
Nmap done: 1 IP address (1 host up) scanned in 1149.12 seconds

Host is up, received arp-response (0.00098s latency).
Not shown: 981 closed udp ports (port-unreach)
PORT      STATE         SERVICE        REASON              VERSION
53/udp    open          domain         udp-response ttl 64 ISC BIND 9.4.2
68/udp    open|filtered dhcpc          no-response
69/udp    open|filtered tftp           no-response
111/udp   open          rpcbind        udp-response ttl 64
137/udp   open          netbios-ns     udp-response ttl 64 Samba nmbd netbios-ns (workgroup: WORKGROUP)
138/udp   open|filtered netbios-dgm    no-response
643/udp   open|filtered sanity         no-response
685/udp   open|filtered mdc-portmapper no-response
1035/udp  open|filtered mxxrlogin      no-response
1718/udp  open|filtered h225gatedisc   no-response
2049/udp  open          rpcbind        udp-response ttl 64
8000/udp  open|filtered irdmi          no-response
19227/udp open|filtered unknown        no-response
19728/udp open|filtered unknown        no-response
28973/udp open|filtered unknown        no-response
48255/udp open|filtered unknown        no-response
55587/udp open|filtered unknown        no-response
63420/udp open|filtered unknown        no-response
64590/udp open|filtered unknown        no-response
```

---
