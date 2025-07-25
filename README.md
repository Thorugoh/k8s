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

- Forwards port 8080 from computer to port 80 of the pod

➜  k8s git:(main) kubectl port-forward pod/nginx-replicaset-4qf88 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080

- Accessing localhost:8080 will give access to nginx on the pod

### Adding Deployment
- Deployment creates replica set based on a spec, if that spec changes, it creates a new replica set with the new config, and deletes the replica set with old configs.
- Deployment will be responsible for deleting the old pods and creating new ones.

- First we delete the old replica set

➜  k8s git:(main) kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   10        10        10      17m

➜  k8s git:(main) ✗ kubectl delete rs nginx-replicaset
replicaset.apps "nginx-replicaset" deleted

➜  k8s git:(main) ✗ kubectl get pods
No resources found in default namespace.

- Lets apply deployment

➜  k8s git:(main) ✗ kubectl apply -f deployment.yaml
deployment.apps/nginx-deployment created

➜  k8s git:(main) ✗ kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   10/10   10           10          25s

➜  k8s git:(main) ✗ kubectl get rs
NAME                        DESIRED   CURRENT   READY   AGE
nginx-deployment-96b9d695   10        10        10      37s

➜  k8s git:(main) ✗ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
nginx-deployment-96b9d695-242ns   1/1     Running   0          51s
nginx-deployment-96b9d695-47jjv   1/1     Running   0          51s
nginx-deployment-96b9d695-5wlpz   1/1     Running   0          51s
nginx-deployment-96b9d695-6xgkj   1/1     Running   0          51s
nginx-deployment-96b9d695-7xlf5   1/1     Running   0          51s
nginx-deployment-96b9d695-h5gz5   1/1     Running   0          51s
nginx-deployment-96b9d695-ktcdc   1/1     Running   0          51s
nginx-deployment-96b9d695-mvwzx   1/1     Running   0          51s
nginx-deployment-96b9d695-qx5hr   1/1     Running   0          51s
nginx-deployment-96b9d695-r972f   1/1     Running   0          51s

## Chaning image on deployment
- Now if we change the image on deployment.yaml we will see that it is terminagint some pods and creating new ones with the new image, we will also have to replica sets created.

➜  k8s git:(main) ✗ kubectl apply -f deployment.yaml
deployment.apps/nginx-deployment configured

k8s git:(main) ✗ kubectl get pods
NAME                               READY   STATUS              RESTARTS   AGE
nginx-deployment-794547544-2rtjw   0/1     ContainerCreating   0          9s
nginx-deployment-794547544-dlmd2   0/1     ContainerCreating   0          9s
nginx-deployment-794547544-fv96k   0/1     ContainerCreating   0          9s
nginx-deployment-794547544-pd4ts   0/1     ContainerCreating   0          9s
nginx-deployment-794547544-q8pvx   0/1     ContainerCreating   0          9s
nginx-deployment-96b9d695-242ns    1/1     Running             0          3m30s
nginx-deployment-96b9d695-47jjv    1/1     Running             0          3m30s
nginx-deployment-96b9d695-5wlpz    1/1     Running             0          3m30s
nginx-deployment-96b9d695-6xgkj    1/1     Running             0          3m30s
nginx-deployment-96b9d695-7xlf5    1/1     Running             0          3m30s
nginx-deployment-96b9d695-h5gz5    1/1     Running             0          3m30s
nginx-deployment-96b9d695-ktcdc    1/1     Running             0          3m30s
nginx-deployment-96b9d695-qx5hr    1/1     Running             0          3m30s

➜  k8s git:(main) ✗ kubectl get rs
NAME                         DESIRED   CURRENT   READY   AGE
nginx-deployment-794547544   10        10        10      33s
nginx-deployment-96b9d695    0         0         0       3m54s

- If we revert the image it will "drain" the pods running from 1 rs to the old one

➜  k8s git:(main) ✗ kubectl apply -f deployment.yaml
deployment.apps/nginx-deployment configured

➜  k8s git:(main) ✗ kubectl get pods
NAME                               READY   STATUS              RESTARTS   AGE
nginx-deployment-794547544-9ncw9   1/1     Running             0          4m28s
nginx-deployment-794547544-dlmd2   1/1     Running             0          4m43s
nginx-deployment-794547544-fv96k   1/1     Running             0          4m43s
nginx-deployment-794547544-j2knm   1/1     Terminating         0          4m31s
nginx-deployment-794547544-pd4ts   1/1     Running             0          4m43s
nginx-deployment-794547544-ptmw8   1/1     Running             0          4m30s
nginx-deployment-794547544-q8pvx   1/1     Running             0          4m43s
nginx-deployment-96b9d695-6wzcz    0/1     ContainerCreating   0          3s
nginx-deployment-96b9d695-b5l6v    1/1     Running             0          3s
nginx-deployment-96b9d695-dfcws    1/1     Running             0          3s
nginx-deployment-96b9d695-kjtgt    0/1     ContainerCreating   0          1s
nginx-deployment-96b9d695-lm4tk    0/1     ContainerCreating   0          0s
nginx-deployment-96b9d695-p6w6w    0/1     ContainerCreating   0          3s
nginx-deployment-96b9d695-wc85z    0/1     ContainerCreating   0          3s

➜  k8s git:(main) ✗ kubectl get rs                  
NAME                         DESIRED   CURRENT   READY   AGE
nginx-deployment-794547544   0         0         0       4m50s
nginx-deployment-96b9d695    10        10        8       8m11s

