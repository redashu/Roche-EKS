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
