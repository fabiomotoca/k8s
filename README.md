# MY K8S PLAYGROUND

## COMMANDS FOR PODs

Create and run a nginx pod
```bash
kubectl run nginx --image=nginx
```

Show running pods with a specific label
```bash
kubectl get pods -l env=dev

kubectl get pods -l "app=App1,function=frontend"

kubectl get pods --selector app=App1

kubectl get pods --selector "app=App1,function=frontend"
```

Create and run a pod called nginx, and create a service type ClusterIP to expose in tcp/80 port
```bash
kubectl run nginx --image=nginx --port=80 --expose
```

Create and run a redis pod, using a redis alpine image, include a label `tier: db`. Also, expose the pod using a ClusterIP kind of service (default).
```bash
kubectl run redis --image=redis:alpine --labels=tier=db

kubectl expose pod redis --name redis-service --port 6379 --target-port 6379
```

Generate POD manifest yaml file with `-o`, don't run it `--dry-run=client`, and save to a file `> nginx-pod.yaml`.
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml
```

## COMMANDS FOR DEPLOYMENTS

Show deployments
```bash
kubectl get deployments
```

Create a deployment using a yaml file (IMPERATIVE)
```bash
kubectl create -f deployment-definition.yaml
```

Create a deployment using a yaml file (DECLARATIVE)
```bash
kubectl apply -f deployment-definition.yaml
```

Change/set a deployment image
```bash
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
```

Show the current deployment status of a deployment
```bash
kubectl rollout status deployment/myapp-deployment
```

Show the rollout history of a deployment
```bash
kubectl rollout history deployment/myapp-deployment
```

Rollback a deployment to the last one
```bash
 kubectl rollout undo deployment/myapp-deployment
```

Generate Deployment YAML file `-o yaml`, but don't create it `--dry-run=client`, also save it to a file `> deployment.yaml`
```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > deployment.yaml
```

Same as above with 3 replicas in total
```bash
kubectl create deployment --image=nginx nginx --replicas=3 --dry-run=client -o yaml > nginx-deployment.yaml
```

Scale a deployment using the `kubectl scale`
```bash
kubectl scale deployment nginx --replicas=5
```

## COMMANDS FOR SERVICES

Expose a service via localhost:8443
```bash
kubectl port-forward service/myapp-service 8443:80
```

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379. Automatically select pod's labels as selectors
```bash
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
```

This will not use the pods labels as selectors, instead it will assume selectors as app=redis. You cannot pass in selectors as an option.
 So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service)
```bash
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml > svc-redis.yaml
```

Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes
```bash
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service
```

Create a service, but the command bellow won't use the pods labels as selectors
```bash
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080
```

