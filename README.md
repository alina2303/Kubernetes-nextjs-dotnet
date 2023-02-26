## Make images for next in the folder of a project

Build container 

```shell
docker build -t nextjs-docker .
```

Add tag and push to docker container registry

```shell
docker image tag nextjs-docker:latest alina8o/my-nextjs:latest
docker image push alina8o/my-nextjs:latest
```

Change pod

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nextjs
  labels:
    app: nextjs
spec:
  containers:
    - name: nextjs
      image: 'alina8o/my-nextjs:latest'
```

Change service. Use the same port as in Dockerfile. App name has to be the same as in pod, metadata.name in pod has to match spec.selector.app in service

```yaml
kind: Service
apiVersion: v1
metadata:
  name: nextjs-service
spec:
  selector:
    app: nextjs
  ports:
    - port: 3000
```

Make ingress. metadata in service has to match backend.service.name, and ports should match as well

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nextjs-asp-ingress
spec:
  rules:
    - http:
        paths:
          - pathType: Prefix
            path: /
            backend:
              service:
                name: nextjs-service
                port:
                  number: 3000
---
```

Applay context of yaml file (navigate where your ingress file is) 

```shell
kubectl apply -f ingress.yaml
```

To access INGRESS run 
```shell
minikube tunnel
```

To see the logs. If you do not have logger in the service you should add it

```shell
kubectl logs name-of-the-pod
```






