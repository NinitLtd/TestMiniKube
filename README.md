# Steps to deploy a simple GoApp on kubernetes (MiniKube). 

## Required source code is stored in `src` directory.

- A `main.go` file for simple webApp.
- A `Dockerfile` to generate required container image. 
- To build docker image use this command -> `docker build -t barotn/hellogo:latest`. 
**Note: The `barotn` is my userid on `hub.docker.com` AND `hellogo` is image name with `latest` tag**
- To push docker image to `hub.docker.com` run -> `docker push barotn/hellogo:latest`.

**Note: A docker image has been pushed to public docker repository for this PoC. please change/update the container image location if you prefer so.**

## Required kubernetes resource configurations are stored in `k8s` directory.

- A `go_app_deployment.yaml` to deploy GoApp. 
- A `go_lb_service.yaml` to create kubernetes service of `type=LoadBalancer`. 

## Deployment steps:
**Prerequisites:**
- You will need a working `minikube` local environment to deploy this application. I've tested with `minikube version: v1.24.0`. [MiniKube](https://minikube.sigs.k8s.io/docs/start/)
- You will also need `kubectl` commandline utility installed on your host. [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)

**Steps:**
- To deploy app run `kubecctl apply -f go_app_deployment.yaml`. This step will create a deployment in `default` namespace with 1 replicaSet and 2 Pods.
- To check status of deployment run `kubectl get deployment go-app -n default`.
```
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
go-app   2/2     2            2           9m23s
```
- To expose this go web app we will need to create a kubernetes service of type `LoadBalancer` or `NodePort`. In this example we will be creating service of type `LoadBalancer`. To create service run `kubectl apply -f go_lb_service.yaml`
- to check status of service run `kubectl get svc go-app -n default`. You will see the status of `EXTERNAL-IP` is pending. 
```
NAME     TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
go-app   LoadBalancer   10.110.62.80   <pending>     9090:31699/TCP   19s
```

- Finally from a separate terminal window we will run `minikube tunnel` which creates a foreground process and populates a `EXTERNAL-IP`.
- To access this app from CLI from your host use `curl http://<EXTERNAL-IP>:9090`

```$kubectl get svc go-app -n default
NAME     TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)          AGE
go-app   LoadBalancer   10.110.62.80   10.110.62.80   9090:31699/TCP   3m31s
```

- You should now be able to access the web app using `CURL` or `web brwser`.

```
curl http://10.110.62.80:9090
Hello, Welcome to the world of Kubernetes!
```

**Note: IMHO this is `PoC` so the build and deployment steps are manual. I would prefer to build `CI/CD` pipelines to automate build and deployment steps.**
