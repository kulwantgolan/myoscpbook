# 10.10.10.17
- Initial Scan

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

  

- We see 443 is open. Access to website over https. plus there is email server related ports open
    - We Interogate the certificate
![email](_images/email.png)
    
    `oretis@brainfuck.htb`
    

```
echo "10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb" >> /etc/hosts
```

##### Failed attempts
> want to access the website https://brainfuck.htb from the windows host machine instead of the VM running Kali (which got VPN connection to htb)

- Set up SSL tunnel ? Fai
    + Add the CN(common name) and SAN(subject alternative name) from nmap result in `/etc/hosts` file
     `10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb`
    + Port forward to local machine: `ssh -L 443:10.10.10.17:443  kali@10.10.14.18`
    + On windows host machine edit the hosts file `c:\Windows\System32\Drivers\etc\hosts`
    `10.10.10.17 www.brainfuck.htb sup3rs3cr3t.brainfuck.htb brainfuck.htb`
    + Access in windows host machine URL: https://www.brainfuck.htb https://sup3rs3cr3t.brainfuck.htb https://brainfuck.htb
