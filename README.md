## Steps to deploy a simple GoApp on kubernetes. 
### This document is specifically prepared to deploy GoApp on 


#### Required source code is stored in `src` directory.
    * A **main.go** file for simple webApp.
    * A `Dockerfile` to generate required container image.

#### Required kubernetes resource configurations are stored in `k8s` directory.

    - A `go_app_deployment.yaml` to deploy GoApp. 
    - A `go_lb_service.yaml` to create kubernetes service of `type=LoadBalancer`

*Note: A docker image has been pushed to public docker.io repository. please change/update the container image location if you have prefer so.*