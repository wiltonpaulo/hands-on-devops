  [Home](https://crwlabs.github.io/hands-on-devops/) | [Dia 01](https://crwlabs.github.io/hands-on-devops/dia-01) |   [Dia 02](https://crwlabs.github.io/hands-on-devops/dia-02) |   [Dia 03](https://crwlabs.github.io/hands-on-devops/dia-03)

## Bem Vindo ao Projeto Hands On DevOps Online

## Setup Inicial

### Instalar Sistema Operacional
1. Logar na conta linuxacademy e criar 3 instancias
> master-node, worker-node1, worker-node2

### Atualizar pacotes e dependencias
1. Repositório

```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```

1. Instalar pacotes

```
sudo yum install -y kubelet kubeadm kubectl
setenforce 0
sed s,enforcing,disabled, /etc/sysconfig/selinux -i
...
Disable swap
```

3. Configurar hostnames

```
sudo hostnamectl set-hostname master-node
sudo hostnamectl set-hostname worker-node1
sudo hostnamectl set-hostname worker-node2
```

## Kubernetes

### Instalar cluster kubernetes
1. Criar o cluster

```
kubeadm init --pod-network-cidr=10.244.0.0/16
```

<details>
<summary>Ver output</summary>

```
W0415 19:40:43.292546    3585 configset.go:202] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]
[init] Using Kubernetes version: v1.18.1
[preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [master-node kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.31.24.89]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [master-node localhost] and IPs [172.31.24.89 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [master-node localhost] and IPs [172.31.24.89 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
W0415 19:41:22.123846    3585 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[control-plane] Creating static Pod manifest for "kube-scheduler"
W0415 19:41:22.125761    3585 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 17.503638 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.18" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node master-node as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node master-node as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: i551h2.sz9x3syvdgi7vl7x
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.24.89:6443 --token i551h2.sz9x3syvdgi7vl7x \
    --discovery-token-ca-cert-hash sha256:8419726439040e89ad709f6c07adff630e996419341a369e4406318bdb2078ff 
```

</details>

1. Para começar a usar o kubernetes, configure na conta de um usuário:

```
To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

3. Configurar a rede com o Calico

```
kubectl apply -f https://docs.projectcalico.org/v3.9/manifests/calico.yaml
```

<details><summary>Ver output</summary>

```
configmap/calico-config created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrole.rbac.authorization.k8s.io/calico-node created
clusterrolebinding.rbac.authorization.k8s.io/calico-node created
daemonset.apps/calico-node created
serviceaccount/calico-node created
deployment.apps/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
```

</details>

4. Rodar join nos worker nodes

```
 kubeadm join 172.31.24.89:6443 --token i551h2.sz9x3syvdgi7vl7x \
>     --discovery-token-ca-cert-hash sha256:8419726439040e89ad709f6c07adff630e996419341a369e4406318bdb2078ff
W0415 20:08:20.478184    4249 join.go:346] [preflight] WARNING: JoinControlPane.controlPlane settings will be ignored when control-plane flag is not set.
[preflight] Running pre-flight checks
        [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
        [WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.18" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```

1. Fazer untaint do node master

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

### Instalar/Configurar Helm

Link para o procedimento: [Install Helm](https://helm.sh/docs/intro/install/)

### Configurar certificado cert-manager
  [Documentação oficial](https://cert-manager.io/docs/installation/kubernetes/)

1. Instalar o CustomResourceDefinitions

```
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.14.1/cert-manager.yaml
```

1. Criar namespace e instalar o helm do cert-manager

```bash
$ kubectl create namespace cert-manager
namespace/cert-manager created
[cloud_user@linuxacademycc1c cert-manager]$ helm repo add jetstack https://charts.jetstack.io
"jetstack" has been added to your repositories
[cloud_user@linuxacademycc1c cert-manager]$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "jetstack" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈ 
[cloud_user@linuxacademycc1c cert-manager]$ helm install \
>   cert-manager jetstack/cert-manager \
>   --namespace cert-manager \
>   --version v0.14.1
NAME: cert-manager
LAST DEPLOYED: Wed Apr 15 21:09:03 2020
NAMESPACE: cert-manager
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
cert-manager has been deployed successfully!

In order to begin issuing certificates, you will need to set up a ClusterIssuer
or Issuer resource (for example, by creating a 'letsencrypt-staging' issuer).

More information on the different types of issuers and how to configure them
can be found in our documentation:

https://cert-manager.io/docs/configuration/

For information on how to configure cert-manager to automatically provision
Certificates for Ingress resources, take a look at the `ingress-shim`
documentation:

https://cert-manager.io/docs/usage/ingress/
[cloud_user@linuxacademycc1c cert-manager]$ 
```

1. Configurar ClusterIssuer e Token para acesso ao Cert-manager

```
$ cloudflare-api-token.yaml
apiVersion: v1
kind: Secret
metadata:
  name: cloudflare-api-token-secret
  namespace: cert-manager
type: Opaque
stringData:
  api-token: xxxxx
```

```
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
 name: letsencrypt-cloudflare
 namespace: cert-manager
spec:
 acme:
   server: https://acme-v02.api.letsencrypt.org/directory
   email: w*******o@gmail.com
   privateKeySecretRef:
     name: letsencrypt-cloudflare
   solvers:
   - dns01:
      cloudflare:
        email: w*******o@gmail.com
        apiTokenSecretRef:
          name: cloudflare-api-token-secret
          key: api-token
   - http01:
       ingress:
         class:  nginx
```

1. Aplicar a configuração do cluster issuer

```shell
kubectl apply -f cloudflare-api-token.yaml
kubectl apply -f clusterissuer.yaml
```

### Instalar/Configurar nginx-ingress
1. Clonar o repositorio de charts

```
mkdir apps
cd apps
git clone https://github.com/helm/charts.git
mv charts/stables/nginx-ingress .
```

1. Alterar o arquivo values.yaml de LoadBalance para ClusterIP

2. Dentro do nginx-ingress criar namespace e depois install do helm-charts nginx-ingress

```
cd nginx-ingress
kubectl create ns nginx-ingress
helm install -f values.yaml -n nginx-ingress nginx-ingress stable/nginx-ingress
```