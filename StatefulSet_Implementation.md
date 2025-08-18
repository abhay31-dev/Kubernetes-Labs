# StatefulSet Implementation

In this lab, we will learn how to create and manage StatefulSets in Kubernetes. StatefulSets are used for applications that require stable, unique network identifiers, persistent storage, and ordered, graceful deployment and scaling.

---

## Step 1: Create a Headless Service

A headless service is required for StatefulSets to provide stable network identities.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
```

Apply it using:

```bash
kubectl apply -f service.yaml
```

---

## Step 2: Create the StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
```

Apply it using:

```bash
kubectl apply -f statefulset.yaml
```

---

## Step 3: Verify the StatefulSet

Check the created pods:

```bash
kubectl get pods -l app=nginx
```

Check the StatefulSet status:

```bash
kubectl get statefulsets
```

---

## Step 4: Scale the StatefulSet

Increase the replicas:

```bash
kubectl scale statefulset web --replicas=5
```

Verify:

```bash
kubectl get pods -l app=nginx
```

---

## Step 5: Delete the StatefulSet

Delete the StatefulSet but keep the PVCs:

```bash
kubectl delete statefulset web
```

Delete along with PVCs:

```bash
kubectl delete statefulset web --cascade=orphan
```

---

# Conclusion
StatefulSets are useful for workloads like databases, where each pod needs a unique, persistent identity and stable storage.
