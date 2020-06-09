
# Helm V2
https://v2.helm.sh/docs/

# Helm V3
https://helm.sh/docs/intro/install/

# Helm V3 新版本发布 (v2与V3区别）
https://yq.aliyun.com/articles/703821?utm_content=g_1000060142

# 为什么你必须尽快转向 Helm v3
https://yq.aliyun.com/articles/709187?spm=a2c4e.11153940.0.0.41635121DxxgXM

# 深度解读Helm 3: 犹抱琵琶半遮面
https://yq.aliyun.com/articles/703943?spm=a2c4e.11153940.0.0.41635121RXj0Ay

# Helm github
The Kubernetes Package Manage
https://github.com/helm/helm

# 开源的 helm hub （图形化查看）
1. Helm Hub
https://hub.helm.sh/
2. kubeapps
https://hub.kubeapps.com/
3. 阿里的
https://developer.aliyun.com/hub/

# kubernetes之helm简介、安装、配置、使用指南
https://blog.csdn.net/bbwangj/article/details/81087911

# Taming Kubernetes with Helm
https://blog.francium.tech/taming-kubernetes-with-helm-dfed2c278b4f

# helm search in github
https://github.com/helm/charts/tree/master/stable

# 搭建chart仓库chartmuseum

[charmuseum](https://github.com/helm/chartmuseum) 提供了良好的 api 接口，适合开发调用

- Helm Chart Repository
  - GET /index.yaml - retrieved when you run helm repo add chartmuseum http://localhost:8080/
  - GET /charts/mychart-0.1.0.tgz - retrieved when you run helm install chartmuseum/mychart
  - GET /charts/mychart-0.1.0.tgz.prov - retrieved when you run helm install with the --verify flag
- Chart Manipulation
  - POST /api/charts - upload a new chart version
  - POST /api/prov - upload a new provenance file
  - DELETE /api/charts/<name>/<version> - delete a chart version (and corresponding provenance file)
  - GET /api/charts - list all charts
  - GET /api/charts/<name> - list all versions of a chart
  - GET /api/charts/<name>/<version> - describe a chart version
  - HEAD /api/charts/<name> - check if chart exists (any versions)
  - HEAD /api/charts/<name>/<version> - check if chart version exists
- Server Info
  - GET / - HTML welcome page
  - GET /health - returns 200 OK

下面是安装步骤

```bash
1. install helm
2. install chartmuseum （systemd / k8s yaml mode)
   - systemd chartmuseum.service
   - helm install --name my-chartmuseum -f custom.yaml stable/chartmuseum
3. install helm-push
4. helm repo list
4. helm repo add chartmuseum http://<NodePort_ip>:<NodePort_port> --username myuser --password mypass
5. helm push mychart/ chartmuseum
6. helm search chartmuseum/
```
参考:

https://www.jianshu.com/p/b31a091a4ef2
https://www.cnblogs.com/Dev0ps/p/11258539.html
https://github.com/helm/charts/tree/master/stable/chartmuseum