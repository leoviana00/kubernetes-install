# Kubernetes Dashboard

Deploying the Dashboard UI

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
```

# Usuário Admin Panel

Criar um arquivo:
```
sudo nano kubernetes-dashboard-service.yaml
```

Conteúdo do arquivo:

```
#Criando usuário admin
#Atribui a função de administrador de cluster
#Cria um novo serviço NodePort que publica o TargetPort 8443 como NodePort #30002:

--- 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
--- 
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
---   
kind: Service
apiVersion: v1
metadata:
  namespace: kubernetes-dashboard
  name: kubernetes-dashboard-service-np
  labels:
    k8s-app: kubernetes-dashboard
spec:
  type: NodePort
  ports:
  - port: 8443
    nodePort: 30002
    targetPort: 8443
    protocol: TCP
  selector:
    k8s-app: kubernetes-dashboard
```

Deploy usuário
```
kubectl apply -f kubernetes-dashboard-service.yaml
```
Pegando o token de acesso ao dashboard
```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

Login
```
https://ip-do-cluster:30002/#/login
```

