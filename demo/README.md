# Strange Loop 2015

## Hello World

```
cd ~/go/src/github.com/kelseyhightower/helloworld/
```

```
less helloworld.go
```

```
go build .
```

```
sudo ./helloworld
```

```
curl http://127.0.0.1
```

```
curl http://127.0.0.1/version
```

```
curl -i http://127.0.0.1/healthz
```

```
redis-server
```

### Building Containers with Docker

#### Building for Linux

```
GOOS=linux go build .
```

#### Creating Docker Image

```
cat Dockerfile
```

```
docker build -t b.gcr.io/kuar/helloworld:1.0.0 .
```

```
docker images
```

## Deploying Applications

```
cat rc/helloworld-v1.yaml
```

```
kubectl create -f rc/helloworld-v1.yaml
```

```
kubectl describe rc helloworld-v1
```

```
kubectl get pods
```

```
kubectl describe pods <pod>
```

```
curl http://<pod-ip>
```

### Port Forwarding

#### Terminal 1

```
kubectl port-forward -p <pod> 10000:80
```

#### Terminal 2

```
curl http://127.0.0.1:10000
```

### Running Adhoc Commands

```
kubectl exec <pod> -c redis -- uname -a
```

### Logs

```
kubectl logs -f <pod> redis
```

## Lets talk about Resource utilization

Tetris!

## Resources

```
kubectl describe quota strangeloop
```

```
kubectl describe limits strangeloop
```

```
kubectl get pods
```

```
kubectl describe pods <pod>
```

## Scale out

```
kubectl scale rc helloworld-v1 --replicas=5
```

## Services

```
cat svc/helloworld.yaml
```

```
kubectl create -f svc/helloworld.yaml
```

```
kubectl describe svc helloworld
```

```
kubectl scale rc helloworld-v1 --replicas=10
```

```
kubectl describe svc helloworld
```

```
kubectl scale rc helloworld-v1 --replicas=5
```

```
kubectl describe svc helloworld
```

## Canary Deployment Pattern

```
cat rc/helloworld-canary.yaml
```

```
kubectl create -f rc/helloworld-canary.yaml
```

```
gcloud compute instances list
```

```
while true; do curl http://<node-public-ip>:36000/version; sleep .5; done
```

### Canary Service

```
cat svc/helloworld-canary.yaml
```

```
kubectl create -f svc/helloworld-canary.yaml
```

```
kubectl describe svc helloworld-canary
```

```
while true; do curl http://<node-public-ip>:36001/version; sleep .5; done
```

## Label Queries

```
kubectl get svc
```

```
kubectl get pods -l app=helloworld,track=stable
```

```
kubectl get pods -l app=helloworld,track=canary
```

## Rolling Updates

#### Terminal 1

```
kubectl rolling-update --update-period=500ms -f rc/helloworld-v2.yaml helloworld-v1
```

#### Terminal 2

```
while true; do curl http://<node-public-ip>:36000; sleep .5; done
```
