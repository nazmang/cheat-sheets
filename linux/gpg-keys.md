# GPG key tasks
Commonly used GPG operations 
#### Create new key automatically
```shell
export GPG_TTY=$(tty); gpg --batch --gen-key <<EOF
%no-protection
Key-Type: 1
Key-Length: 2048
Subkey-Type: 1
Subkey-Length: 2048
Name-Real: Root Superuser
Name-Email: root@foo.bar
Expire-Date: 0
EOF
```
#### List secret keys fingerprints 
```shell
gpg --list-secret-keys --with-colons --fingerprint | sed -n 's/^fpr:::::::::\([[:alnum:]]\+\):/\1/p'

F7682AF92EAF3DDB07CA312C6DCF04ECDB256792
9EE76099E9B9AA5023759CB6898478C044E0A1C5
```
#### Export private and public keys
```shell
gpg --export-secret-keys --armor F7682AF92EAF3DDB07CA312C6DCF04ECDB256792 > private.key
gpg --export --armor F7682AF92EAF3DDB07CA312C6DCF04ECDB256792 > public.key
```
#### Delete secret key without prompt 
```shell
gpg --batch --yes --delete-secret-keys F7682AF92EAF3DDB07CA312C6DCF04ECDB256792
```
#### Delete key without prompt
```shell
gpg --batch --yes --delete-keys F7682AF92EAF3DDB07CA312C6DCF04ECDB256792
```