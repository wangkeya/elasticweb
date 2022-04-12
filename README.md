webhook开发指南
=============

### 项目初始化

```text
# 安装cert-manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml

go mod init elasticweb
```


### 安装webhook
```text

kubebuilder init --domain my.com --license apache2 --owner "wangkeya"

kubebuilder create api --group elasticweb --version v1 --kind ElasticWeb

kubebuilder create webhook \
--group elasticweb \
--version v1 \
--kind ElasticWeb \
--defaulting \
--programmatic-validation
```



修改内容如下
1. dockerfile修改
```dockerfile

# 修改前
FROM kubeimages/distroless-static:latest
# 修改后
RUN GOPROXY=https://goproxy.cn go mod download

# 修改前
# 此处忽略

# 修改后
FROM kubeimages/distroless-static:latest
```
2. manager_auth_proxy_path.yaml修改
```yaml
# 修改后
docker.io/kubesphere/kube-rbac-proxy:v0.11.0
```


3. makefile修改
```makefile

# 修改后
IMG ?= docker.io/wangkeya/controller:latest
```

### 部署
