# SSL Certificates

## Self Signed Certificates
This cheatsheet is designed to show how to create a self-signed SSL Certificates using OpenSSL.

### 1: Generate the CA

1. Generate the private CA Key
```bash
openssl genrsa -aes256 -out ca-private-key.pem 4096
```
2. Generate a CA Root Certificate
```bash
openssl req -x509 -new -nodes -key ca-private-key.pem -sha256 -days 3650 -out ca.pem
```


### Generate Certificate
1. Create an RSA key
```bash
openssl genrsa -out cert-key.pem 4096
```
2. Create a Certificate Signing Request
```bash
openssl req -new -sha256 -subj "/CN=yourcn" -key cert-key.pem -out cert.csr
```
3. Create an extension file  with alternative names
```bash
echo "subjectAltName=DNS:your-dns.record,IP:xxx.xxx.xxx.xxx" >> extfile.cnf
```

4. Create the certificate
```bash
openssl x509 -req -sha256 -days 365 -in cert.csr -CA ca.pem -CAkey ca-key.pem -out cert.pem -extfile extfile.cnf -CAcreateserial
```