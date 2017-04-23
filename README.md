# SSL/TLS Reference Guide

## Overview

- https://www.sslshopper.com/what-is-ssl.html

### What is SSL?
SSL stands for Secure Sockets Layer, an encryption technology that was originally created by Netscape in the 1990s. SSL creates an encrypted connection between your web server and your visitors' web browser allowing for private information to be transmitted without the problems of eavesdropping, data tampering, and message forgery.


To enable SSL on a website, you will need to get an SSL Certificate that identifies you and install it on your web server. 


If SSL is properly deployed, the information transmitted between the web browser and the web server (whether it is contact or credit card information), is encrypted and only seen by the organization that owns the website.


### SSL vs. TLS
SSL and TLS generally mean the same thing. TLS 1.0 was created by RFC 2246 in January 1999 as the next version of SSL 3.0.


Most people are familiar with the term SSL so that is usually the term that is used when the system is using the newer TLS protocol.

### Secure Socket Layer (SSL)
- https://gist.github.com/leommoore/da79ce0931a5471304d9

Secure Socket Layer (SSL) is a mechanism to allow information to be securely communicated. Specifically, it is a cryptographic protocol that enables two parties such as a web server and a browser to exchange information securely by encrypting it before sending and decrypting it upon receipt. It is based on the X.509 standards.


Symmetric and Asymmetric Encryption Encrypting and decrypting requires a secret like a password, which is known as a key. There are two types of key, symmetric and asymmetric. If a symmetric key is used it means that the same key is used to encrypt and decrypt the message. Asymmetric keys consist of a private and public key. The message sender encrypts the message using their private (secret) key and the message receiver can decrypt the message using the senders public key.


Asymmetric keys require more processing resources than symmetric keys. The problem is that to communicate using symmetric keys both parties have to have the symmetric keys first and the question is how to transfer the symmetric key securely. SSL resolves this problem by using a asymmetric key to transfer the symmetric key and then use the symmetric key for the rest of the session.

### Connection Flow
A typically flow would be the following:

- The client (web browser) connects to the https website
- The server sends its SSL Certificate (public key) to the client
- The client validates the certificate
- The client generates a random symmetric key and encrypts it using the public key in the Certificate
- The client sends the encrypted symmetric key to the server
- The server uses its private key to decrypt the symmetric key
- The server and client encrypt all further traffic using the symmetric key
- At the end of the session the symmetric key is discarded


## What is a certificate authority (CA)?
A certificate authority is an entity which issues digital certificates to organizations or people after validating them. 


Certification authorities have to keep detailed records of what has been issued and the information used to issue it, and are audited regularly to make sure that they are following defined procedures. Every certification authority provides a Certification Practice Statement (CPS) that defines the procedures that will be used to verify applications. There are many commercial CAs that charge for their services (VeriSign). Institutions and governments may have their own CAs, and there are also free Certificate Authorities.


Every certificate authority has different products, prices, SSL certificate features, and levels of customer satisfaction. Learn more about choosing a certificate provider or read our SSL Certificate reviews to find the best provider to purchase from.

## Root Certificate Store

- https://certsimple.com/blog/control-the-ssl-cas-your-browser-trusts

The browser you're using right now trusts a bunch of certificate authorities. Which bunch of certificate authorities - properly called a 'root certificate store' - is determined by your OS and browser

The major root certificate stores are Apple, Microsoft, Mozilla, and Android. When you visit a website, the website presents a certificate that's signed by another certificate, which is signed by another certificate, until you reach one of the certificates in the store you're using.


Each certificate store has its own requirements for a certificate authority to get added. However they all require certificate authorities to pass WebTrust for Certification Authorities, an audited assurance process for the policies and procedures for verifying identity, issuing certificates, handling keys, and more.


Occasionally CAs violate the WebTrust requirements: the Chinese government (CNNIC) and Symantec both recently issued fake certificates for google.com. In CNNIC's case, they gave their private key to a third party that issued the fake certificate. CNNIC was removed from the Android and Mozilla root stores, but the Microsoft root store - used by Chrome on Windows and Edge on Windows - only revoked the misissued certificates. Symantec's case was less severe: an accidental, but still troubling, misissuance: in this case the root stores kept their certificates and the employees responsible were fired.


