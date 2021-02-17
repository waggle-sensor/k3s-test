# k3s Test

## Deploying objects to k3s

```sh
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/nginx.yaml
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/curler.yaml
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/mcs-curler.yaml
```

## Checking pod status

```sh
kubectl get pod
```

You should see and entry corresponding to all three of these and they should end up with stauts `running`.

## Checking logs

```sh
kubectl logs -f svc/nginx
kubectl logs -f deployment/curler
kubectl logs -f deployment/mcs-curler
```
