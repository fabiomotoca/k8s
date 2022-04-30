# MY K8S PLAYGROUND

## COMMANDS FOR PODs

Create and run a nginx pod
```bash
kubectl run nginx --image=nginx
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