## Google's Certificate Transparency project

- https://www.certificate-transparency.org/

Certificate Transparency aims to remedy these certificate-based threats by making the issuance and existence of SSL certificates open to scrutiny by domain owners, CAs, and domain users. Specifically, Certificate Transparency has three main goals:
- Make it impossible (or at least very difficult) for a CA to issue a SSL certificate for a domain without the certificate being visible to the owner of that domain.
- Provide an open auditing and monitoring system that lets any domain owner or CA determine whether certificates have been mistakenly or maliciously issued.
- Protect users (as much as possible) from being duped by certificates that were mistakenly or maliciously issued.


##


##


## Knowledge Base

###  acme protocol
- http://ietf-wg-acme.github.io/acme/


## Reference URLs


### mozilla vhost generator
- https://mozilla.github.io/server-side-tls/ssl-config-generator/


### Let's Encrypt
- https://letsencrypt.org/
- https://letsencrypt.org/docs/


### CLoudflare
- https://www.cloudflare.com
- https://github.com/cloudflare/sslconfig/blob/master/conf


### AWS
- https://aws.amazon.com/certificate-manager/faqs/
- https://aws.amazon.com/cloudfront/
- https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/


### monitors
- https://crt.sh/
- https://developers.facebook.com/tools/ct/
- https://letsmonitor.org/
- https://www.ssllabs.com/ssltest/


#### brew
- https://github.com/nabla-c0d3/sslyze
- https://testssl.sh/


### Scott Helme
- https://scotthelme.co.uk/
- https://report-uri.io/
- https://securityheaders.io/


## Let's Encrypt

## ACME protocol challenges options

- https://certbot.eff.org/docs/using.html#getting-certificates-and-choosing-plugins
- standalone
Use standalone mode to obtain a cert if you don’t want to use (or don’t currently have) existing server software.

The standalone plugin does not rely on any other server software running on the machine where you obtain the cert.
- webroot
If you’re running a local webserver for which you have the ability to modify the content being served, and you’d prefer not to stop the webserver during the certificate issuance process, you can use the webroot plugin to obtain a cert

The webroot plugin works by creating a temporary file for each of your requested domains in ${webroot-path}/.well-known/acme-challenge. Then the Let’s Encrypt validation server makes HTTP requests to validate that the DNS for each requested domain resolves to the server running certbot.

Note that to use the webroot plugin, your server must be configured to serve files from hidden directories. If /.well-known is treated specially by your webserver configuration, you might need to modify the configuration to ensure that files inside /.well-known/acme-challenge are served by the webserver.
- apache
- nginx
- manual (http-01, [DNS-01](#dns-01))
If you’d like to obtain a cert running certbot on a machine other than your target webserver or perform the steps for domain validation yourself, you can use the manual plugin.

The manual plugin can use either the http or the dns challenge.


### API rate limits
- https://letsencrypt.org/docs/rate-limits/
#### LE Production API
- The main limit is Certificates per Registered Domain (20 per week). 
- A limit of 100 Names per Certificate (SAN)
- A maximum of 500 Accounts per IP Address per 3 hours
#### LE Staging API
- The Certificates per Registered Domain limit is 30,000 per week.
- The Duplicate Certificate limit is 30,000 per week.
- The Failed Validations limit will be 60 per hour.


### LE clients
- https://letsencrypt.org/docs/client-options/

#### Certbot
- https://certbot.eff.org/about/

#### acme.sh
- https://acme.sh
- https://github.com/Neilpang/acme.sh
- https://github.com/Neilpang/acme.sh/wiki

### DNS-01
When using the dns plugin, LE client will ask you to place a TXT DNS record with specific contents under the domain name consisting of the hostname for which you want a certificate issued, prepended by _acme-challenge.
- https://github.com/Neilpang/acme.sh/tree/master/dnsapi/README.md


## Auxiliar 
- https://cloud-images.ubuntu.com/locator/ec2/
- https://brew.sh/

## ToDo
- wildcard
- SAN
- Revoking
- HSTS


## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D


## License

[The Unlicense](LICENSE)
