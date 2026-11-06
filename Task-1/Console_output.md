~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ minikube start --nodes 3 --cni=calico --driver=docker --cpus 2 --memory 2048
😄  minikube v1.38.1 on Cachyos 
✨  Using the docker driver based on user configuration
❗  Starting v1.39.0, minikube will default to "containerd" container runtime. See #21973 for more info.
📌  Using Docker driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.50 ...
🔥  Creating docker container (CPUs=2, Memory=2048MB) ...
🐳  Preparing Kubernetes v1.35.1 on Docker 29.2.1 ...
🔗  Configuring Calico (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass

👍  Starting "minikube-m02" worker node in "minikube" cluster
🚜  Pulling base image v0.0.50 ...
🔥  Creating docker container (CPUs=2, Memory=2048MB) ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.49.2
🐳  Preparing Kubernetes v1.35.1 on Docker 29.2.1 ...
    ▪ env NO_PROXY=192.168.49.2
🔎  Verifying Kubernetes components...

👍  Starting "minikube-m03" worker node in "minikube" cluster
🚜  Pulling base image v0.0.50 ...
🔥  Creating docker container (CPUs=2, Memory=2048MB) ...
🌐  Found network options:
    ▪ NO_PROXY=192.168.49.2,192.168.49.3
🐳  Preparing Kubernetes v1.35.1 on Docker 29.2.1 ...
    ▪ env NO_PROXY=192.168.49.2
    ▪ env NO_PROXY=192.168.49.2,192.168.49.3
🔎  Verifying Kubernetes components...

❗  /usr/bin/kubectl is version 1.37.2, which may have incompatibilities with Kubernetes 1.35.1.
    ▪ Want kubectl v1.35.1? Try 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

~/MLOps/Kubernetes Course Tasks/Task-1 master* 39s
❯ kubectl get nodes
NAME           STATUS     ROLES           AGE   VERSION
minikube       NotReady   control-plane   37s   v1.35.1
minikube-m02   NotReady   <none>          21s   v1.35.1
minikube-m03   NotReady   <none>          10s   v1.35.1

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl get nodes
NAME           STATUS   ROLES           AGE     VERSION
minikube       Ready    control-plane   2m40s   v1.35.1
minikube-m02   Ready    <none>          2m24s   v1.35.1
minikube-m03   Ready    <none>          2m13s   v1.35.1

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ ls
drwxr-xr-x    - ziadabdelraouf  6 Nov 17:04  screenshots
.rw-r--r-- 1.2k ziadabdelraouf  6 Nov 16:52  deployment.yaml
.rw-r--r--   57 ziadabdelraouf  6 Nov 16:51  namespace.yaml
.rw-r--r-- 1.4k ziadabdelraouf  6 Nov 19:16 󰂺 README.md

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl apply -f namespace.yaml
namespace/webapp created

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl apply -f deployment.yaml
deployment.apps/web-deployment created

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl get pods -n webapp -w
NAME                              READY   STATUS     RESTARTS   AGE
web-deployment-7fb449bd67-f5k98   0/2     Init:0/1   0          6s
web-deployment-7fb449bd67-jr4j8   0/2     Init:0/1   0          6s
web-deployment-7fb449bd67-w8kxb   0/2     Init:0/1   0          6s
web-deployment-7fb449bd67-jr4j8   0/2     Init:0/1   0          31s
web-deployment-7fb449bd67-jr4j8   0/2     Init:0/1   0          31s
web-deployment-7fb449bd67-jr4j8   0/2     PodInitializing   0          38s
web-deployment-7fb449bd67-w8kxb   0/2     Init:0/1          0          74s
web-deployment-7fb449bd67-w8kxb   0/2     Init:0/1          0          75s
web-deployment-7fb449bd67-f5k98   0/2     Init:0/1          0          76s
web-deployment-7fb449bd67-f5k98   0/2     Init:0/1          0          77s
web-deployment-7fb449bd67-w8kxb   0/2     PodInitializing   0          80s
web-deployment-7fb449bd67-f5k98   0/2     PodInitializing   0          83s
web-deployment-7fb449bd67-jr4j8   2/2     Running           0          2m15s
web-deployment-7fb449bd67-f5k98   2/2     Running           0          2m24s
web-deployment-7fb449bd67-w8kxb   2/2     Running           0          2m27s
^C⏎                                                                                                                                                        
~/MLOps/Kubernetes Course Tasks/Task-1 master* 2m 46s
❯ kubectl port-forward -n webapp deploy/web-deployment 8081:80
Forwarding from 127.0.0.1:8081 -> 80
Forwarding from [::1]:8081 -> 80
Handling connection for 8081
^C⏎                                                                                                                                                        
~/MLOps/Kubernetes Course Tasks/Task-1 master* 28s
❯ # changed the initpod to exit 1 

~/MLOps/Kubernetes Course Tasks/Task-1 master* 28s
❯ kubectl apply -f deployment.yaml
deployment.apps/web-deployment configured

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl get pods -n webapp -w
NAME                              READY   STATUS                  RESTARTS     AGE
web-deployment-5cfb696964-lkth6   0/2     Init:CrashLoopBackOff   1 (5s ago)   6s
web-deployment-7fb449bd67-f5k98   2/2     Running                 0            4m54s
web-deployment-7fb449bd67-jr4j8   2/2     Running                 0            4m54s
web-deployment-7fb449bd67-w8kxb   2/2     Running                 0            4m54s
web-deployment-5cfb696964-lkth6   0/2     Init:Error              2 (15s ago)   16s
web-deployment-5cfb696964-lkth6   0/2     Init:CrashLoopBackOff   2 (28s ago)   44s
web-deployment-5cfb696964-lkth6   0/2     Init:Error              3 (30s ago)   46s
^C⏎                                                                                                                                                                                                                                                                              
~/MLOps/Kubernetes Course Tasks/Task-1 master* 1m 35s
❯ kubectl logs web-deployment-5cfb696964-lkth6 -n webapp -c init-done
  kubectl describe pod web-deployment-5cfb696964-lkth6 -n webapp
  kubectl rollout status deploy/web-deployment -n webapp
init failed
Name:             web-deployment-5cfb696964-lkth6
Namespace:        webapp
Priority:         0
Service Account:  default
Node:             minikube-m03/192.168.49.4
Start Time:       Fri, 06 Nov 2026 19:28:08 +0200
Labels:           app=web
                  pod-template-hash=5cfb696964
Annotations:      cni.projectcalico.org/containerID: 79708d4999d08e6ea8e3d9d9e65de50c00887280f229fc622f010909cf1bf408
                  cni.projectcalico.org/podIP: 10.244.151.2/32
                  cni.projectcalico.org/podIPs: 10.244.151.2/32
Status:           Pending
IP:               10.244.151.2
IPs:
  IP:           10.244.151.2
Controlled By:  ReplicaSet/web-deployment-5cfb696964
Init Containers:
  init-done:
    Container ID:  docker://4385dbf253d270080158f155c37cfae5c8aeba63feb58cd8282f89413263bafa
    Image:         busybox:1.36
    Image ID:      docker-pullable://busybox@sha256:73aaf090f3d85aa34ee199857f03fa3a95c8ede2ffd4cc2cdb5b94e566b11662
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      echo 'init failed'; exit 1
    State:          Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Fri, 06 Nov 2026 19:31:04 +0200
      Finished:     Fri, 06 Nov 2026 19:31:04 +0200
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Fri, 06 Nov 2026 19:29:42 +0200
      Finished:     Fri, 06 Nov 2026 19:29:42 +0200
    Ready:          False
    Restart Count:  5
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-c9xwr (ro)
Containers:
  nginx:
    Container ID:  
    Image:         nginx:1.27
    Image ID:      
    Port:          80/TCP
    Host Port:     0/TCP
    Command:
      /bin/sh
      -c
      echo "<h1>hello in MLOPS MENA COMMUNITY : $(date '+%Y-%m-%d %H:%M:%S')</h1>" > /usr/share/nginx/html/index.html
      nginx -g 'daemon off;'
      
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-c9xwr (ro)
  time-updater:
    Container ID:  
    Image:         busybox:1.36
    Image ID:      
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
      while true; do
        PID=$(pgrep -o nginx)
        if [ -n "$PID" ]; then
          echo "<h1>hello in MLOPS MENA COMMUNITY : $(date '+%Y-%m-%d %H:%M:%S')</h1>" > /proc/$PID/root/usr/share/nginx/html/index.html
        fi
        sleep 5
      done
      
    State:          Waiting
      Reason:       PodInitializing
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-c9xwr (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 False 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-c9xwr:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Scheduled  3m37s                default-scheduler  Successfully assigned webapp/web-deployment-5cfb696964-lkth6 to minikube-m03
  Normal   Pulled     42s (x6 over 3m36s)  kubelet            spec.initContainers{init-done}: Container image "busybox:1.36" already present on machine and can be accessed by the pod
  Normal   Created    41s (x6 over 3m36s)  kubelet            spec.initContainers{init-done}: Container created
  Normal   Started    41s (x6 over 3m36s)  kubelet            spec.initContainers{init-done}: Container started
  Warning  BackOff    41s (x7 over 3m35s)  kubelet            spec.initContainers{init-done}: Back-off restarting failed container init-done in pod web-deployment-5cfb696964-lkth6_webapp(5c60012f-ef37-4ee3-9a6b-20bedf0d9db3)
Waiting for deployment "web-deployment" rollout to finish: 1 out of 3 new replicas have been updated...
error: deployment "web-deployment" exceeded its progress deadline

~/MLOps/Kubernetes Course Tasks/Task-1 master* 6m 23s
❯ kubectl exec deploy/web-deployment -n webapp -c nginx -- cat /usr/share/nginx/html/index.html
<h1>hello in MLOPS MENA COMMUNITY : 2026-11-06 17:41:50</h1>

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl exec deploy/web-deployment -n webapp -c nginx -- cat /usr/share/nginx/html/index.html
<h1>hello in MLOPS MENA COMMUNITY : 2026-11-06 17:42:00</h1>

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl get deploy web-deployment -n webapp -o jsonpath='{.spec.template.spec.volumes}'

~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl apply -f deployment.yaml
deployment.apps/web-deployment configured

               
~/MLOps/Kubernetes Course Tasks/Task-1 master*
❯ kubectl get pods -n webapp -w
NAME                              READY   STATUS    RESTARTS   AGE
web-deployment-6d9b58dd8f-gtkz9   2/2     Running   0          10s
web-deployment-6d9b58dd8f-lsdvf   2/2     Running   0          8s
web-deployment-6d9b58dd8f-lwk4d   2/2     Running   0          6s
