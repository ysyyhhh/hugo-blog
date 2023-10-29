# minikube

使用

进入pods的容器

```bash
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash

# 查看对应容器的日志

kubectl logs -f <pod-name> -c <container-name>
```
