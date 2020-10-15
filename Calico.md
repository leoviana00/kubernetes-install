# Iniciando o Cluster

Para iniciar o nosso cluster devemos desabilitar nossa swap:

```
# swapoff -a
```
E comente a entrada referente a swap em seu arquivo fstab:

```
# vim /etc/fstab
```

# Calico no Kubernetes

## Criando um cluster Kubernetes de host.

Inicialize o mestre usando o seguinte comando.

```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

Execute os comandos a seguir para configurar o kubectl (também retornado por kubeadm init).

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

# Instalando o Calico

Instale o operador Tigera Calico e as definições de recursos personalizadas

```
kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
```

Instale o Calico criando o recurso personalizado necessário. Para obter mais informações sobre as opções de configuração disponíveis neste manifesto, consulte a [referência de instalação](https://docs.projectcalico.org/reference/installation/api).

```
kubectl create -f https://docs.projectcalico.org/manifests/custom-resources.yaml
```

Confirme se todos os pods estão sendo executados com o seguinte comando.

```
watch kubectl get pods -n calico-system
```

Configurar mestre para que você possa agendar pods nele.

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

Confirme que agora você tem um nó em seu cluster com o seguinte comando.

```
kubectl get nodes -o wide
```

Ele deve retornar algo como o seguinte.

```
NAME           STATUS   ROLES    AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                       KERNEL-VERSION    CONTAINER-RUNTIME
k8s-master-1   Ready    master   16h   v1.19.3   10.0.1.132    <none>        Debian GNU/Linux 10 (buster)   4.19.0-11-amd64   docker://19.3.13
k8s-worker-1   Ready    <none>   16h   v1.19.3   10.0.1.103    <none>        Debian GNU/Linux 10 (buster)   4.19.0-11-amd64   docker://19.3.13
k8s-worker-2   Ready    <none>   16h   v1.19.3   10.0.1.129    <none>        Debian GNU/Linux 10 (buster)   4.19.0-11-amd64   docker://19.3.13
```
# Inserindo um node ao cluster cluster

Pegando o token para colocar nodes no cluster

```
kubeadm token create --print-join-command
```
O comando que vai aparecer é semelhante a esse abaixo:

```
# kubeadm join --token 39c341.a3bc3c4dd49758d5 IP_DO_MASTER:6443 --discovery-token-ca-cert-hash sha256:37092
```
