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

