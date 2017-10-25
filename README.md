Tugas 1 PKSJ
Penetrasi
===================
-------------

## Daftar isi
- [Pendahuluan](#pendahuluan)
- [Dasar Teori](#dasar-teori)
- [Instalasi Server](#instalasi-server)
- [Instalasi Desktop Ubuntu](#instalasi-desktop-ubuntu)
- [Langkah-langkah Penetrasi](#langkah-langkah-penetrasi)
  - [Ncrack](#crack)
  - [Hydra](#ydra)
  
----------

Pendahuluan
----------

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

StackEdit stores your documents in your browser, which means all your documents are automatically saved locally and are accessible **offline!**

> **Note:**

> - StackEdit is accessible offline after the application has been loaded for the first time.

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

Instalasi Server
----------

----------

Instalasi Desktop Ubuntu
----------

----------

Langkah-langkah Penetrasi
----------
Ncrack
Hydra


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
Sebelum ada user yang terblokir, fail2ban membuat rule di iptables seperti berikut
```
$ sudo iptables -S

-P INPUT ACCEPT
-P FORWARD DROP
-P OUTPUT ACCEPT
...
-N f2b-sshd
-A INPUT -p tcp -m multiport --dports 22 -j f2b-sshd
...
-A f2b-sshd -j RETURN
```

Ketika server telah dipasang fail2ban. Salah seorang client mencoba untuk diakses namun gagal sekian kalo, maka fail2ban akan menulis rule di iptables berupa

```
$ sudo iptables -S

-P INPUT ACCEPT
-P FORWARD DROP
-P OUTPUT ACCEPT
...
-N f2b-sshd
-A INPUT -p tcp -m multiport --dports 22 -j f2b-sshd
...
-A f2b-sshd -s 10.151.36.5/32 -j REJECT --reject-with icmp-port-unreachable
-A f2b-sshd -j RETURN
```

Pada rule diatas, fail2ban memblokir client dengan IP 10.151.36.5.

Jika dilihat dari file log /var/log/fail2ban.log, maka dapat dilihat di baris terakhir,
```
$ tail /var/log/fail2ban.log

2017-10-25 04:54:22,297 fail2ban.filter         [29305]: INFO    [sshd] Found 10.151.36.5
2017-10-25 04:54:24,375 fail2ban.filter         [29305]: INFO    [sshd] Found 10.151.36.5
2017-10-25 04:54:31,918 fail2ban.filter         [29305]: INFO    [sshd] Found 10.151.36.5
2017-10-25 04:54:32,853 fail2ban.actions        [29305]: NOTICE  [sshd] Ban 10.151.36.5
```

Ketika client 10.151.36.5 mengakses lagi dengan ssh, maka ia akan gagal dan mengeluarkan output, berikut

```
$ ssh administrator@10.151.36.22

ssh: connect to host 10.151.36.22 port 22: Connection refused
```

