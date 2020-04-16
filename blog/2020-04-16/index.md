---
layout: article-layout.njk
title: Create SAML-consumable X.509 private public key pair
date: 2020-04-10
tags: post
---
##### Published on {{ date | date : '%Y-%m-%d' }}
#### {{ title }}
###### `Single SignOn`, `SAML`, `X.509`

To implement a Single SignOn (SSO) solution, SAML is a good fit. The library that we choose to use is from [Component Space](https://www.componentspace.com/saml-for-asp-net?gclid=EAIaIQobChMI2r7mo7Ht6AIVC5SzCh1dxg0VEAAYASAAEgIHPPD_BwE). I document below the steps to create the X.509 private public key pair that can be used by Component Space.

But before we start, the process will require us to secure a couple of export/import steps with a password and a PEM phrase. It's important to remember this password. It will need to be used in the code later. Since we don't normally commit the password to source control, if we forget, we can't easily use the code on a another computer or collaborate with team members.

```
$ openssl genrsa -out private.key 1024
```
This will create a file named `private.key`.


```
$ openssl req -new -x509 -key private.key -out publickey.cer -days 365
```

This will prompt us several question such Country, State or Province, City, Organization, Organization Unit, Common Name (domain name), email. When it's done, we will have a total of 2 files: `private.key` and `publickey.cer`.

```
$ openssl pkcs12 -export -out myapp_private_original.pfx -inkey private.key -in publickey.cer
```

This will prompt for password. Twice, just to make sure we don't have any typo. This will be the password that will need to be included in the code later. So it's important to remember this.  When it's done, we will have a total of 3 files: `private.key`, `publickey.cer` and `myapp_private_original.pfx`.

To get this working with Component Space, we will have to do few more steps. 

```
$ certutil -dump myapp_private_original.pfx
```

We will be asked for a password. That's the password we entered earlier. We will see something like

```
certutil -dump idp.pfx
Enter PFX password:
================ Certificate 0 ================
================ Begin Nesting Level 1 ================
Element 0:
Serial Number: 74f0ebfe22358db8433138f9558c9af9
Issuer: CN=www.idp.com
 NotBefore: 22/11/2013 6:20 PM
 NotAfter: 1/01/2050 12:00 AM
Subject: CN=www.idp.com
Signature matches Public Key
Root Certificate: Subject matches Issuer
Cert Hash(sha1): a6 a4 ae 4e 0b 37 8e c7 36 78 e5 81 26 90 af 50 e3 ec 37 69
----------------  End Nesting Level 1  ----------------
  Provider = Microsoft Enhanced RSA and AES Cryptographic Provider
Encryption test passed
CertUtil: -dump command completed successfully.
```

Next, type the command

```
$ openssl pkcs12 -in idp.pfx
```

Again enter the password. This time, we will be prompted for a PEM pass phrase, twice. Just go ahead and use the phrase the same as the password.

```
$ set RANDFILE=.\openssl.rnd
```

then

```
$ openssl pkcs12 -in myapp_private_original.pfx -out myapp_private.pem
```

Again enter the password and the PEM pass phrase twice. When it's done, we will have a total of 4 files: `private.key`, `publickey.cer`, `myapp_private_original.pfx` and `myapp_private.pem`. 

Finally, one more command:

```
$ openssl pkcs12 -export -in myapp_private.pem -out myapp_private.pfx -CSP "Microsoft Enhanced RSA and AES Cryptographic Provider"
```

Again, we will be prompted for the pass phrase and export password, twice. Now we will have 5 files: `private.key`, `publickey.cer`, `myapp_private_original.pfx`, `myapp_private.pem` and `myapp_private.pfx`.

The private key file is `myapp_private.pfx`. Do not share this file. Do share the public key file which is `publickey.cer`.

In future post, we will see how to write SAML provider and consumer that make use of the X.509 certificates created here.

References:
1. [https://stackoverflow.com/questions/16480846/x-509-private-public-key#16481636](https://stackoverflow.com/questions/16480846/x-509-private-public-key#16481636)

2. [https://www.componentspace.com/Forums/1578/SHA256-and-Converting-the-Cryptographic-Service-Provider-Type](https://www.componentspace.com/Forums/1578/SHA256-and-Converting-the-Cryptographic-Service-Provider-Type)