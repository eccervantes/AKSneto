
# AKS - Kubectl From Zero to Hero
As a prerequisite you will have a kubernetes cluster created already on azure or any other cloud provider or mikikube etc.


## Get the configuration files and then export the path into the KUBECONFIG environment variable.

- `az aks get-credentials --resource-group terraform --name AKSneto --file $PWD/config`

- `export KUBECONFIG=$PWD/config`

## Verify your configuration file works 

- `kubectl get nodes`

## Check your namespaces 

- `kubectl get namespaces` or `kubectl get ns`

## Target an specific namespace

- `kubectl -n kube-system get pods`
- `kubectl -n kube-system get pods -o wide`

## Delete a pod

- `kubectl -n kube-system delete <PODNAME>`

    if a Name Space is not specified it will execute the task in the default namespace.
---
## **Using Declarative files (YAML)**
---
## Creating a pod from a file manifest. 

- `kubectl apply -f 1podManifestEpam.yaml`

## Deleting a pod from a file manifest

- `kubectl delete -f 1podManifestEpam.yaml`

## Check for logs on a pod

- run the manifest "3errorPodlog.yaml"
- check the pod status, you will se there is only one container up. 
- verify the logs for an specific container with the following syntax

    `kubectl logs <PODNAME> <CONTAINERNAME>`

    The "-f" flags is used to check the log in real time

    `kubectl logs -f multicontainer nginxibm`

## execute a command on a running container
`kubectl exec -it <CONTAINER-NAME> -- sh`

`kubectl exec -it nginxepam -- sh`

## Apply a Deployment on Kubernetes

A deployment will make sure all the pods are up and running according to the solution you specify, it will recreate the pods that error out, deleted, or crashed. 
The total numer of replicas will be placed within the total nodes of the kluster. 

`kubectl apply -f 4deploymentepam.yaml`


## Deamon set

A Deamon set will have one running pod **on each node of the kubernetes kluster**, so deamont set replicas will be the same as nodes. 


## Statefull Set


It is similar to a deployment but including a volumen to make the data persistent, for example a DB. a volume is necessary in order to achieve the persistance of the data. PVC.

---
## **Services**

for this section we will create a ubuntu pod that could help us troubleshoot how the differente type of services are working. 

you can use the depoyment file "5Ubuntupod.yaml" or

use a manual run of the image.

    - `kubectl apply -f 5Ubuntupod.yaml`
    - `kubectl run ubuntu --image ubuntu -- sleep infinity`
  Make sure you have the iputils-ping and curl package install 
---

- **Cluster IP**
  
  We will use the following file to create a depoyment that already contains a service cluster ip. 


  `kubectl apply -f 6hello-deployment-svs-clusterIP.yaml`
  
  We can verify that the endpoints of the svc are the pods with their respective port defined. 

  `kubectl get all`

  `kubectl describe svc hello | grep Endpoints`
---
- **Node Port**
---
- **load Balancer**
---
## Details on the pods yamls

- env 
- readiness probe
- liveness probes
- obtaining information about your pods
  - `kubectl get pod -o yaml`
   
<!--- this is a comment>