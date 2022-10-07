Type: #note
Topics: [[Cryptography]] 

---

# Security Principle

- [[Authentication]]
- [[Key Exchange]]
- [[Key Confirmation]]
- [[Public Key Infrastructure]]
- [[Certificate Authority]]
- [[X.509 certificate]]

## [[Asymmetric Cryptography]]

Data encrypted with the public key can only be decrypted with the private key, and vice versa.

## [[Authentication]]

## SSL/TLS

Message Authentication Code (MAC), is also part of SSL/TLS.

TLS (Transport Layer Security) is more modern word than SSL (Secure Sockets Layer). 

During the TLS handshake, the client and server use the public and private keys to exchange randomly generated data, and this random data is used to create new keys for encryption, called the session keys.

An SSL certificate is a file installed on a website's [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/). It's simply a data file containing the public key and the identity of the website owner, along with other information. Without an SSL certificate, a website's traffic can't be encrypted with TLS.

Technically, any website owner can create their own SSL certificate, and such certificates are called self-signed certificates. However, browsers do not consider self-signed certificates to be as trustworthy as SSL certificates issued by a certificate authority.

TLS handshake process: https://www.cloudflare.com/en-gb/learning/ssl/what-happens-in-a-tls-handshake/

How browser verify the SSL certificate: 
https://www.ssl.com/article/browsers-and-certificate-validation/

浏览器拿到网站的 SSL 证书后，会根据签发机构的公钥，对证书加密（生成签名），然后比较证书本身的提供的签名是否一致。一致则验证成功。

Browsers are shipped with a built-in list of trusted roots. (These are roots from CAs who have passed the browser’s stringent criteria for inclusion.)

![[Pasted image 20220612111447.png]]

## Diffie-Hellman Key Establishment

Two public values: 
- n , a large prime number
- g , a generator mod n

Protocol:

```
Msg 1. a -> b: g^x mod n 
Msg 2. b -> a: g^y mod n
```

- a computes share key: `(g^y mod n) mod n`
- b computes share key: `(b^x mod n) mod n`

Now we share a key :  `g^{xy} mod n`

The thing is we need some kind of [[authentication]] here to prove a to b, that a is a or verse versa. Otherwise, we have Man in the Middle attack.

