Test001
This is test repo

#This is just an test Part

![](Test1/Error.JPG)



==============================================================================================================================================================

==============================================================================================================================================================

## Below are some of the list of problem if we are using only Docker.

. Multiple application stacks

. Large deployments

. Deployment / Updating / Scheduling

. Replication / Availability

. Resilience

. Storage

. Ingress

## Benefit of using Kubernetes

. Availability - scale as defined in your desired state

. Resilience - if a container exits/dies, a new one is created

. Storage - Local, NFS, iSCSI, GCEP, AWS EBS and more

. Deployments - with Canary pattern

. Scheduling - with Resource Limitations

. Updates - with Rolling Updates

. Networking and Cluster DNS

. Service Discovery

. Ingress

![](Test1/Kubernetes%20Workflow.jpg)

## Below Services runs on Master

. API Server

. Control Manager

. Scheduler

. Etcd Cluster - (Is an advance and fast type of database. This keep the desire state information)

## Below Services runs on Nodes

. Kube-proxy

. Kubelet (a.k.s. agent)

. Docker

*All component communicate with only API Sever*

*Also onlyt API Server can communicate to Etcd Cluster*

*Also the user or administrator communicate to API Server*

## Pod : -

. Is a collection of containers. Which live togeather and die togeather

. The process which manage the Pod is Kubelet.

. Kube-Proxy Manage the Network communication between all the Pod

. Pod is an worker unit.

. Each Pod get an IP address

. Visialize Pod as an Virtual Machine.

. All containers in a Pod share Volume (Storage) and Networking.

## Pod Workflow :-

1. User tell Kubernetes to run a service (e.g. Tomcat)

2. Kubernetes goes to Kublet

3. Kublet communicate to Docker and request to run an container with an Image of "Pause" (Pause is an name of Google Image which is    empty, this is given by Kublet to Dockr. Pause just takes the IP from the Docker an wait. )

4. Kublet also instruct the Docker to give name to this Image "Tomcat"(As we had requested Tomcat Image) and return the IP of the container)

5. Docker give's the the IP address and Name of the Container to Kublet

6. After that Kublet gives actual Image of the application (e.g. Tomcat Service) and Instruct Docker to run this application inside the Name Space which it has recently created with the same Ip (In our case it is Tomcat)

7. Now we have two container (Pause and Tomcat Service) in the same IP and Namespace (i.e. Tomcat) (This is how Pod is created. It now contain multiple container)

8. After that again Kublet request Docker to run one more image of "Proxy" in the same IP and NameSpace (i.e. Tomcat). Proxy Container is used to Proxy the request.

9. Now we have 3 imges (Pause, Tomcat Service and Proxy)in the same NameSpace and IPAddress

10. After that again Kublet request Docker to run one more image of "init Container" in the same IP and NameSpace (i.e. Tomcat). Init Container is fist container which get started when we start a Pod, becasue it contain all the initial files and instruction (like Jar file, etc). Init container plase all the required files in the share volume of the Pod and go to ideal stage.

11. Now we have 4 imges (Pause, Tomcat Service, Proxy and Init)in the same NameSpace and IPAddress

12. Once you run command "Kubectl -p" it will should you the Tomcat 3/3. You will not see the Pause Container whene you run in Kublet beacuse itn't important. But if you log-In to the Docker and run the command then you will see all the 4 Container

### Any one container out of 4 die's the whole Container die's

==============================================================================================================================================================


# Setup Namespace

Namespaces are the default way for kubernetes to separate resources.
    Namespaces do not share anything between them, which is important to know,
    and thus come in handy when you have multiple users on the same cluster,
    that you don't want stepping on each other's toes :)

## 1.1 Create a namespace

Choose a name for your namespace, something unique so you don't clash with one of the other participants at the workshop.

```shell
$ kubectl create namespace my-namespace
namespace "my-namespace" created
```

## 1.2 Scoping the kubectl command

You want to target your own namespace instead of default one every time you use `kubectl`.
    You can run a command in a specific namespace by using the `-n, --namespace=''`-flag.

The below commands do the same thing, because kubernetes commands will default to the default namespace:

```shell
kubectl get pods -n default
kubectl get pods
```

## 1.3 Set your default namespace

It gets tedious however to write this every time you want to select your own namespace,
    so it makes sense to set this as the default.

To get the which name space is set to default run below command.

The namespace which is having * is presently set to default.

```shell
PS C:\WINDOWS\system32> kubectl config get-contexts
CURRENT   NAME                 CLUSTER          AUTHINFO         NAMESPACE
          docker-desktop       docker-desktop   docker-desktop
          docker-for-desktop   docker-desktop   docker-desktop
*         minikube             minikube         minikube

PS C:\WINDOWS\system32> kubectl config current-context
minikube

```

To overwrite the default namespace for your current `context`, run:

```shell
$ kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
Context "<your current context>" modified.
```

Or perform the same step with two individual commands:
```

```
PS C:\Users\Samu\.kube> kubectl config use-context docker-desktop
Switched to context "docker-desktop".
PS C:\Users\Samu\.kube> kubectl config get-contexts
CURRENT   NAME                 CLUSTER          AUTHINFO         NAMESPACE
*         docker-desktop       docker-desktop   docker-desktop
          docker-for-desktop   docker-desktop   docker-desktop
          minikube             minikube         minikube

```


PS C:\WINDOWS\system32> kubectl config current-context
minikube
PS C:\WINDOWS\system32> kubectl  config set-context minikube --namespace=nadeem
Context "minikube" modified.
PS C:\WINDOWS\system32> kubectl config get-contexts
CURRENT   NAME                 CLUSTER          AUTHINFO         NAMESPACE
          docker-desktop       docker-desktop   docker-desktop
          docker-for-desktop   docker-desktop   docker-desktop
*         minikube             minikube         minikube         nadeem
```


You can verify that you've updated your current `context` by running:

```shell
kubectl config get-contexts
```

```
[student-1@kamran-jumpbox ~]$ kubectl config get-contexts
CURRENT   NAME                                                            CLUSTER                                                         AUTHINFO                                                        NAMESPACE
*         gke_praqma-education_europe-north1-a_kamran-test-cluster-0617   gke_praqma-education_europe-north1-a_kamran-test-cluster-0617   gke_praqma-education_europe-north1-a_kamran-test-cluster-0617   student-1
[student-1@kamran-jumpbox ~]$ 
```

Notice that the namespace column has the value of `<my-namespace>`.

Most errors you will get throughout the rest of the workshop will 99% be due to deploying into a namespace,
   
### 1.4 Change the Default Cluster or Move to different Cluster

```
PS C:\Users\Samu\.kube> kubectl config use-context docker-desktop
Switched to context "docker-desktop".
PS C:\Users\Samu\.kube> kubectl config get-contexts
CURRENT   NAME                 CLUSTER          AUTHINFO         NAMESPACE
*         docker-desktop       docker-desktop   docker-desktop
          docker-for-desktop   docker-desktop   docker-desktop
          minikube             minikube         minikube

```

## 1.5 More on Namespaces

Namespaces are quite powerful. On a user level, it is also possible to limit namespaces and resources by users .
    Therefore, please be aware that other people's namespaces are off limits for this workshop; even if you do have access ;)

Kubernetes clusters come with a namespace called `default`, 

    and usually one called `kube-system` which will contain some of the kubernetes services running in the cluster.

You might see later that the namespace is specified directly in the yaml files describing the resources.
    This makes it possible to have the resource created in the specific namespace without specifying the `-n` flag on creation.

