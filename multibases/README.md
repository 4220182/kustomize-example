# 说明

* base: 基础文件
* dev: 开发环境
* staging: 测试环境
* production: 生产环境

```
~/k8s/test/kustomize/example/multibases$ tree
.
├── base
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
├── dev
│   ├── base
│   │   ├── kustomization.yaml
│   │   └── pod-env-global.yaml
│   ├── hello1
│   │   ├── kustomization.yaml
│   │   ├── pod-env.yaml
│   │   └── svc-nodeport.yaml
│   ├── hello2
│   │   ├── kustomization.yaml
│   │   ├── pod-env.yaml
│   │   └── svc-nodeport.yaml
│   └── kustomization.yaml
├── kustomization.yaml
├── production
│   ├── base
│   │   ├── kustomization.yaml
│   │   └── pod-env-global.yaml
│   ├── hello1
│   │   ├── kustomization.yaml
│   │   ├── pod-env.yaml
│   │   └── svc-nodeport.yaml
│   ├── hello2
│   │   ├── kustomization.yaml
│   │   ├── pod-env.yaml
│   │   └── svc-nodeport.yaml
│   └── kustomization.yaml
└── staging
    └── kustomization.yaml

```

发布到开发环境：
```
$ kubectl kustomize ./dev/ |kubectl apply -f -
或
$ kubectl apply -k ./dev/

$ kubectl get po,svc
NAME                                    READY   STATUS    RESTARTS   AGE
pod/hello1-dev-hello-866686dbd5-x4x7w   1/1     Running   0          13m
pod/hello2-dev-hello-77b5d49755-wl2g7   1/1     Running   0          13m

NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/hello1-dev-hello   NodePort    10.254.140.152   <none>        80:8881/TCP      13m
service/hello2-dev-hello   NodePort    10.254.105.108   <none>        80:8882/TCP      13m

```

发布到生产环境：
```
$ kubectl kustomize ./production/ |kubectl apply -f -
或
$ kubectl apply -k ./production/

$ kubectl get svc,po
NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/hello1-pro-hello   NodePort    10.254.43.50     <none>        80:8883/TCP      7s
service/hello2-pro-hello   NodePort    10.254.44.184    <none>        80:8884/TCP      7s

NAME                                    READY   STATUS    RESTARTS   AGE
pod/hello1-pro-hello-7c99859775-lxmqv   1/1     Running   0          7s
pod/hello2-pro-hello-b96d57d4c-mqmqz    1/1     Running   0          7s

```

清除测试：
```
$ kubectl kustomize ./dev/ |kubectl delete -f -
$ kubectl kustomize ./production/ |kubectl delete -f -
或
$ kubectl delete -k ./dev/
$ kubectl delete -k ./production/
```
