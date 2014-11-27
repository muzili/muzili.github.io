---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

Getting a free certificate
==========================

This page guides you through the process of obtaining an HTTPS
certificate for your site. This is a real certificate, not a self-signed
certificate, and works in all major browsers.

The CA which we'll use is StartSSL. They provide basic certificates for
free, although will charge for other types, such as wildcard
certificates.

{{{more}}}

Generating a public/private keypair
-----------------------------------

A keypair can be generated with OpenSSL:

``` {.example}
    openssl req -new -newkey rsa:2048 -keyout example.com.key -nodes -out example.com.csr
```

This command will prompt you for a country name, state name etc. *All of
this can be ignored*. Just hit enter to accept the defaults because
StartSSL doesn't use that information.

The only value you may want to give is a passphrase to protect the
private key. Keep in mind that your webserver needs the private key so,
if you set a passphrase, you'll need to enter it whenever you restart
the webserver.

Now that we have a key, we can get a certificate issued by the CA.

StartSSL
--------

StartSSL is free, but it's not the best designed website in the world.
Below is a series of screenshots which will hopefully guide you through
the process. Not absolutely every step has a screenshot so do the
obvious thing at each step. If you need to click something then the
screenshot may have a red ring around the the target.

Firstly, *use Firefox*. Seriously.

Go to [StartSSL](https://startssl.com) (it should have an EV
certificate). On the frontpage, click “Control Panel” at the top-right
to get started.

We assume that you've never used StartSSL before so need to sign up:

![](../../../../images/startssl/startssl-signup.png) Enter your details:

![](../../../../images/startssl/startssl-account.png) You'll need to
verify the email address by entering the magic value that is sent to
you:

![](../../../../images/startssl/startssl-verify1.png) StartSSL doesn't
use passwords for accounts, it uses client-side certificates. You need
to generate one and install it in your browser:

![](../../../../images/startssl/startssl-genkey.png)

![](../../../../images/startssl/startssl-clientsidecert.png) Once your
account has been setup, you'll be at your "Control Panel". First you
need to prove that you control the site that you are getting a
certificate for. For free certificates, this means being able to accept
email for some distinguished usernames.

![](../../../../images/startssl/startssl-cp.png) You are validating a
domain name.

![](../../../../images/startssl/startssl-valtype.png) You must be able
to receive email for one of the distinguished usernames:

![](../../../../images/startssl/startssl-selemail.png) Wait for the
email and enter the code contained in it into the textbox.

![](../../../../images/startssl/startssl-code2.png) Once you have proved
that you own the domain, finished the validations wizard and enter the
certificate wizard.

![](../../../../images/startssl/startssl-valok.png) You want an HTTPS
certificate.

![](../../../../images/startssl/startssl-certtype.png) Select the domain
that you just validated.

![](../../../../images/startssl/startssl-seldomains.png) I strongly
recommend that you add a name for `www` to the certificate.

![](../../../../images/startssl/startssl-adddomains.png) This bit is
important. *Skip the private key generation.* You already generated a
private key at the very beginning. We're going to give StartSSL the
public key to sign and the private key should never leave your control.

![](../../../../images/startssl/startssl-genprivkey.png) The `openssl`
command that you ran at the beginning created two files. One of them was
a CSR file. You need to open this up and paste its contents into the
textbox. It's a plain ASCII file so almost anything should open it.

![](../../../../images/startssl/startssl-csr.png) Now we're almost done!
This is your signed certificate. Paste it somewhere safe. It's not
secret, but you don't want to lose it!

![](../../../../images/../../../../images/startssl/startssl-savecert.png)

Intermediate certificates
-------------------------

You must configure your webserver with the correct intermediate
certificate in order for your certificate to work. You can download
[StartSSL's intermediate CA
certificate](https://www.startssl.com/certs/sub.class1.server.ca.pem).

Deploy the SSL CA certification
===============================

Download the startssl CA chain
------------------------------

``` {.example}
wget http://www.startssl.com/certs/sub.class1.server.ca.pem -O /etc/pki/tls/certs/sub.class1.server.ca.pem
wget http://www.startssl.com/certs/ca.pem -O /etc/pki/tls/certs/startsslrootca.pem
wget http://www.startssl.com/certs/ca-bundle.pem -O /etc/pki/tls/certs/startssl-ca-bundle.pem
```

Remove the password for your private key
----------------------------------------

``` {.example}
openssl rsa -in example.com.key -out example.com.nopass.key
```

Copy the keys to server
-----------------------

### Put `example.com.key/example.com.nopass.key` to `/etc/pki/tls/private/`

### Put `example.com.crt` to `/etc/pki/tls/certs/`

Apache
------

### Configure the ssl

``` {.example}
SSLEngine on
SSLProxyEngine On
SSLProtocol all -SSLv2
SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:RC4+RSA:+HIGH:+MEDIUM:+LOW
SSLCertificateFile /etc/pki/tls/certs/example.com.crt
SSLCertificateKeyFile /etc/pki/tls/private/example.com.nopass.key
SSLCertificateChainFile /etc/pki/tls/certs/sub.class1.server.ca.pem
```

### Testing

``` {.example}
openssl s_client -connect example.com:https
```

Dovecot
-------

### combine the intermediate certificate with your own certificate.

``` {.example}
cd /etc/pki/tls/certs/
cat example.com.crt sub.class1.server.ca.pem > dovecot.pem
cd /etc/pki/tls/private/
cat example.com.nopass.key > dovecot.pem
```

### add the key and the certificate to your dovecot config in /etc/dovecot/conf.d/10-ssl.conf

``` {.example}
ssl_cert = </etc/pki/tls/certs/dovecot.pem
ssl_key = </etc/pki/tls/private/dovecot.pem
```

### Testing

``` {.example}
openssl s_client -connect dev.ppcam.tv:pop3s
openssl s_client -connect dev.ppcam.tv:imaps
```

Postfix
-------

### Configuration

``` {.example}
smtpd_use_tls = yes
smtpd_tls_auth_only = no
smtpd_tls_key_file = /etc/pki/tls/private/example.com.nopass.key
smtpd_tls_cert_file = /etc/pki/tls/certs/example.com.crt
smtpd_tls_CAfile = /etc/pki/tls/certs/startssl-ca-bundle.pem
smtp_tls_CAfile = $smtpd_tls_CAfile
```

### Testing

``` {.example}
telnet example.com 25
Trying 218.206.111.222...
Connected to example.com.
Escape character is '^]'.
220 mail.example.com NO UCE ESMTP
EHLO example.com
250-mail.example.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-AUTH PLAIN LOGIN
250-AUTH=PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
```

Reference
=========

<https://github.com/ioerror/duraconf>
-------------------------------------

<https://www.digitalocean.com/community/articles/how-to-set-up-apache-with-a-free-signed-ssl-certificate-on-a-vps>
------------------------------------------------------------------------------------------------------------------

<http://rene.bz/setting-smtp-authentication-over-tls-postfix/>
--------------------------------------------------------------

<http://wiki2.dovecot.org/SSL/DovecotConfiguration>
---------------------------------------------------
