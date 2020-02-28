# How to install wiki.js via Kubuernetes in macOS

Create wikijs.yml with below content

```text
apiVersion:v1
kind:Pod
metadata:
name:wikijs
labels:
env:dev
spec:
containers:
-name:wikijs
image:requarks/wiki:beta
ports:
-containerPort:3000
env:
-name:DB_TYPE
value:"sqlite"
-name:DB_FILEPATH
value:"/tmp/wiki.db"
```

And then run

```text
kubectl apply -f ./wikijs.yml
```

By default, you’re allow to access Pod from host network. You will have to expose Pod to host. To do that, you need port forwarding. Run

```text
kubectl port-forward wikijs 3000:3000
```

