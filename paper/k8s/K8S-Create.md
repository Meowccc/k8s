# K8S 安裝



### 產生新的 join token
```shell
kubeadm token create
```
### 查看新的 join token
```shell
kubeadm token list
```

### 產生新的 Hash 值
```shell
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```