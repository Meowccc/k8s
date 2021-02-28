# 建立私有Image庫


### 因為沒有https 需設定 /etc/docker/daemon.json
```json
{
  "live-restore": true,
  "group": "dockerroot",
  "insecure-registries": ["<ip>:<port>"]
}
```

### 重新啟動 Docker Service
```shell
$ systemctl restart docker
```

### Pull docker registry image
```
docker pull registry
```

### 建立 Docker Registry Server
```
docker run --name <registry-name> -d -p 5000:5000 -v <path>:/var/lib/registry  registry
```

### 指定 Docker Image Tag
```
docker tag <image-name> <ip>:<port>/<image-name>
```

### Push Docker Image 
```
docker push <ip>:<port>/<image-name>
```


### 查看 Private Registry 的 Image 
```shell
$ curl -X GET http://<ip>:<port>/v2/_catalog
```

### 查看 Docker Image 有哪些 Tag
```shell
$ curl -X GET http://<ip>:<port>/v2/<image-name>/tags/list
```

---

# 使用 Docker Registry Web

### Pull hyper/docker-registry-web
```
docker pull hyper/docker-registry-web
```

### 啟動 Docker Registry Web
```
$ docker run -d -p 6060:8080 --name registry-web --restart=always  --link registry -e REGISTRY_URL=http://<ip>:<port>/v2 hyper/docker-registry-web
```




