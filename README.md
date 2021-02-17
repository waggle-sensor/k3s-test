# k3s Test

## Deploying objects to a running k3s cluster

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

## Checking nginx / curler logs for local connectivity

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

You should be seeing wget logs here

```sh
kubectl logs -f deployment/curler
```

```text
Connecting to nginx (10.43.26.24:80)
saving to '/dev/null'
null                 100% |********************************|   612  0:00:00 ETA
'/dev/null' saved
ok <-- look for ok
```

## Checking nginx / curler logs for internet connectivity

```sh
kubectl logs -f deployment/mcs-curler
```

```text
Connecting to apache.org (207.244.88.140:80)
saving to '/dev/null'
null                 100% |********************************| 86012  0:00:00 ETA
'/dev/null' saved
ok <-- look for ok
```

(Yes, I know it's called mcs-curler but is hitting apache.org... I didn't feel like renaming everything.)
