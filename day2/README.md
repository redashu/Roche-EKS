# Roche-EKS

### EKS info 

<img src="eks.png">

### CNI info 

<img src="cni.png">

### Intro to controller in eks

<img src="eks11.png">

### Introduction to controller 

<img src="deploy.png">

### New apiVersion for RS & deployment 

<img src="apiv.png">

### Creating YAML for Deployment 

```
kubectl  create   deployment  ashu-deploy  --image=nginx  --port 80 --dry-run=client -o yaml

```

### Creating deployment 

```
 ashu-k8s-manifest git:(master) ✗ kubectl  create  -f  ashudeploy.yaml 
deployment.apps/ashu-deploy created
➜  ashu-k8s-manifest git:(master) ✗ kubectl  get deploy
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
ajaychavda        1/1     1            1           2s
akanksha-deploy   0/1     1            0           1s
ashu-deploy       1/1     1            1           6s
auto-deploy       0/1     1            0           1s
jai-deploy        0/1     1            0           1s
vj-deployment     1/1     1            1           2s
➜  ashu-k8s-manifest git:(master) ✗ kubectl  get  rs   
NAME                         DESIRED   CURRENT   READY   AGE
ajaychavda-74fc465dd8        1         1         1       10s
akanksha-deploy-6d6fb58df6   1         1         1       9s
ashu-deploy-676d64ddbf       1         1         1       14s
auto-deploy-6756b45fd5       1         1         1       9s
jai-deploy-f95879f8          1         1         1       9s
rp-deployment1-5c86756969    1         1         1       7s
vj-deployment-94b9b7b98      1         1         1       10s
➜  ashu-k8s-manifest git:(master) ✗ kubectl  get  pod 
NAME                               READY   STATUS    RESTARTS   AGE
ajaychavda-74fc465dd8-fkxkg        1/1     Running   0          32s
akanksha-deploy-6d6fb58df6-9kcld   1/1     Running   0          31s
ashu-deploy-676d64ddbf-hbgwd       1/1     Running   0          36s
auto-deploy-6756b45fd5-4g5sj       1/1     Running   0          31s
jai-deploy-f95879f8-q4dp2          1/1     Running   0          31s
nci-nginx-c5d7b4bf5-jkn9q          1/1     Running   0          17s
nci-nginx-c5d7b4bf5-w6xj2          1/1     Running   0          17s
niranjan-deploy-64765f4769-2fpjl   1/1     Running   0          14s
ranjith-deploy-99b4b894b-xs8ml     1/1     Running   0          7s
rp-deployment1-5c86756969-c4f4r    1/1     Running   0          29s
```

### scaling pod 

<img src="scale.png">


### manual horizent pod scaling by Deployment 

```
kubectl  scale deployment ashu-deploy  --replicas=3
deployment.apps/ashu-deploy scaled
➜  ashu-k8s-manifest git:(master) ✗ kubectl get deploy
NAME              READY    UP-TO-DATE   AVAILABLE   AGE
ajaychavda        1/1      1            1           12m
ajays-deploy      1/1      1            1           9m37s
akanksha-deploy   1/1      1            1           12m
ashu-deploy       3/3      3            3           12m
jai-deploy        2/2      2            2           12m
myapp             14/100   100          14          6m54s
```

### Kubernets LB 

<img src="lb1.png">

### type of service in K8s 

```
kubectl   create  service                      
Create a service using a specified subcommand.

Aliases:
service, svc

Available Commands:
  clusterip      Create a ClusterIP service
  externalname   Create an ExternalName service
  loadbalancer   Create a LoadBalancer service
  nodeport       Create a NodePort service

Usage:
  kubectl create service [flags] [options]

Use "kubectl create service <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

### Creating External & itnerlb 


```
 kubectl  expose  deployment  ashu-deploy --type LoadBalancer --port 80 --name ashulb1 --dry-run=client -o yaml 

apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashu-deploy
  name: ashulb1
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: ashu-deploy
  type: LoadBalancer
status:
  loadBalancer: {}
➜  ashu-k8s-manifest git:(master) ✗ kubectl  expose  deployment  ashu-deploy --type LoadBalancer --port 80 --name ashulb1 --dry-run=client -o yaml 
 >ashulbnew1.yaml 
➜  ashu-k8s-manifest git:(master) ✗ kubectl create -f ashulbnew1.yaml 
service/ashulb1 created
➜  ashu-k8s-manifest git:(master) ✗ 

```

### checking service 

```
 kubectl  get service
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)        AGE
ashulb1      LoadBalancer   10.100.194.211   ad1a64a22e9584e5f9b978bef93bf0c0-1418892613.us-east-1.elb.amazonaws.com   80:31234/TCP   119s
jailb        LoadBalancer   10.100.168.186   a47eab2e4688546c082880702ac6527b-1186562089.us-east-1.elb.amazonaws.com   80:31851/TCP   97s
kubernetes   ClusterIP      10.100.0.1       <none>                                                                    443/TCP        69m
lbname       LoadBalancer   10.100.115.48    <pending>                                                                 80:32764/TCP   2s
shivrajlb1   ClusterIP      10.100.195.222   <none>                                                                    80/TCP         64s
vishalb      LoadBalancer   10.100.221.77    aaabe799275b44b61a14d469b4367906-2108821066.us-east-1.elb.amazonaws.com   80:31155/TCP   42s
```

