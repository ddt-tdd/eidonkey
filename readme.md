EIDONKEY
========

A Rust project to read out and use the Belgian eID card through a local SSL-WebServer. Using AJAX calls in your web application, you are able to read out or use the Belgian eId card.
This replaces the java applet. We'll try also that you don't need to use the Middleware.

Notes
-----
This is highly experimental. 
The certificate and private key delivered in the project should be replace with your own certificate/private key.
The private key is not protected (it is plain). During setup it should be protected by the OS and with a password.
You have to import the certificate into your OS or Firefox browser:
- Windows -> Certificate store
- Mac OS X -> Key chain
- Linux -> /etc/ssl, /local/etc/ssl or somewhere else in a protected directory

Supported now with only GET method:
https://localhost:10443/identity -> identity information 
https://localhost:10443/address -> address information
https://localhost:10443/version -> version information about eidonkey
https://localhost:10443/photo -> photo in base 64 
https://localhost:10443/status -> some status of reader used, ATR of card, etc
https://localhost:10443/signature/authentication?data=<HASH> -> signature with the private key corresponding with authentication certificate
https://localhost:10443/signature/signing?data=<HASH> -> signature with the private key corresponding with signing certificate
https://localhost:10443/certificates/authentication -> get authentication certificate
https://localhost:10443/certificates/signing -> get signing certificate
https://localhost:10443/certificates/rootca -> get rootca certificate
https://localhost:10443/certificates/ca -> get ca certificate which signed the signing and authentication certificate
https://localhost:10443/certificates/rrn -> get rrn certificate

TODO
----
1) Number of retries not yet implemented
2) Auto detection of card in readers (now only the first reader is used)
3) Pinpad reader support
4) Eid Viewer as an Electron application

To compile
----------
- wxWidgets 3.1 must be pre installed
- openssl-1.0.1h must be pre installed
- nss certutil from firefox is in binary form in the Git repo

Create self-signed certificate
------------------------------
openssl req -x509 -sha256 -newkey rsa:2048 -keyout certificate.key -out certificate.crt -days 1024 -nodes -subj '/CN=localhost'
or
eidonkey --gencert
it will generate cert.crt and cert.key (unprotected private key) in the current directory, which can directly be used by the service.
But now the CA and SSL certificates never leave the application.