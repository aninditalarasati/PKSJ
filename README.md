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

A. Ncrack

Ncrack is a high-speed network authentication cracking tool. It was built to help companies secure their networks by proactively testing all their hosts and networking devices for poor passwords. Security professionals also rely on Ncrack when auditing their clients. Ncrack was designed using a modular approach, a command-line syntax similar to Nmap and a dynamic engine that can adapt its behaviour based on network feedback. It allows for rapid, yet reliable large-scale auditing of multiple hosts.

Ncrack's features include a very flexible interface granting the user full control of network operations, allowing for very sophisticated bruteforcing attacks, timing templates for ease of use, runtime interaction similar to Nmap's and many more. Protocols supported include RDP, SSH, HTTP(S), SMB, POP3(S), VNC, FTP, SIP, Redis, PostgreSQL, MySQL, and Telnet.

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

