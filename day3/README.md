# Roche-EKS

### Creating cluster using eksctl 

### cluster.yaml

```
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: ashueks-cluster
  region: ap-south-1 

nodeGroups:
  - name: ashu-nodegroup-1
    instanceType: t2.small
    desiredCapacity: 1
    volumeSize: 20 
    volumeType: gp2 
    minSize: 1
    maxSize: 3 

fargateProfiles:
  - name: ashu-fargate-1 
    selectors: 
      - namespace: default-ashu 
        labels:
          hello: ashu

```

### creating it

```
 eksctl  create  cluster  -f  cluster.yaml 
2024-04-17 11:19:06 [ℹ]  eksctl version 0.175.0-dev+5b28c1794.2024-03-22T12:39:36Z
2024-04-17 11:19:06 [ℹ]  using region ap-south-1
2024-04-17 11:19:06 [ℹ]  skipping ap-south-1c from selection because it doesn't support the following instance type(s): t2.small
```

## Creating 2 tier webapp

### mysql depoyment 
```
kubectl  create  deployment  ashu-db  --image=mysql  --port 3306 --dry-run=client -o yaml
```
### Creating db root pass secret

```
kubectl  create  secret   generic  ashudb-root-cred --from-literal   ashu-pass="RocheDb@098$"  --dry-run=client -o yaml
apiVersion: v1
data:

```

### Creating configmap to store db name 

```
kubectl  create  configmap  ashu-db-name  --from-literal  mydb_name=ashudbroche  --dry-run=client -o yaml
apiVersion: v1
data:

```

### Creating deployment of mysql 

```
 ashu-k8s-manifest git:(master) ✗ kubectl  apply -f  webappday3
configmap/ashu-db-name configured
deployment.apps/ashu-db created
secret/ashudb-root-cred configured
secret/ashudb-generaldb-cred configured
➜  ashu-k8s-manifest git:(master) ✗ kubectl  get  po
NAME                       READY   STATUS    RESTARTS   AGE
ashu-db-799b5db8d8-tgr7p   1/1     Running   0          77s
➜  ashu-k8s-manifest git:(master) ✗ 
```


### Creating clusterIP type service for DB 

```
kubectl  expose  deployment  ashu-db   --port 3306  --name  ashudb-lb --dry-run=client -o yaml 
apiVersion: v1
kind: Service

```
### creating deployment for webapp

```
kubectl  create  deployment  ashu-webapp --image=wordpress:6.2.1-apache  --dry-run=client -o yaml 
apiVersion: apps/v1

```

### running webapp deployment 

```
 ashu-k8s-manifest git:(master) ✗ kubectl apply -f webappday3 
configmap/ashu-db-name configured
deployment.apps/ashu-db configured
secret/ashudb-root-cred configured
secret/ashudb-generaldb-cred configured
service/ashudb-lb configured
deployment.apps/ashu-webapp created
➜  ashu-k8s-manifest git:(master) ✗ kubectl  get  po
NAME                           READY   STATUS    RESTARTS   AGE
ashu-db-799b5db8d8-tgr7p       1/1     Running   0          83m
ashu-webapp-78d669d4f9-lfj4v   1/1     Running   0          11s
➜  ashu-k8s-manifest git:(master) ✗ 
```

### Creating metric server in EKS

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

```

