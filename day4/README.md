# Roche-EKS

### Creating pod with script 

```
ubectl  create -f day4/ashulogpod.yaml 
pod/ashulogpod created
➜  Roche-EKS git:(master) ✗ kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
app          1/1     Running   0          157m
ashulogpod   1/1     Running   0          8s
```

### checking data

```
 Roche-EKS git:(master) ✗ kubectl  get po
NAME         READY   STATUS    RESTARTS   AGE
app          1/1     Running   0          158m
ashulogpod   1/1     Running   0          57s
➜  Roche-EKS git:(master) ✗ kubectl  exec -it ashulogpod  -- sh 
/ # cd /mnt/
/mnt # ls
time.txt
/mnt # cat time.txt 
Thu Apr 18 05:30:33 UTC 2024
Thu Apr 18 05:30:38 UTC 2024
Thu Apr 18 05:30:43 UTC 2024
Thu Apr 18 05:30:48 UTC 2024
```

### Storage in k8s 

<img src="st1.png">

### using volume 

```
 day4 git:(master) ✗ kubectl replace -f ashulogpod.yaml --force
pod "ashulogpod" deleted
pod/ashulogpod replaced
➜  day4 git:(master) ✗ kubectl  get  po
NAME         READY   STATUS    RESTARTS   AGE
app          1/1     Running   0          3h54m
ashulogpod   1/1     Running   0          8s
➜  day4 git:(master) ✗ kubectl  exec -it  ashulogpod  -- sh 
/ # cd /mnt/data/
/mnt/data # ls
time.txt
/mnt/data # cat  time.txt 
Thu Apr 18 06:46:54 UTC 2024
Thu Apr 18 06:46:59 UTC 2024
Thu Apr 18 06:47:04 UTC 2024
Thu Apr 18 06:47:09 UTC 2024
Thu Apr 18 06:47:14 UTC 2024
Thu Apr 18 06:47:19 UTC 2024
/mnt/data # exit
➜  day4 git:(master) ✗ kubectl  replace -f ashulogpod.yaml --force 
pod "ashulogpod" deleted
pod/ashulogpod replaced
➜  day4 git:(master) ✗ kubectl  exec -it  ashulogpod  -- sh        
/ # cat  /mnt/data/time.txt 
Thu Apr 18 06:46:54 UTC 2024
Thu Apr 18 06:46:59 UTC 2024
Thu Apr 18 06:47:04 UTC 2024
Thu Apr 18 06:47:09 UTC 2024
Thu Apr 18 06:47:14 UTC 2024
Thu Apr 18 06:47:19 UTC 2024
Thu Apr 18 06:47:24 UTC 2024
Thu Apr 18 06:47:29 UTC 2024
Thu Apr 18 06:47:34 UTC 2024
Thu Apr 18 06:47:39 UTC 2024
Thu Apr 18 06:47:44 UTC 2024
Thu Apr 18 06:47:49 UTC 2024
Thu Apr 18 06:47:54 UTC 2024
Thu Apr 18 06:47:59 UTC 2024
Thu Apr 18 06:48:04 UTC 2024
Thu Apr 18 06:48:09 UTC 2024
Thu Apr 18 06:48:14 UTC 2024
Thu Apr 18 06:48:19 UTC 2024
Thu Apr 18 06:48:26 UTC 2024
Thu Apr 18 06:48:31 UTC 2024
Thu Apr 18 06:48:36 UTC 2024
Thu Apr 18 06:48:41 UTC 2024
/ # exit
```

