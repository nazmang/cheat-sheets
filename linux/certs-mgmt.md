# Certificates management

### Show expiration and issuer information about the server SSL certificate

```
echo | openssl s_client -servername yourdomain.com -connect yourdomain.com:443 2>/dev/null | openssl x509 -noout -issuer -subject -dates
```
### Generate Private key and CSR with one command
```
openssl req -out yourdomain.com.csr -new -newkey rsa:4096 -nodes -keyout yourdomain.com.key -subj "/C=US/ST=NY/L=New York/O=Your Company, Inc./OU=IT/CN=yourdomain.com"
```
### Generate Private key and CSR with SAN (Subject Alternative Names)
```
openssl req -new -sha512 -nodes -out yourdomain.com.csr -newkey rsa:4096 -keyout yourdomain.com.key -config <(
cat <<-EOF
[req]
default_bits = 4096
prompt = no
default_md = sha512
days = 365
req_extensions = req_ext
distinguished_name = dn

[ dn ]
C=US
ST=New York
L=Rochester
O=End Point
OU=Testing Domain
emailAddress=admin@yourdomain.com
CN = www.yourdomain.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = yourdomain.com
DNS.2 = www.yourdomain.com
IP = 1.2.3.4
EOF
)
```
### Generate a Self-Signed Certificate from CSR
```
openssl x509 -req -days 365 -in yourdomain.com.csr -signkey yourdomain.com.key -out yourdomain.com.crt
```
### Verify Certificate
```
openssl x509 -text -noout -in yourdomain.com.crt
```
## Creating Elliptic Curve Keys using OpenSSL

### Generate a Private key for a curve
```
openssl ecparam -name prime256v1 -genkey -noout -out private.pem
```
### Generate corresponding Public key
```
openssl ec -in private.pem -pubout -out public.pem
```
### Create a Self-Signed Certificate
```
openssl req -new -x509 -key private.pem -out cert.pem -days 365
```
### Convert .pem to .pfx
```
openssl pkcs12 -export -inkey private.pem -in cert.pem -out cert.pfx
```