# Instalação do Kubernetes

## Requisitos

```
- x86-64 processor
- 2CPU
- 2GB RAM
- 10GB free disk space
- RedHat Enterprise Linux 7.x+, CentOS 7.x+, Ubuntu 16.04+, or Debian 9.x+
```
## Instalação do Docker

Instalando o docker

```
# curl -fsSL https://get.docker.com | bash
```

Verificando a versão do Docker

```
docker --version
```

Defina o Docker para iniciar na inicialização digitando o seguinte

```
sudo systemctl enable docker
```

Verifique se o Docker está em execução

```
sudo systemctl status docker
```

Para iniciar o Docker se não estiver em execução

```
sudo systemctl start docker
```

## Instalação do Kubernetes

Instalando o kubeadm, kubelet and kubectl
```
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

Configure o driver cgroup usado pelo kubelet no nó do plano de controle:

```
sudo docker info | grep -i cgroup Cgroup Driver: cgroupfs

sudo sed -i "s / cgroup-driver = systemd / cgroup-driver = cgroupfs / g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

Reiniciando o Kubelet 

```
systemctl daemon-reload
systemctl restart kubelet
```

