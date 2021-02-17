# k3s Test

## Deploying objects to k3s

```sh
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/nginx.yaml
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/curler.yaml
kubectl apply -f https://raw.githubusercontent.com/waggle-sensor/k3s-test/main/mcs-curler.yaml
```

You should see something like:

```sh
service/nginx created
deployment.apps/nginx created

deployment.apps/curler created

deployment.apps/mcs-curler created

```

## Checking pod status

```sh
kubectl get pod
```

You should see something like:

```sh
NAME                          READY   STATUS              RESTARTS   AGE
nginx-76ccf9dd9d-lrmrb        0/1     ContainerCreating   0          12s
curler-6d9cb768b-dxpjd        0/1     ContainerCreating   0          12s
mcs-curler-84978d6986-nf4ff   0/1     ContainerCreating   0          12s
```

If the images are pulled successfully, you should see the status change to `Running`.

```sh
NAME                          READY   STATUS    RESTARTS   AGE
curler-6d9cb768b-dxpjd        1/1     Running   0          58s
mcs-curler-84978d6986-nf4ff   1/1     Running   0          58s
nginx-76ccf9dd9d-lrmrb        1/1     Running   0          58s
```

## Checking logs

You should be seeing GET request logs showing up from the nginx service.

```sh
kubectl logs -f svc/nginx
```

```text
10.42.0.3 - - [17/Feb/2021:16:02:03 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"
10.42.0.3 - - [17/Feb/2021:16:02:04 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"
10.42.0.3 - - [17/Feb/2021:16:02:05 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"
10.42.0.3 - - [17/Feb/2021:16:02:06 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"
10.42.0.3 - - [17/Feb/2021:16:02:07 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"
```

You should be seeing wget 

```sh
kubectl logs -f deployment/curler
```

```sh
kubectl logs -f deployment/mcs-curler
```
