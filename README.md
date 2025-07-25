## Commands
➜  k8s kubectl apply -f pod.yaml -> Apply manifest on k8s
pod/nginx created

➜  k8s kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          55s

➜  k8s kubectl delete pod nginx
pod "nginx" deleted

➜  k8s kubectl get pods
No resources found in default namespace.

### Adding a new layer to manager pods:
- Replica set, manages our pods and verifying if pods are on, if they off, replica set will recreat it to us.
- How does it works: It uses pod label to know what to look for

➜  k8s git:(main) ✗ kubectl apply -f replicaset.yaml
replicaset.apps/nginx-replicaset created

➜  k8s git:(main) ✗ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   1         1         1       7s

➜  k8s git:(main) ✗ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-z9qrk   1/1     Running   0          45s

- Now if we try to delete the pod replica set will automatically recreat it since replicaset is the one who manages it. And it wants to have at least 1 up.

➜  k8s git:(main) ✗ kubectl delete pod nginx-replicaset-z9qrk 
pod "nginx-replicaset-z9qrk" deleted

➜  k8s git:(main) ✗ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-4qf88   1/1     Running   0          6s

- Apply 10 replicas instead

➜  k8s git:(main) kubectl apply -f replicaset.yaml
replicaset.apps/nginx-replicaset configured

➜  k8s git:(main) ✗ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   10        10        2       6m59s

➜  k8s git:(main) ✗ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   10        10        10      7m20s

➜  k8s git:(main) ✗ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-4qf88   1/1     Running   0          4m37s
nginx-replicaset-5gzpz   1/1     Running   0          41s
nginx-replicaset-7glfp   1/1     Running   0          41s
nginx-replicaset-8zgjl   1/1     Running   0          41s
nginx-replicaset-c2nhp   1/1     Running   0          41s
nginx-replicaset-dvbgb   1/1     Running   0          41s
nginx-replicaset-fzhw5   1/1     Running   0          41s
nginx-replicaset-lm4jz   1/1     Running   0          41s
nginx-replicaset-ppx6g   1/1     Running   0          41s
nginx-replicaset-wgzd5   1/1     Running   0          41s