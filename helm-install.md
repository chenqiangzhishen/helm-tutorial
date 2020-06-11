
# Helm 客户端安装

Helm 的安装方式很多，这里采用二进制的方式安装。更多安装方法可以参考 Helm 的官方帮助文档。

```bash
#从官网下载最新版本的二进制安装包到本地：https://github.com/kubernetes/helm/releases
wget https://get.helm.sh/helm-v2.9.1-linux-amd64.tar.gz
tar -zxvf helm-v2.9.1-linux-amd64.tar.gz # 解压压缩包
# 把 helm 指令放到bin目录下
mv ./linux-amd64/helm  /usr/local/bin/
helm help # 验证
```

Tiller 是以 Deployment 方式部署在 Kubernetes 集群中的，只需使用以下指令便可简单的完成安装。

`$ helm init`

由于 Helm 默认会去 storage.googleapis.com 拉取镜像，如果你当前执行的机器不能访问该域名的话可以使用以下命令来安装
```bash
# 创建服务端
helm init --service-account tiller --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.9.1  --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
 
# 创建TLS认证服务端，参考地址：https://github.com/gjmzj/kubeasz/blob/master/docs/guide/helm.md
helm init --service-account tiller --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.9.1 --tiller-tls-cert /etc/kubernetes/ssl/tiller001.pem --tiller-tls-key /etc/kubernetes/ssl/tiller001-key.pem --tls-ca-cert /etc/kubernetes/ssl/ca.pem --tiller-namespace kube-system --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
```

```bash
-[appuser@chenqiang-dev ~]$ helm init --client-only --stable-repo-url https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts/
Creating /home/appuser/.helm/repository/repositories.yaml 
Adding stable repo with URL: https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts/ 
Adding local repo with URL: http://127.0.0.1:8879/charts 
$HELM_HOME has been configured at /home/appuser/.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
-[appuser@chenqiang-dev ~]$ 
-[appuser@chenqiang-dev ~]$ helm repo add incubator https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts-incubator/
"incubator" has been added to your repositories
-[appuser@chenqiang-dev ~]$ 
-[appuser@chenqiang-dev ~]$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
...Successfully got an update from the "incubator" chart repository
Update Complete. ⎈ Happy Helming!⎈ 
```
# Helm 服务端安装 Tiller

接下来，我们来创建服务端 Tiller 组件

```bash

# 创建服务端
-[appuser@chenqiang-dev ~]$ helm init --service-account tiller --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.9.1  --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
$HELM_HOME has been configured at /home/appuser/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
Happy Helming!
 
# 创建TLS认证服务端，参考地址：https://github.com/gjmzj/kubeasz/blob/master/docs/guide/helm.md
-[appuser@chenqiang-dev ~]$ helm init --service-account tiller --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.9.1 --tiller-tls-cert /etc/kubernetes/ssl/tiller001.pem --tiller-tls-key /etc/kubernetes/ssl/tiller001-key.pem --tls-ca-cert /etc/kubernetes/ssl/ca.pem --tiller-namespace kube-system --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
$HELM_HOME has been configured at /home/appuser/.helm.

Tiller (the Helm server-side component) has been upgraded to the current version.
Happy Helming!

```

helm install stable/nginx-ingress --set controller.hostNetwork=true,rbac.create=true




