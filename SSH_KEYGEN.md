# Steps to Generate SSH on Desktop with Github

check exist key

```bash
ls ~/.ssh
```

generate key

```bash
ssh-keygen -t rsa -b 4096 -C "<your-email>" 
```

show public key

```bash
cat ~/.ssh/id_rsa.pub
```
copy key and insert to https://github.com/settings/keys 