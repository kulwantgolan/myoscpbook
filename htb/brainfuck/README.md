# 10.10.10.17
1. Initial Scan

```bash
root@kali:~/playground/brainfuck# nmap -sC -sC -oA nmap 10.10.10.17
Starting Nmap 7.80 ( https://nmap.org ) at 2020-07-01 19:29 EDT
Nmap scan report for brainfuck.htb (10.10.10.17)
Host is up (0.29s latency).
Not shown: 995 filtered ports
PORT    STATE SERVICE
22/tcp  open  ssh
| ssh-hostkey: 
|   2048 94:d0:b3:34:e9:a5:37:c5:ac:b9:80:df:2a:54:a5:f0 (RSA)
|   256 6b:d5:dc:15:3a:66:7a:f4:19:91:5d:73:85:b2:4c:b2 (ECDSA)
|_  256 23:f5:a3:33:33:9d:76:d5:f2:ea:69:71:e3:4e:8e:02 (ED25519)
25/tcp  open  smtp
|_smtp-commands: brainfuck, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
110/tcp open  pop3
|_pop3-capabilities: SASL(PLAIN) USER AUTH-RESP-CODE RESP-CODES TOP UIDL CAPA PIPELINING
143/tcp open  imap
|_imap-capabilities: have listed ENABLE LITERAL+ LOGIN-REFERRALS IDLE ID Pre-login AUTH=PLAINA0001 capabilities SASL-IR post-login OK IMAP4rev1 more
443/tcp open  https
|_http-title: Brainfuck Ltd. &#8211; Just another WordPress site
| ssl-cert: Subject: commonName=brainfuck.htb/organizationName=Brainfuck Ltd./stateOrProvinceName=Attica/countryName=GR
| Subject Alternative Name: DNS:www.brainfuck.htb, DNS:sup3rs3cr3t.brainfuck.htb
| Not valid before: 2017-04-13T11:19:29
|_Not valid after:  2027-04-11T11:19:29
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
| tls-nextprotoneg: 
|_  http/1.1

Nmap done: 1 IP address (1 host up) scanned in 62.84 seconds

```
![nmap result](_images/nmap.png)

  

2. We see 443 is open (Access to website over https) plus there is email server related ports open
    - We Interogate the certificate
![email](_images/email.png)
    
    `oretis@brainfuck.htb`

3. From nmap as well as from certificates we know CN(common names) and SAN (Subject alternative name). Added that to host file.

```
echo "10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb" >> /etc/hosts
```





##### Failed attempts
> Want to access the website https://brainfuck.htb from the windows host machine instead of the VM running Kali (which got VPN connection to htb)


- Set up SSH tunnel ? Failed :(
    + Add the CN(common name) and SAN(subject alternative name) from nmap result in `/etc/hosts` file
     `10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb`
    + Port forward to local machine: `ssh -L 443:10.10.10.17:443  kali@10.10.14.18`
    + On windows host machine edit the hosts file `c:\Windows\System32\Drivers\etc\hosts`
    `10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb`
    + Access in windows host machine URL: https://www.brainfuck.htb https://sup3rs3cr3t.brainfuck.htb https://brainfuck.htb
    
- Set up `Reverse SSH` tunnel. Worked :)
    + MY PC(WINDOWS HOST) ---> (KALI VM) NIC1(NAT,eth0):192.168.75.135 VPN(tun0):10.10.14.18  (and a route to 10.10.10.1 via 10.10.14.1) 
    + Can access https://10.10.10.17 on KALI VM but **Want to access https://10.10.10.17 on MY PC(WINDOWS HOST)**
    + Create a SSH tunnel with  Forwarding port: source port `5432` and a `Dynamic` destination port <using putty>
    + Set up SOCK5 proxy 127.0.0.1:5432 and also turn on DNS uing SOCK5 proxy <using foxy proxy plugin for firefox>
    + Now can access the website accessible form other PC
    
![ssh](_images/ssh.png)
![forwarded port](_images/forwardedports.png)
![socks](_images/socks.png)
![dns](_images/dns.png)

> Now can access `https://10.10.10.17` or `https://www.brainfuck.htb` on Host machine



