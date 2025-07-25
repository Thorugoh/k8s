# 🐳 Kubernetes Study Guide

This repo documents my hands-on study of Kubernetes, showcasing how each abstraction works: Pods, ReplicaSets, Deployments, and Services. Commands are real, outputs are included, and each step builds on the last.

---

## 📚 Table of Contents

* [Pods](#pods)
* [ReplicaSets](#replicasets)
* [Deployments](#deployments)
* [Updating Images](#updating-images)
* [Services & Load Balancing](#services--load-balancing)

> 📓 Want the full terminal output and study notes? Check out [STUDY\_LOG.md](./STUDY_LOG.md)

---

## 🔹 Pods

```bash
kubectl apply -f pod.yaml
kubectl get pods
kubectl delete pod nginx
```

> A Pod is the smallest deployable unit in K8s.

After deletion, the pod is gone since nothing is managing it.

---

## 🔸 ReplicaSets

ReplicaSet ensures a specified number of pod replicas are running.

```bash
kubectl apply -f replicaset.yaml
kubectl get rs
kubectl get pods
kubectl delete pod <pod-name>
```

Update to 10 replicas:

```bash
kubectl apply -f replicaset.yaml
kubectl get rs
kubectl get pods
```

Port-forward to access one of the pods:

```bash
kubectl port-forward pod/<pod-name> 8080:80
```

---

## 🧱 Deployments

A Deployment manages ReplicaSets and supports updates, rollbacks, and scaling.

```bash
kubectl delete rs nginx-replicaset
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get rs
kubectl get pods
```

---

## 🔄 Updating Images

When changing the image in `deployment.yaml`:

```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl get rs
```

> K8s spins up a new ReplicaSet and gradually replaces the old pods.

To rollback (revert image), re-apply the previous `deployment.yaml`.

---

## 🌐 Services & Load Balancing

Pods have dynamic IPs. Use a Service to expose them under a stable name.

```bash
kubectl apply -f service.yaml
kubectl get svc
kubectl port-forward svc/nginx-service 8080:80
```

> Requests to `localhost:8080` are routed using round-robin to multiple pods.

Access by service name inside the cluster:

```bash
curl http://nginx-service
```

### Exposing Services Externally

Change service type to `LoadBalancer`:

```yaml
spec:
  type: LoadBalancer
```

Reapply the service:

```bash
kubectl apply -f service.yaml
kubectl get svc
```

> In local clusters, external IP will be `<pending>`. On cloud providers (e.g., AWS), a public IP will be provisioned.

---

## 🧠 Summary

This repo walks through essential K8s building blocks in order:

* Pod → ReplicaSet → Deployment → Service
* You learn how K8s maintains desired state, performs rolling updates, and balances traffic

---

## 🛠 Requirements

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- A local Kubernetes cluster (e.g. [kind](https://kind.sigs.k8s.io/), Docker Desktop, or your preferred setup)
