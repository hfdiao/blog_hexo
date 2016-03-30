title: '[RT] Using stunnel to telnet into GMail IMAP'  
date: 2011-10-17 21:58:03  
tags:  
- ssl  
- stunnel  
- telnet  
- imap  
---  

PS: [原文](http://wiredbytes.com/node/8)被墙， 转来方便墙内翻阅。 最近在搞IMAP相关的东西， 一直头疼不知道怎么命令行下测gmail的一些行为， 这篇文章真是帮大忙了。  

<pre>
By edwin - Posted on 12 February 2009

Here is a case study of how stunnel can be used to test an SSL based protocol. We will create an stunnel configuration that reroutes the IMAP port (TCP 143) to the Secure IMAP port (TCP 993) on GMail's IMAP server (imap.gmail.com). We will than test the setup by using telnet.

I will be using Ubuntu 8.10 (Intrepid Ibex).

First, let's install stunnel.

sudo apt-get install stunnel

Edit /etc/default/stunnel4, change ENABLED=0 to ENABLED=1

Edit /etc/stunnel/stunnel.conf as shown in the example below:

; Sample stunnel configuration file by Michal Trojnara 2002-2006
; Some options used here may not be adequate for your particular configuration
; Please make sure you understand them (especially the effect of chroot jail)

; Certificate/key is needed in server mode and optional in client mode
;cert = /etc/stunnel/mail.pem
;key = /etc/stunnel/mail.pem

; Protocol version (all, SSLv2, SSLv3, TLSv1)
sslVersion = SSLv3

; Some security enhancements for UNIX systems - comment them out on Win32
chroot = /var/lib/stunnel4/
setuid = stunnel4
setgid = stunnel4
; PID is created inside chroot jail
pid = /stunnel4.pid

; Some performance tunings
socket = l:TCP_NODELAY=1
socket = r:TCP_NODELAY=1
;compression = rle

; Workaround for Eudora bug
;options = DONT_INSERT_EMPTY_FRAGMENTS

; Authentication stuff
;verify = 2
; Don't forget to c_rehash CApath
; CApath is located inside chroot jail
;CApath = /certs
; It's often easier to use CAfile
;CAfile = /etc/stunnel/certs.pem
; Don't forget to c_rehash CRLpath
; CRLpath is located inside chroot jail
;CRLpath = /crls
; Alternatively you can use CRLfile
;CRLfile = /etc/stunnel/crls.pem

; Some debugging stuff useful for troubleshooting
debug = 7
output = /var/log/stunnel4/stunnel.log

; Use it for client mode
client = yes

; Service-level configuration

;[pop3s]
;accept = 995
;connect = 110

[imaps]
accept = 143
connect = imap.gmail.com:993

;[ssmtp]
;accept = 465
;connect = 25

;[https]
;accept = 443
;connect = 80
;TIMEOUTclose = 0

; vim:ft=dosini

Start up Stunnel

sudo /etc/init.d/stunnel4 start

Verify that the IMAP is listening on the local server.

netstat -an | grep -iw LISTEN
tcp 0 0 0.0.0.0:143 0.0.0.0:* LISTEN

The following requires that your GMail account have IMAP enabled. This is not enabled by default. Replace username@gmail.com with your real email address. Replace password with your real password.

telnet localhost 143
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
* OK Gimap ready for requests from 71.65.199.7 c5if2789008nfi.67
)
01 LOGIN username@gmail.com password
01 OK username@gmail.com authenticated (Success)
02 LOGOUT
* BYE LOGOUT Requested
02 OK 73 good day (Success)
Connection closed by foreign host.

That's it. If you're feeling adventourous you can use Hydra to brute force an account you own.

./hydra -l yourfriend@gmail.com -P password.txt -V localhost imap
</pre>
