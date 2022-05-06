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

Watch for pod creation process
```bash
kubectl get pods --watch
```

Watch a POD (live) log as you're in its terminal - For a POD with only 1 container -
```bash
kubectl logs -f <POD-NAME>
```

Watch a POD (live) log as you're in its terminal - For a POD with multiple containers -
```bash
kubectl logs -f <POD-NAME> -c <CONTAINER-NAME>
```

Create and run a pod called nginx, and create a service type ClusterIP to expose in tcp/80 port
```bash
kubectl run nginx --image=nginx --port=80 --expose
```

Create and run a Redis pod, using a Redis Alpine image, include a label `tier: db`. Also, expose the pod using a ClusterIP kind of service (default).
```bash
kubectl run redis --image=redis:alpine --labels=tier=db

kubectl expose pod redis --name redis-service --port 6379 --target-port 6379
```

Generate POD manifest YAML file with `-o`, don't run it `--dry-run=client`, and save to a file `> nginx-pod.yaml`.
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml
```

## COMMANDS FOR NODES

Show node labels
```bash
kubectl get nodes --show-labels

kubectl describe nodes <NODE NAME> | grep -i -A10 labels
```

Add a label color=blue to a specific node
```bash
kubectl label nodes <NODE NAME> color=blue
```

Add a taint to a node
```bash
kubectl taint nodes node1 size=small:NoSchedule
```

To remove a taint from a node, put the "-" in front of the key/value:taint
```bash
kubectl taint nodes node1 size=small:NoSchedule-
```

:dart: [More taint commands at kubernetes.io](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint)

## COMMANDS FOR DEPLOYMENTS

Deployment Strategies: 
- Recreate
- Rolling Update(default)

Show deployments
```bash
kubectl get deployments
```

Create a deployment using a YAML file (IMPERATIVE)
```bash
kubectl create -f deployment-definition.yaml
```

Create a deployment using a YAML file (DECLARATIVE)
```bash
kubectl apply -f deployment-definition.yaml
```

Change/set a deployment image for a template spec container
```bash
kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
```

Show the current deployment status of a deployment
```bash
kubectl rollout status deployment/myapp-deployment
```

Show the rollout history and revisions of a deployment
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

# Metrics Server

:dart: [metrics-server@github](https://github.com/kubernetes-sigs/metrics-server)

:dart: [Linux Watch Command](https://linuxize.com/post/linux-watch-command/)

Get nodes CPU and memory utilization
```bash
kubectl top node

watch "kubectl top node"
```

Get PODs CPU and memory utilization
```bash
kubectl top pods

watch "kubectl top pods"
```

# ConfigMaps

:dart: `cm is the abbreviation to ConfigMap`

Show ConfigMaps configured in the system
```bash
kubectl get configmap
```

Describe a ConfigMap
```bash
kubectl describe configmap <NAME>

kubectl get configmap <NAME> -o yaml
```

Create a new config map named my-config based on folder bar
```bash
kubectl create configmap my-config --from-file=path/to/bar
```
  
Create a new config map named my-config with specified keys instead of file basenames on disk
```bash
kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt
```
  
Create a new config map named my-config with key1=config1 and key2=config2
```bash
kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

Create a new config map named my-config from the key=value pairs in the file
```bash
kubectl create configmap my-config --from-file=path/to/bar
```

Create a new config map named my-config from an env file
```bash
kubectl create configmap my-config --from-env-file=path/to/foo.env --from-env-file=path/to/bar.env
```

# OTHERS

List events and sources (Good to see which schedule provisioned a POD for example)
```bash
kubectl get events
```

View scheduler logs
```bash
kubectl logs <POD-NAME>

kubectl logs <SCHEDULER-NAME> --n kube-system
```
