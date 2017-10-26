Tugas 1 PKSJ
Penetrasi
===================
-------------

halo!
Pada tugas pertama ini, kami mencoba penetration testing.
aplikasi yang kami gunakan adalah hydra dan ncrack.
sistem yang diserang adalah sebuah server ubuntu.

## Daftar isi
- [Pendahuluan](#pendahuluan)
- [Dasar Teori](#dasar-teori)
- [Instalasi Server](#instalasi-server)
- [Instalasi Penetrator](#instalasi-penetrator)
- [Langkah-langkah Penetrasi](#langkah-langkah-penetrasi)
  - [Ncrack](#crack)
  - [Hydra](#ydra)
  
----------

Pendahuluan
----------
apa itu penetration testing?
penetration testing adalah simulasi serangan yang dilakukan terhadap suatu jaringan. 
hal ini dilakukan untuk menemukan kelemahan yang ada di dalam sebuah sistem.
-------------

Dasar Teori
----------

A. THC-Hydra

Hydra is a parallized login cracker which supports numerous protocols to attack. New modules are easy to add, beside that, it is flexible and very fast.

Hydra was tested to compile on Linux, Windows/Cygwin, Solaris 11, FreeBSD 8.1, OpenBSD, OSX, QNX/Blackberry, and is made available under GPLv3 with a special OpenSSL license expansion.

Currently this tool supports:
    Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-POST, HTTPS-HEAD, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, S7-300, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP, SOCKS5, SSH (v1 and v2), Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP.

For HTTP, POP3, IMAP and SMTP, several login mechanisms like plain and MD5 digest etc. are supported.

This tool is a proof of concept code, to give researchers and security consultants the possiblity to show how easy it would be to gain unauthorized access from remote to a system.

The program was written van Hauser and is additiionally supported by David Maciejak.
        
(Sumber*: https://www.thc.org/thc-hydra/)

* : Teridentifikasi oleh google chrome sebagai Dangerous

B. Ncrack

Ncrack merupakan *tool* pembobol autentikasi jaringan komputer berkecepatan tinggi. *Tool* ini dibuat untuk membantu perusahaan-perusahaan mengamankan jaringan komputer dengan secara proaktif menguji semua *host* dan perangkat jaringan apakah ada yang memiliki *password* lemah. Para ahli keamanan juga bergantung pada Ncrack ketika mengaudit *client* mereka. Ncrack didesain untuk menggunakan pendekatan modular, sintaks *command-line* yang mirip dengan Nmap, dan mesin yang dinamis sehingga bisa menyesuaikan perilakunya dengan respon dari jaringan. Ncrack menyediakan audit *multi-host* yang cepat tapi tetap dapat diandalkan (*reliable*).

Fitur-fitur pada Ncrack di antaranya interface yang sangat fleksibel sehingga memberikan kontrol penuh pada pengguna atas operasi-operasi jaringan, serangan bruteforce yang canggih, *timing template* untuk memudahkan penggunaan, *runtime interaction* yang mirip dengan Nmap, dan masih banyak lagi. Protokol-protokol yang didukung mencakup RDP, SSH, HTTP(S), SMB, POP3(S), VNC, FTP, SIP, Redis, PostgreSQL, MySQL, dan Telnet.

(https://nmap.org/ncrack/)

Instalasi Server
-------------

Langkah-langkah instalasi ubuntu server :


> **Note:**

> - buka aplikasi VirtualBox, klik New
> - masukkan konfigurasi yang sesuai dengan server ubuntu
> - tambahkan adapter untuk menghubungkan ubuntu server dan desktop(penetrator)
> - caranya dengan klik kanan di virtual machine->settings->network->adapter2->enable dengan attached to host-only adaptor
> - adapter 2 sudah terpasang
> - masukkan kaset ubuntu server
> - caranya buka settings->storage->masukkan disk->ok
> - start ubuntu server dan diinstall seperti biasa
> - tambahkan ip untuk ssh, sebagai penghubung ubuntu server dan penetrator.
> - caranya etc/network/interfaces
> sesuaikan nama interface dengan nama virtual network card. kami menggunakan enp0s8.

----------



Instalasi Penetrator
-------------------

StackEdit can be combined with <i class="icon-provider-gdrive"></i> **Google Drive** and <i class="icon-provider-dropbox"></i> **Dropbox** to have your documents saved in the *Cloud*. The synchronization mechanism takes care of uploading your modifications or downloading the latest version of your documents.

> **Note:**

> - Full access to **Google Drive** or **Dropbox** is required to be able to import any document in StackEdit. Permission restrictions can be configured in the settings.
> - Imported documents are downloaded in your browser and are not transmitted to a server.
> - If you experience problems saving your documents on Google Drive, check and optionally disable browser extensions, such as Disconnect.

#### <i class="icon-refresh"></i> Open a document

You can open a document from <i class="icon-provider-gdrive"></i> **Google Drive** or the <i class="icon-provider-dropbox"></i> **Dropbox** by opening the <i class="icon-refresh"></i> **Synchronize** sub-menu and by clicking **Open from...**. Once opened, any modification in your document will be automatically synchronized with the file in your **Google Drive** / **Dropbox** account.

#### <i class="icon-refresh"></i> Save a document

You can save any document by opening the <i class="icon-refresh"></i> **Synchronize** sub-menu and by clicking **Save on...**. Even if your document is already synchronized with **Google Drive** or **Dropbox**, you can export it to a another location. StackEdit can synchronize one document with multiple locations and accounts.

----------




Konfigurasi Countermeasure
---------
- Pemasangan fail2ban dapat dipasang dari repositori ubuntu. Pemasangan dapat dilakukan dengan `$ sudo apt-get install fail2ban`.

- Konfigurasi dari fail2ban terdapat pada direktori /etc/fail2ban. Disarankan untuk membuat file jail.local tersendiri untuk mengonfigurasi pengaturan. File jail.local ini akan mengonverwrite pengaturan yang terdapat pada jail.conf.

- Ganti durasi waktu banned selama 3 menit atau 180 second

```
bantime = 180

```

- Ganti jumlah percobaan sejumlah 3 kali.
```
maxretry = 3
```

- Pada dasarnya, fail2ban akan membuat rules pada wirewall menggunakan iptables. Dan rules akan ditulis ulang ketika terdapat client yang mencoba mengakses sekian kali sesuai konfigurasi yang ditulis. 


Testing
----------
Hydra dijalankan pada Ubuntu Desktop untuk brute force Ubuntu Server.

Berikut hasil dari logfile fail2ban `/var/log/fail2ban.log` pada Ubuntu Server:

```
$ sudo tail -f /var/log/fail2ban.log

2017-10-25 21:29:57,562 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,586 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,595 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,604 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,613 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,623 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,633 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,643 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,664 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,670 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,676 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,681 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,686 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,688 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,791 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,803 fail2ban.filter         [2811]: INFO    [sshd] Found 192.168.56.101
2017-10-25 21:29:57,925 fail2ban.actions        [2811]: NOTICE  [sshd] Ban 192.168.56.101
2017-10-25 21:29:58,136 fail2ban.actions        [2811]: NOTICE  [sshd] 192.168.56.101 already banned
2017-10-25 21:29:59,138 fail2ban.actions        [2811]: NOTICE  [sshd] 192.168.56.101 already banned
```
Berikut daftar rules dari iptables. Pada daftar rules dibawah terdapat rule untuk memblokir IP 192.168.56.101.

```
$ sudo iptables -S

-P INPUT ACCEPT
-P FORWARD ACCEPT
-P OUTPUT ACCEPT
-N f2b-sshd
-A INPUT -p tcp -m multiport --dports 22 -j f2b-sshd
-A f2b-sshd -s 192.168.56.101/32 -j REJECT --reject-with icmp-port-unreachable
-A f2b-sshd -j RETURN
```


Pada percobaan ini, meskipun dibatasi `maxretry = 3`, SSH Server masih menerima percobaan brute force sejumlah 16 kali. Hingga akhirnya fail2ban memblock attacker dengan delay 190 ms dari percobaan kelima.
