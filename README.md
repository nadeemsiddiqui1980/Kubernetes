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

# Config File

Config file contains all the configuration details of our kubernetes cluster and it is stored inside the Kube Folder.

```
PS C:\Users\Samu\.kube> cat .\config
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNU1USXpNREUyTXpBMU1sb1hEVEk1TVRJeU56RTJNekExTWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTUJJClpxMXJnT2w0REtiOUZ5dFRnc09OMUxSZXo3Wm92V3RDcGplUzNIUFdkTGtlRHVINzFxRVU5ZG9lUlA4YnBmTHoKckh6UVFRd296cmNjTmJTVjBIeHdYcHFPNUljM3ZUU1FZSjdrNnppdWNBZVkvQ2k3NHkzZ0w0UVdLQUFEN2RHKwp3Z1d0TFg2eUo3eDJiK1BJaUxLODdtSmhrQmFFZFdyNU5FQUtqNXROYmZyRWc0MEZudEs1U1lwWmRQT0JaT2pqCnBvSzdKRk0rcWVpY3B2YUVWTjFwamlQakhFMC8zZ2xmMG8wanJ3U1JtaVJ2dDgwL3BIdFV3VEYvd1RkVXlucWoKT3cvcmw2OXBBZXVSRWl3RWRDUFk2NENxTmVoSGFGMVlMVEc5S0xlWU43clJmdnpWU0JkaFc4Nks3MEcwZ09DZApJZ2xpZ3lGRHlONmNMcitBZ3dNQ0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFIRDZzeGNtVnJabXZrdUFLL1VudFY1MmNNT0cKYWY3RUhtRjAvSktDb29UVlh6VmFrb1hRK0VwYmxXcWlZWHdQRUhQdWtqdkFDZm5VZFBvcWJsaitCWTVOZmttRApxYkhZT0VXVm1pOGZjQ1RBb216YlhpYTdab1RLbzltQWlVK2Z4OEFneFZQMEYwUlkxbkg1ZklRUkViL0txQnd6CjRvYTRGR2c2d1ZCU0swSFZvV2l3cFpDM05OQlFZcDhranFYbUFoTmtOMW5kRFVwb3dEZUtLMDMzT2d0eXBPUCsKOU9FUHIxMzBGVHVPbXJzL2dJOVN4NEtvTHNvS0c2KzdxYTRYQ3huUklqdjk5dGoxVDBsdGljQjBELzJOQVEySQo5TDB3RnAzTmZad3JmSjk1Z1BsZk1taG5vUkluQnJaQ1cwb2JLRHlOS2RxbHRUVE1tblVHR00rZnpzaz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
- cluster:
    certificate-authority: C:\Users\Samu\.minikube\ca.crt
    server: https://172.18.57.156:8443
  name: minikube
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-for-desktop
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM5RENDQWR5Z0F3SUJBZ0lJY21XZm9aaFdoUW93RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB4T1RFeU16QXhOak13TlRKYUZ3MHlNREV5TWpreE5qTTBOREZhTURZeApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sc3dHUVlEVlFRREV4SmtiMk5yWlhJdFptOXlMV1JsCmMydDBiM0F3Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLQW9JQkFRRFk1R1FpRm9QRnRzNVkKbHZqZE9yZndGNFBLR0FTWS9wbkZLRDZBd2UyU09wT3BOMmxjTDdmTVFrUzA3U1ROcXFwTVJNU1Jsckw3ZnhTZQpBYUMxVnhPM0J2ajJQQUcvcUpIMmtDT21ycGpZOW1NbUliTEdWOEI1RzU4b3hhSmtFaGJJai9TVnF2VWpvNzRUClIxSGoyMnVQTzE2U2NPL0VKNkJNc3hhZy9md0tkQ1hpcmVUWTJHL0ZqdkltRVovOFhuVk9uYmZhQVBXTm5CR04KUmxiMzczcE4zUTJ5cCt1MUNsNzh2QkhBSnYyQWljdjViZm5LMFYvSFBTUjFTVkhwWksrRkJpcGg1NWtlcTg2cwpiVTgwck1DVnJublQ5M2NkUEpobCs2WGlxZC9WWU0zb29HN09zbm9jZWxZTUtSRWlldzZTVnI2RnA0Znd3M1JhCkZJcS9EUEVoQWdNQkFBR2pKekFsTUE0R0ExVWREd0VCL3dRRUF3SUZvREFUQmdOVkhTVUVEREFLQmdnckJnRUYKQlFjREFqQU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FRRUFMeGw0dHgyY0ltVUFWcG00UHhzUVQ4YzkvUm1FQTNtMQpOcWRsMDFzTVppZkczMjFGalRmWk8wTmw5RVVwWUl5TEltaEpRaWtieVpWZmR5ZFRlMkZyRk9PUkpxNVdoSzdKCjN3SGw5Um9SZWdjRTk1RllPWWFESzc4V3FMWXFtQWI0Rjg4L0dKZUFCUkVOaElFUnlualRuMmtIbm5Tbmsxd2UKSE5HUGZCcFJOZXNuNk81WnlYUHBCVkxKbGlnSmVvdGhFckVLUlVPbzFkMlBFVjkrczdDdzRDbHgvNmVxVlF3eApCbkZSWlBhMDB6WXB3VnF4eDROeVRMemNRZkN4NHgybDN2VWp4OFIzalQvRTNLRitnWmJCUG1BajJLNHhTM3JsCkd4MnpER0FjNmI2aGpUZzZ2czd4OTl0WkxiOVRzd2tZMS95N3ZTUUlSSCtHQnRmcDdGWm1iQT09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    client-key-data: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb2dJQkFBS0NBUUVBMk9Sa0loYUR4YmJPV0piNDNUcTM4QmVEeWhnRW1QNlp4U2crZ01IdGtqcVRxVGRwClhDKzN6RUpFdE8wa3phcXFURVRFa1pheSszOFVuZ0dndFZjVHR3YjQ5andCdjZpUjlwQWpwcTZZMlBaakppR3kKeGxmQWVSdWZLTVdpWkJJV3lJLzBsYXIxSTZPK0UwZFI0OXRyanp0ZWtuRHZ4Q2VnVExNV29QMzhDblFsNHEzawoyTmh2eFk3eUpoR2YvRjUxVHAyMzJnRDFqWndSalVaVzkrOTZUZDBOc3FmcnRRcGUvTHdSd0NiOWdJbkwrVzM1Cnl0RmZ4ejBrZFVsUjZXU3ZoUVlxWWVlWkhxdk9yRzFQTkt6QWxhNTUwL2QzSFR5WVpmdWw0cW5mMVdETjZLQnUKenJKNkhIcFdEQ2tSSW5zT2tsYStoYWVIOE1OMFdoU0t2d3p4SVFJREFRQUJBb0lCQUVscXhocGoxS2NRZ2ppcQpvZ01BNVZKNEl6dzlkUkQwM2NoSEh5RW1nK3lEdDRnSUlibjZ6UlJ2T2lLa1ExajY5RHBzN0x6N2JncURzYzdxCmJpUDBIZEJPbytkMTJJR3Y1Zmk0UWRraU1Nc0FXLytFV0tlYS9LUUNIWllIa0RpZmh1Yk5FOVcxME5VSGtFZW0KVktuMGxDd2Z4SnQ5Ynl2TzNnd1ljd2g3OE56NFpIUHVYS3VQUkxHVGtLK2ZCbnN1d2labTkrWllJTWMvNVlSbApZRUlBNVlJTk9EZlBBa2RWRFFKV08wZmUwbjJ2L284RUVjSk9XOFZ2cURYYUFYVmp1b2FVSWwrQXRYL2k4dWZHCnlYRDZIR3o0Y0gzcmxkL0IvREtRQm9BUW5TZjhOQ0ZDaGlTNElBTkIwN3JxSlNwQzFQL2dneHhJQncyeHNtc0MKZnJENkxLRUNnWUVBNjUyeHR3SHQzZWduczhwLzI5N2hSRGwwNXMvRHpDUWQyUlRTU29wUElCSVF2WnM1WXJMagplZTNXM01qRnMrSWR0Y1AwWEtWMmUxR2h1Yi9SSkRlWVk3WDBtUzlpblFhOVIyWnhxRGFOSHdwZkZIMG41cW9FCnBjbmd5aEFySGFrNjlkZWtMS3FnTmREVjJ3NjhkeGg3QlhxOFMxSndtRmFBaHJaU2dtVGtraThDZ1lFQTY2Z0MKdWd5eFZFZEtPcmh5QjhKbStVTTJyeGtDZFFkQ0VPaVdWbUg3RzRQZUdxeFoxT25oUEgyN21UOFdZOGtMZU9pNgpVZ1JsV0x5K3pWV0dUNnpBd2R1anNtUGYzR3ZZSjdFbFJpY3FGOUN0ZjN2MzNTRGZMd2F2TStTeE1sVTJ5SDVtCkVBVkJ3YUFYekZ4Q21JTFljSFUrU0Fnb1owWW1maWdXSFl1aGJhOENnWUFQN2g2STRCR0VFbUUwejdrclZYdG4Ka0hidDhCZ3Q0amMrYVNENnR6VTRWdUJZNFhqVXlvR0V5ZWJnRUpjRlhZRml1N2YyMTUwV0kyUEsya1E2cmFPWgpBa0ZpWmdqRjB5SFRCUU1rTzJQNU9FdExhRmJkU3B0NzFoVmp0QW9tUEQzblIwZ3JXUEh5RVllVUF3QU5FVk9vCkFDOWc3RmIraGNLMDJQamxKZ3NxTXdLQmdHNXh1STF2dzNCSFZSKytNQnM0M2ovMlkxdWU4Z3JkRXZhUHUxM1MKMy9nZVRtcmIyZUl5bHRCZDhSMDZkd2pmUVpReUpwaW4zTVBBK2YrTUZMMmtybFpzMVFTWFVHU2kycFNIcm50NQpnWDNWM0dxQ05FR2IxVjNaMlNVT0Nvb1hhK3g5YU9JYlJKMDFwZEd1Yjd2QW55WGRuUW52WU5nK0JXNWM1VGlGCnAydWJBb0dBSURTRlpydVNnSVhhS3A0Nm80SVBtU0V3Z3NGaFBGT0NHR3dnK0tlV3dhY0wreFd5c3ZVZzM3Vm8KbllCUTBSUzVVZGo3Qm5LNHhBN0d5bURzd1ROaGVQMzMrUk1HMHc4T2p2MVRsUldRYjlUY2NIM0U3MmpOR3ZJMQpNYUlyMU44YmZRUXhRODVackJMT3dTZkNweVk3U0VzUHlacmFiZElyaTRnWmVzRENZSnM9Ci0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==
- name: minikube
  user:
    client-certificate: C:\Users\Samu\.minikube\client.crt
    client-key: C:\Users\Samu\.minikube\client.key
PS C:\Users\Samu\.kube> kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
- cluster:
    certificate-authority: C:\Users\Samu\.minikube\ca.crt
    server: https://172.18.57.156:8443
  name: minikube
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-for-desktop
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: minikube
  user:
    client-certificate: C:\Users\Samu\.minikube\client.crt
    client-key: C:\Users\Samu\.minikube\client.key
PS C:\Users\Samu\.kube> kubectl config view --minify=true
apiVersion: v1
clusters:
- cluster:
    certificate-authority: C:\Users\Samu\.minikube\ca.crt
    server: https://172.18.57.156:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: C:\Users\Samu\.minikube\client.crt
    client-key: C:\Users\Samu\.minikube\client.key

```
==============================================================================================================================================================

# Namespaces:
Namespaces are the default way for kubernetes to separate resources. Namespaces do not share anything between them, which is important to know. This is quite powerful concept, but not unusual, as in computing - we are used to having isolated environments such as home directories, jailed environments, etc. Kubernetes clusters come with a namespace called **default**.

When you execute a kubectl command without specifying a namespace, it is run in/against the namespace named **default**! So far all the commands you have executed in the previous exercise, have been executed in the *default* namespace. You can optionally use the namespace flag (-n <namespace>) to execute the command a specific namespace. When you are creating Kubernetes objects though *yaml* files, you can specify a namespace for a particular resource. 


It is also possible to limit namespaces and resources at user level

## Create a namespace

Use the following syntax to create a namespace and to list objects from a namespace:
```
kubectl create namespace <name>

kubectl get pods -n <name>
```

Here are few examples:
```
$ kubectl create namespace dev
namespace "dev" created

$ kubectl create namespace test
namespace "test" created

$ kubectl create namespace prod
namespace "prod" created
```

```
$ kubectl get namespaces
NAME          STATUS    AGE
default       Active    2d
dev           Active    12s
prod          Active    5s
test          Active    8s
```


The below commands do the same thing, because kubernetes commands will default to the *default* namespace:

```
kubectl get pods -n default
kubectl get pods
```

## Manage Kubernetes objects in a namespace:

So let's try to create an nginx deployment in the *dev* namespace.

```
$ kubectl --namespace=dev run nginx --image=nginx
deployment "nginx" created

$ kubectl get pods -n dev
NAME                     READY     STATUS    RESTARTS   AGE
nginx-4217019353-fk9ph   1/1       Running   0          10s
```

So, you get the idea. You include `-n <namespacename>` in any kubectl invokation, and it will operate on the given namespace only. You can also use `--all-namespaces=true` to get objects from all namespaces.

The yaml files for the coming exercises will give you a better impression of what is sensible.



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
    
    
    
==============================================================================================================================================================



We will cover below topisc in this section:

* Pod creation / deletion
* Deployments
* Logging into the pods/containers
* Viewing logs of the pods/containers

# Pods and Deployments:

A **Pod** (*not container*) is the basic building-block/worker-unit in Kubernetes. *Normally* a pod is a part of a **Deployment**. 

## Creating pods using 'run' command:
We start by creating our first deployment. Normally people will run an nginx container/pod as first example o deployment. You can surely do that. But, we will run a different container image.
The reason is that it will work as a multitool for testing and debugging throughout this course. Besides it too runs nginx! 


Here is the command to do it:

```
kubectl run multitool  --image=praqma/network-multitool
```

You should be able to see the following output:

```
$ kubectl run multitool --image=praqma/network-multitool 
deployment "multitool" created
```

What this command does behind the scenes is, it creates a deployment named multitool, starts a pod using this docker image (praqma/network-multitool), and makes that pod a memeber of that deployment. You don't need to confuse yourself with all these details at this stage. This is just extra (but vital) information. Just so you know what we are talking about, check the list of pods and deployments:

List of pods:
```
$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
multitool-3148954972-k8q06   1/1       Running   0          3m
```

List of deployments:
```
$ kubectl get deployments
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
multitool   1         1         1            1           3m
```

There is actually also a replicaset, which is created as a result of the `run` command above, but we did not mention earlier; because that is not super important to know at this point. It is something which deals with the number of copies of this pod.  It is shown below just for the sake of completeness.
```
$ kubectl get replicasets
NAME                   DESIRED   CURRENT   READY     AGE
multitool-3148954972   1         1         1         3m
```

Ok. The bottom line is that we wanted to have a pod running ,and we have that. 


Lets setup another pod, a traditional nginx deployment, with a specific version - 1.7.9. 


Setup an nginx deployment with nginx:1.7.9
```
kubectl run nginx  --image=nginx:1.7.9
```

You get another deployment and a replicaset as a result of above command, shown below, so you know what to expect:

```
$ kubectl get pods,deployments,replicasets
NAME                            READY     STATUS    RESTARTS   AGE
po/multitool-3148954972-k8q06   1/1       Running   0          25m
po/nginx-1480123054-xn5p8       1/1       Running   0          14s

NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/multitool   1         1         1            1           25m
deploy/nginx       1         1         1            1           14s

NAME                      DESIRED   CURRENT   READY     AGE
rs/multitool-3148954972   1         1         1         25m
rs/nginx-1480123054       1         1         1         14s
```


## Alternate / preffered way to deploy pods:
You can also use the `nginx-simple-deployment.yaml` file to create the same nginx deployment. The file is in suport-files directory of this repo. However before you execute the command shown below, note that it will try to create a deployment with the name **nginx**. If you already have a deployment named **nginx** running, as done in the previous step, then you will need to delete that first.

Delete the existing deployment using the following command:
```
$ kubectl get deployments
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
multitool   1         1         1            1           32m
nginx       1         1         1            1           7m

$ kubectl delete deployment nginx
deployment "nginx" deleted
```

Now you are ready to proceed with the example below:

```
$ kubectl create -f nginx-simple-deployment.yaml 
deployment "nginx" created
```


The contents of `nginx-simple-deployment.yaml` are as follows:
```
$ cat nginx-simple-deployment.yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```


Verify that the deployment is created:
```
$ kubectl get deployments
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
multitool   1         1         1            1           59m
nginx       1         1         1            1           36s
```


Check if the pods are running:
```
$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
multitool-3148954972-k8q06   1/1       Running   0          1h
nginx-431080787-9r0lx        1/1       Running   0          40s
```

## Deleting a pod, the kubernetes promise of resilience:

Before we move forward, lets see if we can delete a pod, and if it comes to life automatically:
```
$ kubectl delete pod nginx-431080787-9r0lx 
pod "nginx-431080787-9r0lx" deleted
```

As soon as we delete a pod, a new one is created, satisfying the desired state by the deployment, which is - it needs at least one pod running nginx. So we see that a **new** nginx pod is created (with a new ID):
```
$ kubectl get pods
NAME                         READY     STATUS              RESTARTS   AGE
multitool-3148954972-k8q06   1/1       Running             0          1h
nginx-431080787-tx5m7        0/1       ContainerCreating   0          5s

(after few more seconds)

$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
multitool-3148954972-k8q06   1/1       Running   0          1h
nginx-431080787-tx5m7        1/1       Running   0          12s
```

## Creating a standalone pod:
Often times you will need to create a pod, without making it a member of a deployment or daemon-set, or anything else. For those instances, here is how you would create a standalone pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: standalone-nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:alpine
```

Save the above few lines of code as a yaml file, and use `kubectl create -f <filename>` to create this pod.

```
$ kubectl create -f support-files/standalone-nginx-pod.yaml 
pod "standalone-nginx-pod" created

$ kubectl get pods
NAME                   READY     STATUS    RESTARTS   AGE
standalone-nginx-pod   1/1       Running   0          4s
$
```

The example above will work with container images, which have some sort of daemon/service process running as their entrypoint. If you want to run something which does not have a **service process** in the container image, you can pass it a custom command, such as shown below: 

```
apiVersion: v1
kind: Pod
metadata:
  name: standalone-busybox-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']

```
The above code will create a pod, which will go into a sleep for 3600 seconds ( one hour), and will exit (die) dilently. Good to run certain diagnostics.

```
$ kubectl create -f support-files/standalone-busybox-pod.yaml 
pod "standalone-busybox-pod" created
$

$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
standalone-busybox-pod   1/1       Running   0          30s
$
```



```
$ kubectl run multitool --image praqma/network-multitool
```


## Exec into the pod/container:

Just like `docker exec`, you can `exec` into a kubernetes pod/container by using `kubectl exec`. This is a good way to troubleshoot any problems. All you need is the name of the pod (and container name - in case it is a multi-container pod). 

You can `exec` into the pod like so:

```
[C:\Users\Samu\.kube]$ kubectl exec -it standalone-busybox-pod /bin/sh

/ # 
```

You can do a lot of troubleshooting after you exec (log) into the pod:
```
[C:\Users\Samu\.kube]$ kubectl exec -it standalone-busybox-pod /bin/sh

/ # ls -l
total 16
drwxr-xr-x    2 root     root         12288 Feb 14 18:58 bin
drwxr-xr-x    5 root     root           360 Mar  8 12:51 dev
drwxr-xr-x    1 root     root            66 Mar  8 12:51 etc
drwxr-xr-x    2 nobody   nogroup          6 Feb 14 18:58 home
dr-xr-xr-x  127 root     root             0 Mar  8 12:51 proc
drwx------    1 root     root            26 Mar  8 12:55 root
dr-xr-xr-x   13 root     root             0 Mar  7 10:49 sys
drwxrwxrwt    2 root     root             6 Feb 14 18:58 tmp
drwxr-xr-x    3 root     root            18 Feb 14 18:58 usr
drwxr-xr-x    1 root     root            17 Mar  8 12:51 var

/ # nslookup yahoo.com
Server:		10.32.0.10
Address:	10.32.0.10:53

Non-authoritative answer:
Name:	yahoo.com
Address: 2001:4998:c:1023::4
Name:	yahoo.com
Address: 2001:4998:44:41d::4
/ # exit
$
```

An example of network-multitool: 
```
$ kubectl run multitool --image=praqma/network-multitool 
deployment.apps "multitool" created

$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
multitool-5558fd48d4-lggqg   1/1       Running   0          12s
standalone-busybox-pod       1/1       Running   0          13m
standalone-nginx-pod         1/1       Running   0          22m
$
```

```
$ kubectl exec -it multitool-5558fd48d4-lggqg /bin/bash
bash-4.4# dig +short yahoo.com
98.137.246.8
98.137.246.7
72.30.35.10
98.138.219.231
72.30.35.9
98.138.219.232
bash-4.4# 


bash-4.4# dig +short kubernetes.default.svc.cluster.local
10.32.0.1

bash-4.4# 
```


## Logs:
Logs can be very helpful in troubleshooting why a certain pod/container is not behaving the way you expect it to. You can check logs of the pods by using `kubectl logs [-f] <pod-name>`. E.g. to watch the logs of the nginx pod, you can do the following:

```
$ kubectl logs standalone-nginx-pod
10.200.2.4 - - [31/Dec/2019:13:15:18 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.61.1" "-"
$
```
The above example shows, that the nginx web service in this pod was accessed by a client with an IP `10.200.2.4` . 



## Accessing the pods:

Now the question comes, How can we access nginx webserver at port 80 in this pod? For now we can do it from within the cluster. First, we need to know the IP address of the nginx pod. We use the `-o wide` parameters with the `get pods` command:

```
$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP             NODE
multitool-3148954972-k8q06   1/1       Running   0          1h        100.96.2.31    ip-172-20-60-255.eu-central-1.compute.internal
nginx-431080787-tx5m7        1/1       Running   0          12m       100.96.1.148   ip-172-20-49-54.eu-central-1.compute.internal
```

**Bonus info:** The IPs you see for the pods (e.g. 100.96.2.31) are private IPs and belong to something called *Pod Network*, which is a completely private network inside a Kubernetes cluster, and is not accessible from outside the Kubernetes cluster.

Now, we `exec` into our multitool, as shown below and use the `curl` command from the pod to access nginx service in the nginx pod:

```
$ kubectl exec -it multitool-3148954972-k8q06 bash
[root@multitool-3148954972-k8q06 /]#
```

```
[root@multitool-3148954972-k8q06 /]# curl -s 100.96.1.148 | grep h1
<h1>Welcome to nginx!</h1>
[root@multitool-3148954972-k8q06 /]# 
```

We accessed the nginx webserver in the nginx pod using another (multitool) pod in the cluster, because at this point in time the nginx web-service (running as pod) is not accessible through a *service*. Services are explained separately.


This concludes the exercise, happy coding!

# To check the component status use below command

```
PS C:\Users\Samu\.kube> kubectl get cs
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok
controller-manager   Healthy   ok
etcd-0               Healthy   {"health":"true"}

PS C:\WINDOWS\system32> kubectl get componentstatuses
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}

```



# To get detail information about the cluster or namespace 

```
PS C:\Users\Samu\.kube> kubectl describe node minikube
Name:               minikube
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=minikube
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 30 Dec 2019 23:33:46 +0530
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 31 Dec 2019 12:09:24 +0530   Mon, 30 Dec 2019 23:33:43 +0530   KubeletHasSufficientMemory   kubelet has suffici
ent memory available
  DiskPressure     False   Tue, 31 Dec 2019 12:09:24 +0530   Mon, 30 Dec 2019 23:33:43 +0530   KubeletHasNoDiskPressure     kubelet has no disk
 pressure
  PIDPressure      False   Tue, 31 Dec 2019 12:09:24 +0530   Mon, 30 Dec 2019 23:33:43 +0530   KubeletHasSufficientPID      kubelet has suffici
ent PID available
  Ready            True    Tue, 31 Dec 2019 12:09:24 +0530   Tue, 31 Dec 2019 08:58:42 +0530   KubeletReady                 kubelet is posting
ready status
Addresses:
  InternalIP:  172.18.57.156
  Hostname:    minikube
Capacity:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1986524Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  17784772Ki
 hugepages-2Mi:      0
 memory:             1986524Ki
 pods:               110
System Info:
 Machine ID:                 a1784f50ca124b80bebc2c63df4032d9
 System UUID:                fc037f35-b96a-8f46-b2d2-95a554021253
 Boot ID:                    a8125901-ec65-4007-ba13-7ee48d95cad3
 Kernel Version:             4.19.81
 OS Image:                   Buildroot 2019.02.7
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://19.3.5
 Kubelet Version:            v1.17.0
 Kube-Proxy Version:         v1.17.0
Non-terminated Pods:         (11 in total)
  Namespace                  Name                                          CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                          ------------  ----------  ---------------  -------------  ---
  kube-system                coredns-6955765f44-qrbsp                      100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     12h
  kube-system                coredns-6955765f44-wjvq8                      100m (5%)     0 (0%)      70Mi (3%)        170Mi (8%)     12h
  kube-system                etcd-minikube                                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         12h
  kube-system                kube-addon-manager-minikube                   5m (0%)       0 (0%)      50Mi (2%)        0 (0%)         12h
  kube-system                kube-apiserver-minikube                       250m (12%)    0 (0%)      0 (0%)           0 (0%)         12h
  kube-system                kube-controller-manager-minikube              200m (10%)    0 (0%)      0 (0%)           0 (0%)         12h
  kube-system                kube-proxy-rnqlr                              0 (0%)        0 (0%)      0 (0%)           0 (0%)         12h
  kube-system                kube-scheduler-minikube                       100m (5%)     0 (0%)      0 (0%)           0 (0%)         12h
  kube-system                storage-provisioner                           0 (0%)        0 (0%)      0 (0%)           0 (0%)         12h
  kubernetes-dashboard       dashboard-metrics-scraper-7b64584c5c-96725    0 (0%)        0 (0%)      0 (0%)           0 (0%)         4h38m
  kubernetes-dashboard       kubernetes-dashboard-79d9cd965-m2mtf          0 (0%)        0 (0%)      0 (0%)           0 (0%)         4h38m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                755m (37%)  0 (0%)
  memory             190Mi (9%)  340Mi (17%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:              <none>

```

------------------

## Useful commands

    kubectl config get-contexts
    kubectl config use-context minikube
    kubectl version
    kubectl cluster-info
    kubectl get nodes
    kubectl get pods
    kubectl get all
    kubectl describe pod
    kubectl get events --sort-by=.metadata.creationTimestamp
    kubectl api-resources # kubectl 1.11+
    kubectl api-versions


Cheatsheat: [https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

==============================================================================================================================================================

# Service Discovery and Loadbalancing

In almost every Kubernetes cluster, there is an addon called CoreDNS (previously KubeDNS), which provides service discovery within the cluster, using DNS mechanism. Everytime a *service* is created in kubernetes cluster, it is registered in CoreDNS with the name of the service, it's ClusterIP. e.g. `nginx.default.svc.cluster.local` . 

  Each service will have a name, a clusterIP, and also the list of backends linked with this service. This *service* also acts as an internal load balancer, when the service has more than one endpoints. e.g. A nginx delployment can have four replicas. When exposed as a service, the service will have four endpoints. When this service is accessed by a client (a pod or any other process), the service does load balancing between these endpoints. 


## Accessing a service:
To access the actual process/service inside any given pod (e.g. nginx web service), we need to *expose* the related deployment as a kubernetes *service*. We have three main ways of exposing the deployment , or in other words, we have three ways to define a *service*. We can access these three types of services in three different ways. The three types of services are:

* ClusterIP
* NodePort
* LoadBalancer

### Service type: ClusterIP
Expose the deployment as a service - type=ClusterIP:
```
$ kubectl expose deployment nginx --port 80 --type ClusterIP
service "nginx" exposed
```

Check the list of services:
```
$ kubectl get services
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   100.64.0.1       <none>        443/TCP   2d
nginx        ClusterIP   100.70.204.237   <none>        80/TCP    4s
```

Notice, there are two services listed here. The first one is named **kubernetes**, which is the default service created (automatically) when a kubernetes cluster is created for the first time. It does not have any EXTERNAL-IP. This service is not our focus right now.

The service in focus is nginx, which does not have any external IP, nor does it say anything about any other ports except 80/TCP. This means it is not accessible over internet, but we can still access it from within cluster , using the service IP, not the pod IP. Lets see if we can access this service from our multitool.

```
[root@multitool-3148954972-k8q06 /]# curl -s 100.70.204.237 | grep h1
<h1>Welcome to nginx!</h1>
[root@multitool-3148954972-k8q06 /]# 
```

It worked! 


You can also access the same service using it's DNS name:

```
[root@multitool-3148954972-k8q06 /]# curl -s nginx.default.svc.cluster.local  | grep h1
<h1>Welcome to nginx!</h1>
[root@multitool-3148954972-k8q06 /]# 
```

You can also use the `describe` command to describe any Kubernetes object in more detail. e.g. we use `describe` to see more details about our nginx service:

```
$ kubectl describe service nginx
Name:              nginx
Namespace:         default
Labels:            app=nginx
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP:                100.70.204.237
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         100.96.1.148:80
Session Affinity:  None
Events:            <none>
```

You can of-course use `... describe pod ...` , ` ... describe deployment ...` , etc.


**Additional notes about the Cluster-IP:**
* The IPs assigned to services as Cluster-IP are from a different Kubernetes network called *Service Network*, which is a completely different network altogether. i.e. it is not connected (nor related) to pod-network or the infrastructure network. Technically it is actually not a real network per-se; it is a labelling system, which is used by Kube-proxy on each node to setup correct iptables rules. (This is an advanced topic, and not our focus right now).
* No matter what type of service you choose while *exposing* your deployment, Cluster-IP is always assigned to that particular service.
* Every service has end-points, which point to the actual pods service as a backend of a particular service.
* As soon as a service is created, and is assigned a Cluster-IP, an entry is made in Kubernetes' internal DNS against that service, with this service name and the Cluster-IP. e.g. `nginx.default.svc.cluster.local` would point to `100.70.204.237` . 


### Service type: NodePort

Our nginx service is still not reachable from outside, so now we re-create this service as NodePort.

```
$ kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   100.64.0.1       <none>        443/TCP   17h
nginx        ClusterIP   100.70.204.237   <none>        80/TCP    15m
```

```
$ kubectl delete svc nginx
service "nginx" deleted
```

```
$ kubectl expose deployment nginx --port 80 --type NodePort
service "nginx" exposed
```

```
$ kubectl get svc
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   100.64.0.1     <none>        443/TCP        17h
nginx        NodePort    100.65.29.172  <none>        80:32593/TCP   8s
```

Notice that we still don't have an external IP, but we now have an extra port `32593` for this pod. This port is a **NodePort** exposed on the worker nodes. So now, if we know the IP of our nodes, we can access this nginx service from the internet. First, we find the public IP of the nodes:
```
$ kubectl get nodes -o wide
NAME                                            STATUS    ROLES     AGE       VERSION        EXTERNAL-IP     OS-IMAGE                             KERNEL-VERSION   CONTAINER-RUNTIME
gke-dcn-cluster-35-default-pool-dacbcf6d-3918   Ready     <none>    17h       v1.8.8-gke.0   35.205.22.139   Container-Optimized OS from Google   4.4.111+         docker://17.3.2
gke-dcn-cluster-35-default-pool-dacbcf6d-c87z   Ready     <none>    17h       v1.8.8-gke.0   35.187.90.36    Container-Optimized OS from Google   4.4.111+         docker://17.3.2
```

Even though we have only one pod (and two worker nodes), we can access any of the node with this port, and it will eventually be routed to our pod. So, lets try to access it from our local work computer:

```
$ curl -s 35.205.22.139:32593 | grep h1
<h1>Welcome to nginx!</h1>
```

It works!

### Service type: LoadBalancer
So far so good; but, we do not expect the users to know the IP addresses of our worker nodes. It is not a flexible way of doing things. So we re-create the service as `type=LoadBalancer`. The type LoadBalancer is only available for use, if your k8s cluster is setup in any of the public cloud providers, GCE, AWS, etc.

```
PS C:\Users\Samu\.kube> kubectl delete svc nginx
service "nginx" deleted
```

```
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
service "nginx" exposed
```

```
$ kubectl get svc
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      100.64.0.1     <none>        443/TCP        17h
nginx        LoadBalancer   100.69.15.89   <pending>     80:31354/TCP   5s
```

In few minutes of time the external IP will have some value instead of the word 'pending' . 
```
$ kubectl get svc
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP      100.64.0.1     <none>        443/TCP        17h
nginx        LoadBalancer   100.69.15.89   35.205.60.29  80:31354/TCP   5s

```

Now, we can access this service without using any special port numbers:
```
PS C:\Users\Samu\.kube> curl -s 35.205.60.29 | grep h1
<h1>Welcome to nginx!</h1>
PS C:\Users\Samu\.kube>
```

**Additional notes about LoadBalancer:**
* A service defined as LoadBalancer will still have some high-range port number assigned to it's main service port, just like NodePort. This has a clever purpose, but is an advance topic and is not our focus at this point.


## High Availability / Load balancing

To prove that multiple pods of the same deployment provide high availability, we do a small exercise. To visualize it, we need to run a small web server which could return us some uniqe content when we access it. We will use our trusted multitool for it. Lets run it as a separate deployment and access it from our local computer.

```shell
$ kubectl create deployment customnginx --image=praqma/network-multitool
deployment.apps/customnginx created
$ kubectl scale deployment customnginx --replicas=4
deployment.extensions/customnginx scaled
```

```shell
$ kubectl get pods
NAME                           READY     STATUS    RESTARTS   AGE
customnginx-3557040084-1z489   1/1       Running   0          49s
customnginx-3557040084-3hhlt   1/1       Running   0          49s
customnginx-3557040084-c6skw   1/1       Running   0          49s
customnginx-3557040084-fw1t3   1/1       Running   0          49s
multitool-5f9bdcb789-k7f4q     1/1       Running   0          19m
```

Lets create a service for this deployment as a type=LoadBalancer:

```shell
$ kubectl expose deployment customnginx --port=80 --type=LoadBalancer
service/customnginx exposed
```

Verify the service and note the public IP address:

```shell
$ kubectl get services
NAME          TYPE           CLUSTER-IP    EXTERNAL-IP        PORT(S)        AGE
customnginx   LoadBalancer   100.67.40.4   35.205.60.41       80:30087/TCP   1m
kubernetes    ClusterIP      100.64.0.1    <none>             443/TCP        17h
```

Query the service, so we know it works as expected:

```shell
$ curl -s 35.205.60.41 | grep IP
Container IP: 100.96.1.150 <BR></p>
```

Next, setup a small bash loop on your local computer to curl this IP address, and get it's IP address.

```shell
$ while true; do sleep 1; curl -s 35.205.60.41; done
Praqma Network MultiTool (with NGINX) - customnginx-7fcfd947cf-zbvtd - 100.96.2.36 <BR></p>
Praqma Network MultiTool (with NGINX) - customnginx-7fcfd947cf-zbvtd - 100.96.1.150 <BR></p>
Praqma Network MultiTool (with NGINX) - customnginx-7fcfd947cf-zbvtd - 100.96.2.37 <BR></p>
Praqma Network MultiTool (with NGINX) - customnginx-7fcfd947cf-zbvtd - 100.96.2.37 <BR></p>
Praqma Network MultiTool (with NGINX) - customnginx-7fcfd947cf-zbvtd - 100.96.2.36 <BR></p>
^C
```

We see that when we query the LoadBalancer IP, it is giving us result/content from all four containers. None of the curl commands is timed out. Now, if we kill three out of four pods, the service should still respond, without timing out. We let the loop run in a separate terminal, and kill three pods of this deployment from another terminal.

```shell
$ kubectl delete pod customnginx-3557040084-1z489 customnginx-3557040084-3hhlt customnginx-3557040084-c6skw
pod "customnginx-3557040084-1z489" deleted
pod "customnginx-3557040084-3hhlt" deleted
pod "customnginx-3557040084-c6skw" deleted
```

Immediately check the other terminal for any failed curl commands or timeouts.

```shell
Container IP: 100.96.1.150 <BR></p>
Container IP: 100.96.1.150 <BR></p>
Container IP: 100.96.2.37 <BR></p>
Container IP: 100.96.1.149 <BR></p>
Container IP: 100.96.1.149 <BR></p>
Container IP: 100.96.1.150 <BR></p>
Container IP: 100.96.2.36 <BR></p>
Container IP: 100.96.2.37 <BR></p>
Container IP: 100.96.2.37 <BR></p>
Container IP: 100.96.2.38 <BR></p>
Container IP: 100.96.2.38 <BR></p>
Container IP: 100.96.2.38 <BR></p>
Container IP: 100.96.1.151 <BR></p>
```

We notice that no curl command failed, and actually we have started seeing new IPs. Why is that? It is because, as soon as the pods are deleted, the deployment sees that it's desired state is four pods, and there is only one running, so it immediately starts three more to reach that desired state. And, while the pods are in process of starting, one surviving pod takes the traffic.

```shell
$ kubectl get pods
NAME                           READY     STATUS        RESTARTS   AGE
customnginx-3557040084-0s7l8   1/1       Running       0          15s
customnginx-3557040084-1z489   1/1       Terminating   0          16m
customnginx-3557040084-3hhlt   1/1       Terminating   0          16m
customnginx-3557040084-bvtnh   1/1       Running       0          15s
customnginx-3557040084-c6skw   1/1       Terminating   0          16m
customnginx-3557040084-fw1t3   1/1       Running       0          16m
customnginx-3557040084-xqk1n   1/1       Running       0          15s
```

This proves, Kubernets provides us High Availability, using multiple replicas of a pod.

## Clean up

Delete deployments and services as follow:

```shell
$ kubectl delete deployment customnginx
$ kubectl delete deployment multitool
$ kubectl delete service customnginx
```



==============================================================================================================================================================


# Init containers:
At times you may want to do some prep work for a container before starting it. That pre-work could be done by another container, which would do it's thing and exit before the main container starts. One example could be that you want to serve some static website content which exists as a git hub repository. So you would want something to pull that static content and provide it to the web server. This is called init-container. 

Below is an example, in which we have a "simple-website". The website exists as git repository at `https://github.com/fly2nadeem/simple-website.git` , we want to serve it through the nginx service.

Here is the yaml file which contains the definition of a pod that uses an init container.
```
$ cat support-files/init-container-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-container-demo
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: web-content-dir
      mountPath: /usr/share/nginx/html
  initContainers:
  - name: helper
    image: alpine/git
    command:
    - git 
    - clone
    - https://github.com/fly2nadeem/simple-website.git
    - /web-content/
    volumeMounts:
    - name: web-content-dir
      mountPath: "/web-content"
  volumes:
  - name: web-content-dir
    emptyDir: {}
```

Create the pod using the following command:

```
$ kubectl create -f support-files/init-container-pod.yaml 
pod "init-container-demo" created
$ 
```

Watch the pod going through several phases:
```
$ kubectl get pods -w
NAME                         READY     STATUS     RESTARTS   AGE
init-container-demo          0/1       Init:0/1   0          0s
multitool-5558fd48d4-snr8j   1/1       Running    0          22h
init-container-demo   0/1       Init:0/1   0         4s
init-container-demo   0/1       PodInitializing   0         6s
init-container-demo   1/1       Running   0         7s
$ 
```

Final state would look like this:
```
$ kubectget pods 
NAME                         READY     STATUS    RESTARTS   AGE
init-container-demo          1/1       Running   0          1m
multitool-5558fd48d4-snr8j   1/1       Running   0          22h
$ 
```

Examine the pod using `kubectl describe` and also by logging into it using `kubectl exec` .


```
PS C:\Users\Samu\.kube> kubectl exec -it init-container-demo /bin/sh
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # ls /bin
arch           cp             false          ionice         ls             mv             reformime      stat           watch
ash            date           fatattr        iostat         lzop           netstat        rev            stty           zcat
base64         dd             fdflush        ipcalc         makemime       nice           rm             su
bbconfig       df             fgrep          kbd_mode       mkdir          pidof          rmdir          sync
busybox        dmesg          fsync          kill           mknod          ping           run-parts      tar
cat            dnsdomainname  getopt         link           mktemp         ping6          sed            touch
chgrp          dumpkmap       grep           linux32        more           pipe_progress  setpriv        true
chmod          echo           gunzip         linux64        mount          printenv       setserial      umount
chown          ed             gzip           ln             mountpoint     ps             sh             uname
conspy         egrep          hostname       login          mpstat         pwd            sleep          usleep
/ # ls /usr/share/nginx/html/
simple-website
/ # exit
PS C:\Users\Samu\.kube> kubectl exec -it multitool-6d598c8f8-lt9st bash
bash-5.0# exit
exit

```


```
PS C:\Users\Samu\.kube> kubectl get pod -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
init-container-demo         1/1     Running   0          10m     172.17.0.5   minikube   <none>           <none>
multitool-6d598c8f8-lt9st   1/1     Running   0          4h19m   172.17.0.7   minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl exec -it multitool-6d598c8f8-lt9st bash
bash-5.0# curl 172.17.0.5
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.17.6</center>
</body>
</html>
bash-5.0# exit
exit

```


```
S C:\Users\Samu\.kube> kubectl describe pod  init-container-demo
Name:               init-container-demo
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               minikube/172.18.57.156
Start Time:         Tue, 31 Dec 2019 19:13:10 +0530
Labels:             <none>
Annotations:        <none>
Status:             Running
IP:                 172.17.0.5
Init Containers:
  helper:
    Container ID:  docker://e18d67d7d81f548df17f5bcb3e4614053a78b16e502cd1934118a3bba2c4e737
    Image:         alpine/git
    Image ID:      docker-pullable://alpine/git@sha256:8f5659025d83a60e9d140123bb1b27f3c334578aef10d002da4e5848580f1a6c
    Port:          <none>
    Host Port:     <none>
    Command:
      git
      clone
      https://github.com/fly2nadeem/simple-website.git
      /web-content/
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Tue, 31 Dec 2019 19:14:09 +0530
      Finished:     Tue, 31 Dec 2019 19:18:29 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-2crx5 (ro)
      /web-content from web-content-dir (rw)
Containers:
  nginx:
    Container ID:   docker://32ac3b1c217ecaac775c12b18ebd2b161359657058589f0ad4c5f2442c405819
    Image:          nginx:alpine
    Image ID:       docker-pullable://nginx@sha256:0e61b143db3110f3b8ae29a67f107d5536b71a7c1f10afb14d4228711fc65a13
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 31 Dec 2019 19:18:32 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /usr/share/nginx/html from web-content-dir (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-2crx5 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  web-content-dir:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:
    SizeLimit:  <unset>
  default-token-2crx5:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-2crx5
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  11m    default-scheduler  Successfully assigned default/init-container-demo to minikube
  Normal  Pulling    11m    kubelet, minikube  Pulling image "alpine/git"
  Normal  Pulled     10m    kubelet, minikube  Successfully pulled image "alpine/git"
  Normal  Created    10m    kubelet, minikube  Created container helper
  Normal  Started    10m    kubelet, minikube  Started container helper
  Normal  Pulled     6m35s  kubelet, minikube  Container image "nginx:alpine" already present on machine
  Normal  Created    6m33s  kubelet, minikube  Created container nginx
  Normal  Started    6m32s  kubelet, minikube  Started container nginx
PS C:\Users\Samu\.kube> kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
init-container-demo         1/1     Running   0          28m
multitool-6d598c8f8-lt9st   1/1     Running   0          4h37m

```

We can now delete the init-container as our task is completed.

```
PS C:\Users\Samu\.kube> kubectl delete pod init-container-demo
pod "init-container-demo" deleted
PS C:\Users\Samu\.kube> kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
multitool-6d598c8f8-lt9st   1/1     Running   0          4h39m

```

# Multi-container pods and side-cars:
There are instances when you may have two containers in the same pod. A very primitiv example would be a pod in which one container generates the web content on continuous basis, and another container serves the web content. This is different from init container example. In the init-container example, the "puller" pulls the static content only once, saves it in a shared storage, and exists, and the main web-server container serves that static content. In this example there is no puller, instead there is a "content-generator", which will run as a **side-car** , and will constantly add content in the the shared storage volume, which the web server will use to serve the content.

Here is the code for such a multi-contianer pod:

```
$ cat multi-container-pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-demo
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: web-content-dir
      mountPath: /usr/share/nginx/html
  - name: content-generator
    image: busybox
    command: ['sh', '-c', 'while true; do echo Date and Time is $(date) >> /web-content/index.html && sleep 5; done']
    volumeMounts:
    - name: web-content-dir
      mountPath: "/web-content"

  volumes:
  - name: web-content-dir
    emptyDir: {}
$ 
```


Lets create this pod:

```
$ kubectl create -f multi-container-pod.yaml 
pod "multi-container-demo" created
$

$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP            NODE
multi-container-demo         2/2       Running   0          21s       10.200.0.22   kubeadm-node1
multitool-5558fd48d4-snr8j   1/1       Running   0          23h       10.200.1.28   kubeadm-node2
$
```

Lets login into the multitool and try to access the web page of the nginx container from the multicontainer-pod.

```
$ kubectl exec -it multitool-5558fd48d4-snr8j bash

bash-4.4# curl 10.200.0.22

Date and Time is Sat Dec 31 16:21:28 UTC 2019
Date and Time is Sat Dec 31 16:21:33 UTC 2019
Date and Time is Sat Dec 31 16:21:38 UTC 2019
Date and Time is Sat Dec 31 16:21:43 UTC 2019
Date and Time is Sat Dec 31 16:21:48 UTC 2019
bash-4.4#
```

Notice that the web page has entries with time difference of 5 seconds. If you "watch" this, you will see it in action.

```
bash-4.4# watch curl -s 10.200.0.22
Date and Time is Sat Dec 31 16:21:28 UTC 2019
Date and Time is Sat Dec 31 16:21:33 UTC 2019
Date and Time is Sat Dec 31 16:21:38 UTC 2019
Date and Time is Sat Dec 31 16:21:43 UTC 2019
Date and Time is Sat Dec 31 16:21:48 UTC 2019
Date and Time is Sat Dec 31 16:21:53 UTC 2019
Date and Time is Sat Dec 31 16:21:58 UTC 2019
Date and Time is Sat Dec 31 16:22:03 UTC 2019
```

Lets exec into the content-generator container in this multi-container pod. If you try to exec into any of the container in a multi-container pod, without specifying the name of the container, you will see the following message:

```
$ kubectl exec -it multi-container-demo /bin/sh
Defaulting container name to nginx.
Use 'kubectl describe pod/multi-container-demo -n default' to see all of the containers in this pod.
/ #
```

You can see that if you do not specify the container name while doing exec (or even log - later), you will be sent to the first container kubernetes sees in the pod, which may not be the correct one. In our case it defaulted to the nginx container. You need to use `kubectl describe pod <pod-name>` command to get the list of containers of a pod. In our case the name of the container is "content-generator".

So, now we will specify the exact container in the pod to exec into:

```shell
$ kubectl exec -it multi-container-demo -c content-generator /bin/sh
/ # ls
bin          etc          proc         sys          usr          web-content
dev          home         root         tmp          var

/ # ps
PID   USER     TIME  COMMAND
    1 root      0:00 sh -c while true; do echo Date and Time is $(date) >> /web-content/index.html && sleep 5; done
   74 root      0:00 /bin/sh
   84 root      0:00 sleep 5
   86 root      0:00 ps
/ # exit
$
``` 
I checked the processes running in that container and see that the command I specified in the container's specification, is running. 

Similarly, if I want to check logs of a container in a multi-container pod, I have to do the same:

```
$ kubectl logs -f multi-container-demo -c nginx 
10.200.1.28 - - [31/Dec/2019:16:22:05 +0000] "GET / HTTP/1.1" 200 360 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:21 +0000] "GET / HTTP/1.1" 200 495 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:28 +0000] "GET / HTTP/1.1" 200 540 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:35 +0000] "GET / HTTP/1.1" 200 630 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:37 +0000] "GET / HTTP/1.1" 200 630 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:39 +0000] "GET / HTTP/1.1" 200 675 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:41 +0000] "GET / HTTP/1.1" 200 675 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:43 +0000] "GET / HTTP/1.1" 200 675 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:45 +0000] "GET / HTTP/1.1" 200 720 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:47 +0000] "GET / HTTP/1.1" 200 720 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:49 +0000] "GET / HTTP/1.1" 200 765 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:51 +0000] "GET / HTTP/1.1" 200 765 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:53 +0000] "GET / HTTP/1.1" 200 765 "-" "curl/7.61.1" "-"
10.200.1.28 - - [31/Dec/2019:16:22:55 +0000] "GET / HTTP/1.1" 200 810 "-" "curl/7.61.1" "-"
```

==============================================================================================================================================================


# Why to use CPU and memory limits?
It is possible that the worker node in the kubernetes cluster you received from your instructor, is a small spec node (1 CPU and 1.7 GB RAM). In that case, please note that certain *system pods* running in **kube-system** namespace take away as much as 60% (or 600m) of the CPU, and you are left with 40% of CPU to run your pods in order to do these excercises. It is also important to note that by default each `nginx` and `nginx:alpine` instance will take away 10% (or 100m) CPU. (This behavior was seen on a small one-node k8s cluster in GCP). This means, that at any given time, you will not be able to run more than four (light weight) pods in total. So when you do the **scaling** and **rolling updates** exercises, you need to keep that in mind. You can limit the use of CPU and Memory resources in your pods and assign them very little CPU. e.g. Allocating `5m` CPU to nginx pods does not have any negative effect, and they just run fine.

If you are running a cluster of your own such as minikube or kubeadm based cluster, and if you setup the VMs to have multiple CPUs during the VM setup, you will not likely encounter this limitation. But you should still make a note of it. However, this does not mean that you do not setup these limits in your deployments just because you have large sized worker nodes. It is actually highly recommended that you setup the CPU and memory limits for all your pods/deployments to protect abuse of system resources. This abuse may happen if an application has a fault in it's code and starts over-consuming cpu and/or memory. It is also possible that someone gains access to a pod running inside your cluster, and starts abusing with malicious intent, such as sending spam, or mining coins, etc. We have actually seen this happening! :) 

It is also useful to set these limits because it is useful for the Kubernetes Scheduler. If the scheduler knows how much cpu and memory a pod needs , it will find the right nodes to run those pods on. If there are no cpu and memory limits defined, then the scheduler has absolutely no clue on where to place thse pods, and it just uses it's best guess to place it on a node and hopes that it works! :) (There is a bit more to it, but this is the essence). 

Check this document for more information: [https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)


## Example of not setting CPU and memory limits:
Lets create and run a simple nginx deployment without using any CPU or memory limits and see what is going on in the cluster:

```
$ kubectl run nginx --image=nginx:alpine
deployment.apps "nginx" created

$ kubectl get rs
NAME                   DESIRED   CURRENT   READY     AGE
multitool-5558fd48d4   1         1         1         10d
nginx-6b4b85b77b       1         1         1         15m

$ kubectl get pods
NAME                         READY     STATUS    RESTARTS   AGE
multitool-5558fd48d4-pqkj5   1/1       Running   1          10d
nginx-6b4b85b77b-w8xww       1/1       Running   0          15s
```


You can *describe* the nginx deployment and the related pod and replica-set. If no cpu and memory limits were provided when this deployment was created, then you will not find that information in the describe deployment|rs|pod commands. 

```
$ kubectl describe deployment nginx

$ kubectl describe rs nginx-6b4b85b77b

$ kubectl describe pod nginx-6b4b85b77b-w8xww
```


Let's check which node the pod is running on and investigate the node:
```
$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
multitool-5558fd48d4-pqkj5   1/1       Running   1          10d       172.17.0.4   minikube
nginx-6b4b85b77b-w8xww       1/1       Running   0          17m       172.17.0.5   minikube
```

We have *some* information about resources in the worker node.

```
$ kubectl describe node minikube
Name:               minikube
Roles:              master
. . . 
Addresses:
  InternalIP:  192.168.122.249
  Hostname:    minikube
Capacity:
 cpu:                2
 ephemeral-storage:  16058792Ki
 hugepages-2Mi:      0
 memory:             1942288Ki
 pods:               110
Allocatable:
 cpu:                2
 ephemeral-storage:  14799782683
 hugepages-2Mi:      0
 memory:             1839888Ki
 pods:               110
. . . 
Non-terminated Pods:         (11 in total)
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                                ------------  ----------  ---------------  -------------
  default                    multitool-5558fd48d4-pqkj5          0 (0%)        0 (0%)      0 (0%)           0 (0%)
  default                    nginx-6b4b85b77b-w8xww              0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                coredns-86c58d9df4-fhqpl            100m (5%)     0 (0%)      70Mi (3%)        170Mi (9%)
  kube-system                coredns-86c58d9df4-pljtz            100m (5%)     0 (0%)      70Mi (3%)        170Mi (9%)
  kube-system                etcd-minikube                       0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-addon-manager-minikube         5m (0%)       0 (0%)      50Mi (2%)        0 (0%)
  kube-system                kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-proxy-4k6vs                    0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)
  kube-system                storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ------------  ----------  ---------------  -------------
  755m (37%)    0 (0%)      190Mi (10%)      340Mi (18%)
Events:         <none>
```


Notice that the `CPU Requests`, `CPU Limits`, `Memory Requests` and `Memory Limits` are all set to `0` or `0%` - for nginx pod. This means there are no limits for this pod, and if this pod is somehow abused, it can consume entire CPU and memory of this worker node.


## Example of CPU and memory limits - using nginx:

Lets delete the existing nginx deployment.

```
$ kubectl delete deployment nginx
```

Create a new one with some limits. Here is what the deployment file looks like, with cpu and memory limits defined in it
```
$ cat support-files/nginx-with-cpu-memory-limit.yaml 
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx          # arbitrary label(s) assigned to this deployment. This can be used by an upstream object, such as - a service.
spec:
  replicas: 1
  selector:
    matchLabels:        # labels 'used' by the deployment and replica-set selector, to find related pods.
      app: nginx
  template:
    metadata:
      labels:
        app: nginx      # label assigned to the pods of this deployment
    spec:
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: "1m"
            memory: "5Mi"
          limits:
            cpu: "5m"
            memory: "10Mi"
```


Create the deployment:
```
$ kubectl create -f nginx-with-cpu-memory-limit.yaml 
deployment.extensions/nginx created

$ kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
multitool-5558fd48d4-pqkj5   1/1     Running   1          10d
nginx-84cc7d8b59-5flwm       1/1     Running   1          5s
```

Now, lets check deployment, rs and pods for cpu and memory information.

```
$ kubectl describe deployment nginx
Name:                   nginx
Namespace:              default
. . . 
  Containers:
   nginx:
    Image:      nginx:1.9.1
    Port:       80/TCP
    Host Port:  0/TCP
    Limits:
      cpu:     5m
      memory:  10Mi
    Requests:
      cpu:        1m
      memory:     5Mi
. . . 
```


Check the related replica-set for the same information:
```
$ kubectl describe rs nginx-84cc7d8b59
Name:           nginx-84cc7d8b59
Namespace:      default
. . . 
Controlled By:  Deployment/nginx
. . . 
  Containers:
   nginx:
    Image:      nginx:1.9.1
    Port:       80/TCP
    Host Port:  0/TCP
    Limits:
      cpu:     5m
      memory:  10Mi
    Requests:
      cpu:        1m
      memory:     5Mi
. . . 
```

Check the pod for cpu and memory information:

```
$ kubectl describe pod nginx-84cc7d8b59-5flwm
Name:               nginx-84cc7d8b59-5flwm
Namespace:          default
. . . 
Node:               minikube/192.168.122.249
. . . 
Controlled By:      ReplicaSet/nginx-84cc7d8b59
Containers:
  nginx:
    Image:          nginx:1.9.1
. . .
    Limits:
      cpu:     5m
      memory:  10Mi
    Requests:
      cpu:        1m
      memory:     5Mi
. . .
QoS Class:       Burstable
. . . 
```

This time all three objects (deployment, replica-set and pods), have the cpu and memory information. Also notice that the QoS class for the pod has changed from "Best Effort" to "Burstable". You will notice this if you did a `describe` earlier, on a pod without cpu and memory limits set.


If we do a `describe` on the worker node now, we will see that the node knows about these limits for the nginx pod.

```
$ kubectl describe node minikube
Name:               minikube
Roles:              master
. . . 
Taints:             <none>
. . .
  Hostname:    minikube
. . . 
Non-terminated Pods:         (11 in total)
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                ------------  ----------  ---------------  -------------  ---
  default                    multitool-5558fd48d4-pqkj5          0 (0%)        0 (0%)      0 (0%)           0 (0%)         10d
  default                    nginx-84cc7d8b59-5flwm              1m (0%)       5m (0%)     5Mi (0%)         10Mi (0%)      12m
  kube-system                coredns-86c58d9df4-fhqpl            100m (5%)     0 (0%)      70Mi (3%)        170Mi (9%)     19d
  kube-system                coredns-86c58d9df4-pljtz            100m (5%)     0 (0%)      70Mi (3%)        170Mi (9%)     19d
  kube-system                etcd-minikube                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         19d
  kube-system                kube-addon-manager-minikube         5m (0%)       0 (0%)      50Mi (2%)        0 (0%)         19d
  kube-system                kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)         19d
  kube-system                kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)         19d
  kube-system                kube-proxy-4k6vs                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         4d13h
  kube-system                kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)         19d
  kube-system                storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         19d
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests     Limits
  --------           --------     ------
  cpu                756m (37%)   5m (0%)
  memory             195Mi (10%)  350Mi (19%)
  ephemeral-storage  0 (0%)       0 (0%)
Events:              <none>
```


## Example of CPU and memory limits - using stress:

In this exercise, you create a Pod that has one Container. The Container has a request of 0.5 CPU and a limit of 1 CPU. Here is the configuration file for the Pod: 

```
$ cat support-files/stress-cpu-request-limit-pod.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: cpu-stress-demo
spec:
  containers:
  - name: cpu-stress-demo
    image: vish/stress
    resources:
      limits:
        cpu: "1"
      requests:
        cpu: "0.5"
    args:
    - -cpus
    - "2"

```


Lets run this pod and see what it does.

```
$ kubectl create -f support-files/stress-cpu-request-limit-pod.yaml 
pod/cpu-stress-demo created

$ kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
cpu-stress-demo              1/1     Running   0          6s    172.17.0.6   minikube   <none>           <none>
multitool-5558fd48d4-pqkj5   1/1     Running   1          10d   172.17.0.4   minikube   <none>           <none>
nginx-84cc7d8b59-5flwm       1/1     Running   1          29m   172.17.0.5   minikube   <none>           <none>
```

Lets check the node's status straight away:

```
$ kubectl describe node minikube
Name:               minikube
Roles:              master
Taints:             <none>
. . . 
  Namespace                  Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                ------------  ----------  ---------------  -------------  ---
  default                    cpu-stress-demo                     500m (25%)    1 (50%)     0 (0%)           0 (0%)         41s
  default                    multitool-5558fd48d4-pqkj5          0 (0%)        0 (0%)      0 (0%)           0 (0%)         10d
  default                    nginx-84cc7d8b59-5flwm              1m (0%)       5m (0%)     5Mi (0%)         10Mi (0%)      29m
. . . 
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests     Limits
  --------           --------     ------
  cpu                1256m (62%)  1005m (50%)
  memory             195Mi (10%)  350Mi (19%)
  ephemeral-storage  0 (0%)       0 (0%)
Events:              <none>
```

If you login to your worker node over ssh, you will see the process hogging the CPU and load average going high:

Load Average:
```
$ uptime
 11:06:24 up 4 days, 21:45,  1 user,  load average: 1.43, 0.98, 0.73
```

CPU usage - using OS `top` command:
```
$ top -b -o %CPU
top - 11:10:38 up 4 days, 21:49,  1 user,  load average: 3.66, 1.59, 0.98
Tasks: 132 total,   2 running, 130 sleeping,   0 stopped,   0 zombie
%Cpu0  :  20.0/46.7   67[|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                 ]
%Cpu1  :  12.5/43.8   56[|||||||||||||||||||||||||||||||||||||||||||||||||||||||||                                           ]
GiB Mem : 68.1/1.9      [                                                                                                    ]
GiB Swap:  6.5/1.0      [                                                                                                    ]

  PID USER      PR  NI    VIRT    RES  %CPU  %MEM     TIME+ S COMMAND
24966 root      20   0    5.4m   3.7m 100.0   0.2   6:20.23 S /stress -logtostderr -cpus 2
 2991 root      20   0   10.1g  80.5m   6.2   4.2 121:05.99 S etcd --advertise-client-urls=https://192.168.39.48:2379 --cert-file=/var+
    1 root      20   0  181.2m   7.7m   0.0   0.4   0:23.64 S /sbin/init noembed norestore
    2 root      20   0    0.0m   0.0m   0.0   0.0   0:00.01 S [kthreadd]
    4 root       0 -20    0.0m   0.0m   0.0   0.0   0:00.00 I [kworker/0:0H]
    6 root       0 -20    0.0m   0.0m   0.0   0.0   0:00.00 I [mm_percpu_wq]
```

Lets kill this pod.

```
$ kubectl delete -f support-files/stress-cpu-request-limit-pod.yaml 
pod "cpu-stress-demo" deleted
```



# Best practice
Often the developers forget to setup the cpu and memory requirement/limits for their applications in the deployment definitions. If an abusive/broken app is run, it can take down an entire worker node; and if that app has multiple replicas running on multiple worker nodes, then multiple worker nodes will be severely affected. It is best to setup a very low default CPU and memory limit on namespace level, so if someone runs a deployment without the limits, their applications will either not start, or will be very slow. They can then incorporate these requirements and limits in their pod/deployment definitions to fix this.

Check this document on how to setup default CPU and memory limits on namespace level: [https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)


==============================================================================================================================================================

# Replicas and Rolling update:

## Create Deployment

Recreate the nginx deployment that we did earlier:

```shell
$ kubectl create deployment nginx --image=nginx:1.7.9
```

And, expose the pod using a load balancer service (remember that it might take a
few minutes for the cloud infrastructure to deploy the load balancer, i.e. the
external IP might be shown as `pending`):

```shell
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
```

Note down the loadbalancer IP from the services command:

```shell
$ kubectl get service
```

In case, you are doing this excercise on your local kubernetes clsuter (minikube, kubeadm, etc), then you can simply expose this service as NodePort and use the worker-node-name/IP:nodeport to achieve the same.

```
$ kubectl expose deployment nginx --port 80 --type NodePort
```

```
$ kubectl get services
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.32.0.1     <none>        443/TCP        1h
nginx        NodePort    10.32.254.6   <none>        80:30900/TCP   7m
$ 
```


Increase the replicas to four:

```shell
$ kubectl scale deployment nginx --replicas=4
```

From another terminal on your machine check (using load balancer IP) which version is currently running and to see changes when rollout is happening:

```shell
$ while true; do  curl -sI 35.205.60.29  | grep Server; sleep 2; done
```

On local kubernetes cluster, it would be:
```
$ while true; do  curl -sI k8s-worker-node:30900  | grep Server; sleep 1; done
```


## Update Deployment

Rollout an update to  the image:

```shell
$ kubectl set image deployment nginx nginx=nginx:1.9.1 --record
```

Check the rollout status:

```shell
$ kubectl rollout status deployment nginx
```

Investigate rollout history:

```shell
$ kubectl rollout history deployment nginx
```

Try rolling out other image version by repeating the `set image` command from
above.  Suggested image versions are 1.12.2, 1.13.12, 1.14.1, 1.15.2.

Try also rolling out a version that does not exist:

```shell
$ kubectl set image deployment nginx nginx=nginx:100.200.300 --record
```

what happened - do the curl operation still work?  Investigate the running pods with:

```shell
$ kubectl get pods
```
You should see `ImagePullBackOff` under STATUS of some of the pods. 


## Undo Update

The rollout above using a non-existing image version caused some pods to be
non-functioning. Next, we will undo this faulty deployment. First, investigate
rollout history:

```shell
$ kubectl rollout history deployment nginx
```

Undo the rollout and restore the previous version:

```shell
$ kubectl rollout undo deployment nginx
```

Investigate the running pods:

```shell
$ kubectl get pods
```

```
PS C:\Users\Samu\.kube> kubectl set image deployment nginx nginx=nginx:1.9.3 --record
deployment.apps/nginx image updated
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h28m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          13m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          13m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-cjjqh       1/1     Terminating         0          13m     172.17.0.9    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          69m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          4s      <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          3s      <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h29m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          13m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          13m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          69m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          18s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          17s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h29m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          13m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          13m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          69m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          22s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          21s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl rollout status deployment nginx
Waiting for deployment "nginx" rollout to finish: 2 out of 4 new replicas have been updated...
PS C:\Users\Samu\.kube> TerminatingError(): "The pipeline has been stopped."
>> TerminatingError(): "The pipeline has been stopped."
PS C:\Users\Samu\.kube> kubectl rollout history deployment nginx
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl.exe set image deployment nginx nginx=nginx:1.9.3 --record=true
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h30m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          14m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          14m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          70m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          78s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          77s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h30m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          14m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          14m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          70m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          81s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          80s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h30m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          15m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          15m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          70m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          84s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          83s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running             0          6h30m   172.17.0.7    minikube   <none>           <none>
nginx-5997c9cfc-627tq       1/1     Running             0          15m     172.17.0.10   minikube   <none>           <none>
nginx-5997c9cfc-bltnq       1/1     Running             0          15m     172.17.0.8    minikube   <none>           <none>
nginx-5997c9cfc-wjzpl       1/1     Running             0          70m     172.17.0.5    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      0/1     ContainerCreating   0          87s     <none>        minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      0/1     ContainerCreating   0          86s     <none>        minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running   0          6h33m   172.17.0.7    minikube   <none>           <none>
nginx-7d6fccd947-95pf5      1/1     Running   0          2m45s   172.17.0.8    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      1/1     Running   0          5m2s    172.17.0.11   minikube   <none>           <none>
nginx-7d6fccd947-m8s6x      1/1     Running   0          2m54s   172.17.0.9    minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      1/1     Running   0          5m1s    172.17.0.12   minikube   <none>           <none>
PS C:\Users\Samu\.kube> kubectl rollout history deployment nginx
deployment.apps/nginx
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl.exe set image deployment nginx nginx=nginx:1.9.3 --record=true
PS C:\Users\Samu\.kube> kubectl get pods -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
multitool-6d598c8f8-lt9st   1/1     Running   0          6h34m   172.17.0.7    minikube   <none>           <none>
nginx-7d6fccd947-95pf5      1/1     Running   0          3m      172.17.0.8    minikube   <none>           <none>
nginx-7d6fccd947-ghhwx      1/1     Running   0          5m17s   172.17.0.11   minikube   <none>           <none>
nginx-7d6fccd947-m8s6x      1/1     Running   0          3m9s    172.17.0.9    minikube   <none>           <none>
nginx-7d6fccd947-tcdzh      1/1     Running   0          5m16s   172.17.0.12   minikube   <none>           <none>

```

## Clean up

Delete deployments and services as follow:

```shell
$ kubectl delete deployment nginx
$ kubectl delete service nginx
```


==============================================================================================================================================================

# Kubernetes secrets and Config maps


## A simple .js application:
Our [maginificent app](./secrets/secretapp.js) requries an API key and a value for the variable `LANGUAGE`. Here is what our application looks like initially:

```
$ cat secrets/secretapp.js 
var http = require('http');
var server = http.createServer(function (request, response) {
  const LANGUAGE = 'English';
  const API_KEY = 'abc-123-456-789-xyz';
  response.write(`Language: ${LANGUAGE}\n`);
  response.write(`API Key: ${API_KEY}\n`);
  response.end(`\n`);
});
server.listen(3000);
```

Notice, we have hard-coded the values for API_KEY and LANGUAGE:

```
  const LANGUAGE = 'English';
  const API_KEY = 'abc-123-456-789-xyz';
```

Rather than hardcode this sensitive information and commit it to git repository for all the world to see, we should source these values from environment variables. 

The first step to fixing it, would be to make sure that our variables get their values from the process's environment. So, we change the code like this:

```shell
  const LANGUAGE = process.env.LANGUAGE;
  const API_KEY = process.env.API_KEY;
```

Lets create a Docker container for this app and pass these values as environment variables in the container:

```shell
$ cat secrets/Dockerfile

FROM node:9.1.0-alpine
EXPOSE 3000
ENV LANGUAGE English
ENV API_KEY abc-123-456-789-xyz
COPY secretapp.js .
ENTRYPOINT node secretapp.js
```

Lets run this Docker container image as a pod in our Kubernetes cluster. (This container image is available as `praqma/secrets-demo`). We can run that in our Kubernetes cluster by using the [the deployment file](secrets/deployment.yml). Here is what the deployment file looks like:

```
$ cat secrets/deployment.yml 
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: envtest
spec:
  replicas: 1
  template:
    metadata:
      labels:
        name: envtest
    spec:
      containers:
      - name: envtest
        image: praqma/secrets-demo
        imagePullPolicy: Always
        ports:
        - containerPort: 3000
        env:
        - name: LANGUAGE
          value: Polish
        - name: API_KEY
          value: def-333-444-555-jkl
```

Notice the env values added at the bottom of the deployment file. Create/run the deployment:

```shell
$ kubectl apply -f secrets/deployment.yml
deployment.extensions/envtest created
```

Expose the deployment on a nodeport. Remember that this application runs on port `3000`.

```
$ kubectl get deployments
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
envtest     1         1         1            0           14s
multitool   1         1         1            1           2h
nginx       4         4         4            4           2h

$ kubectl expose deployment envtest --type=NodePort
service/envtest exposed

$ kubectl get services
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)          AGE
envtest   NodePort       10.59.252.33    <none>          3000:30771/TCP   9s
nginx     LoadBalancer   10.59.240.197   146.148.21.83   80:32423/TCP     1h
```

Now, notice that despite the default value in the Dockerfile, it should now be overwritten by the values of environment variables in deployment  file! Though the problem still is, that we moved the hard coded values from our application code to the deployment file. The deployment file will of-course be part of the related git repository, and these variables and their values will be visible to anyone. We need to find a better way of passing the variables to the application/pod.

### Secrets using the kubernetes secret resource

Let's create the API key as a kubernetes **secret**:

```shell
$ kubectl create secret generic apikey --from-literal=API_KEY=def-333-444-555-jkl
secret/apikey created
```

Kubernetes supports different kinds of preconfigured secrets, but for now we'll deal with a generic one.

Similarly create `language` as kubernetes **configmap**:

```shell
$ kubectl create configmap language --from-literal=LANGUAGE=German
configmap/language created
```

Now run `kubectl get secets` and `kubectl get configmap` to see if these objects are created:

```shell
$ kubectl get secrets
NAME                  TYPE                                  DATA      AGE
apikey                Opaque                                1         4m
default-token-td78d   kubernetes.io/service-account-token   3         3h
```

```shell
$ kubectl get configmaps
NAME       DATA      AGE
language   1         2m
```

> Try to investigate the secret by using the kubectl describe command:
> ```shell
> $ kubectl describe secret apikey
> ```
> Note that the actual value of API_KEY is not shown. To see the encoded value use:
> ```shell
> $ kubectl get secret apikey -o yaml
> ```

Last step is to change the Kubernetes deployment (secrets/deployment.yml) file to use the secrets.

Change this:

```shell
        env:
        - name: LANGUAGE
          value: Polish
        - name: API_KEY
          value: def-333-444-555-jkl
```

To this:

```shell
        env:
        - name: LANGUAGE
          valueFrom:
            configMapKeyRef:
              name: language
              key: LANGUAGE
        - name: API_KEY
          valueFrom:
            secretKeyRef:
              name: apikey
              key: API_KEY
```

After you have edited the `deployment.yml` file (or you can use the prepared one `secrets/final.deployment.yml`), you need to apply the new version of the file by using: `kubectl apply -f deployment.yml` .

You should now see the variables being loaded from configmap and secret respectively.

#### What happens when secrets and configmaps change?
Pods are not recreated automatically when serets or configmaps change. After you change the secrets and config maps values, you need to restart the pods which are using those secrets and config maps. 

Here is an example of changing/updating an existing secret and config map:
```shell
$ kubectl create configmap language --from-literal=LANGUAGE=German -o yaml --dry-run | kubectl replace -f -
configmap/language replaced

$ kubectl create secret generic apikey --from-literal=API_KEY=klm-333-444-555-pqr -o yaml --dry-run | kubectl replace -f -
secret/apikey replaced
```

Then delete the pod (so it's recreated with the replaced configmap and secret) :

```shell
$ kubectl delete pod envtest-3380598928-kgj9d
pod "envtest-3380598928-kgj9d" deleted
```

Access it in a web browser again, to see the updated values. You need the (external) IP of any of the worker nodes to reach this service over NodePort.

```
$ kubectl get nodes -o wide
NAME                                                  STATUS   ROLES    AGE   VERSION          INTERNAL-IP   EXTERNAL-IP     OS-IMAGE                             KERNEL-VERSION   CONTAINER-RUNTIME
gke-ndc-oslo-training-cl-default-pool-654cdad8-5gpf   Ready    <none>   4h    v1.11.7-gke.12   10.123.0.8    104.155.27.73   Container-Optimized OS from Google   4.14.91+         docker://17.3.2
gke-ndc-oslo-training-cl-default-pool-654cdad8-g1qf   Ready    <none>   4h    v1.11.7-gke.12   10.123.0.7    35.233.51.246   Container-Optimized OS from Google   4.14.91+         docker://17.3.2
gke-ndc-oslo-training-cl-default-pool-654cdad8-r09t   Ready    <none>   4h    v1.11.7-gke.12   10.123.0.6    35.240.117.25   Container-Optimized OS from Google   4.14.91+         docker://17.3.2 
```


----------

## nginx web-server on SSL - uses secrets (as files) and configmaps (as files):
This example is more suited for system administrators. In this example, we want to run nginx web-server, which listens on port 443, by using some self-signed SSL certificates. This involves:

* providing SSL certificates to nginx pods, and,
* providing a custom nginx configuration, so the pods know how to use these certificates and what port to listen on.

To achieve these objectives, we will create SSL certs as secrets, and a custom nginx configuration as configmap, and use them in our deployment.


Generate self signed certs: (check support-files/  directory)
```
./generate-self-signed-certs.sh
```
This will create `tls.*` files.


Create  (tls type) secret for nginx:

```
kubectl create secret tls nginx-certs --cert=tls.crt --key=tls.key
```

Examine the secret you just created:
```
kubectl describe secret nginx-certs
```

```
kubectl get secret nginx-certs -o yaml
```


Create a custom configuration nginx: (check support-files/  directory)

```
$ cat support-files/nginx-connectors.conf
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}

server {
    listen       443;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    ssl on;
    ssl_certificate /certs/tls.crt;
    ssl_certificate_key /certs/tls.key;
}
```


```
kubectl create configmap nginx-config --from-file=support-files/nginx-connectors.conf
```

Examine the configmap you just created:

```
kubectl describe configmap nginx-config
```

```
kubectl get configmap nginx-config -o yaml
```


Create a nginx deployment with SSL support using the secret and config map you created in the previous steps (above): (check support-files/  directory)

```
$ cat support-files/nginx-ssl.yaml 
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: certs-volume
        secret:
          secretName: nginx-certs
      - name: config-volume
        configMap:
          name: nginx-config
      containers:
      - name: nginx
        image: nginx:1.15.1
        ports:
        - containerPort: 443
        - containerPort: 80
        volumeMounts:
        - mountPath: /certs
          name: certs-volume
        - mountPath: /etc/nginx/conf.d
          name: config-volume
```


```
kubectl create -f nginx-ssl.yaml
```

You should be able to see nginx running. Expose it as a service and curl it from your computer. You can also curl it through the multitool pod from within the cluster.




==============================================================================================================================================================

# Storage

There are several ways for making data peristent. Few of them will be explained in this kata.

## emtpryDir
An “emptyDir” volume type is always empty at the pod start time and gets data from whatever the container process(es) - or other containers in the same pod - fill it with. As soon as the pod exits - or dies - the emptyDir volume is deleted with it. If the pod moves from one node to another, the contents of emptyDir are deleted permanently. So this is not really an example of data persistence. But it helps explain certain point.

Some uses for an emptyDir are:
* scratch space, such as for a disk-based merge sort
* checkpointing a long computation for recovery from crashes
* holding files that a content-manager container fetches while a webserver container serves the data

Example:
Say you have a git repository, which contains your static web-content; which you want to serve through a web server running in a container. For this to work, you use a multi-container pod, with web-server being the main container; and a helper container, which pulls the latest from the web-content git repository and put it in a special emptyDir based volume. This volume is then shared with the web-server, which mounts the same volume on it’s document-root mount point, and serves that content. We have already seen this in the init-containers exercise [02-init-and-multi-container-pods.md](02-init-and-multi-container-pods.md). Here it is once again:

```
$ cat support-files/init-container-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-container-demo
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - name: web-content-dir
      mountPath: /usr/share/nginx/html
  initContainers:
  - name: helper
    image: alpine/git
    command:
    - git 
    - clone
    - https://github.com/Praqma/simple-website.git
    - /web-content/
    volumeMounts:
    - name: web-content-dir
      mountPath: "/web-content"
  volumes:
  - name: web-content-dir
    emptyDir: {}
```


## hostPath
A `hostPath` volume type will always mount a directory from the host file system into a mount point inside the pod. Whatever the contents of the said host directory, will show up in the mount point in the container. On a Kubernetes cluster, hostPath volume will prove to be a nightmare for its use as a general persistent-storage for running containers, because what if the container moves from one node to another? How will the volume move from one node to another? It won’t - and that is not the use-case for this type of volume. 

Containers which are more of *system* containers, and are concerned with only one node on which they run, may use this volume type. Say you want to monitor/read/process information about any running containers on a node. To be able to do that, the monitoring container would need to mount certain host directories from the host on certain mount points inside that container. **cAdvisor** is an example of a container which needs to mount host paths. 

Another special case where you can use hostPath as persistent storage option is the test/development single node clusters, such as minikube. On such clusters, since there is just one node, you can pretty much assure that whenever a pod restarts, it will always be on the same node and will find it's data; and also, that the data in certain host directories /hostPath), will be safe / persistent. 

However, one important one to keep under consideration is that the files or directories created on the underlying hosts are only writable by `root`. You either need to run your process as root in a privileged Container or modify the file permissions on the host to be able to write to a hostPath volume. This will be a challenge most of the time, and that is what makes hostPath a bad choice for being used as persistent storage.

Below is an example of a pod using hostPath.


```
$ cat support-files/hostPath-example-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-example-pod
spec:
  containers:
  - image: nginx:alpine
    name: web-server
    volumeMounts:
    - name: web-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: web-volume
    hostPath:
      # directory location on host
      path: /data/web-server
      type: Directory
```

Create the pod:

```
$ kubectl create -f support-files/hostPath-example-pod.yaml 
pod "hostpath-example-pod" created
```

Notice the pod remains in `ContainerCreating` status:
```
$ kubectl get pods -w
NAME                         READY     STATUS              RESTARTS   AGE
hostpath-example-pod         0/1       ContainerCreating   0          45s
multitool-5558fd48d4-pqkj5   1/1       Running             0          2d
```

Investigate by describing the pod:
```
$ kubectl describe pod hostpath-example-pod
. . . 
Status:             Pending
. . . 
Events:
  Type     Reason       Age              From               Message
  ----     ------       ----             ----               -------
  Normal   Scheduled    1m               default-scheduler  Successfully assigned default/hostpath-example-pod to minikube
  Warning  FailedMount  5s (x8 over 1m)  kubelet, minikube  MountVolume.SetUp failed for volume "web-volume" : hostPath type check failed: /data/web-server is not a directory
```

On the actual host (kubernetes worker node), the hostPath directory does not exist.

Check by logging into the host, in this case minikube:

``` 
$ minikube ssh

# ls -l /data/
total 4
drwxr-xr-x 3 root root 4096 Mar 10 00:08 minikube
# 
```

So there is no directory called `web-server` on the host. Lets create it:

```
$ minikube ssh

# mkdir /data/web-server
# ls -l /data/
total 8
drwxr-xr-x 3 root root 4096 Mar 10 00:08 minikube
drwxr-xr-x 2 root root 4096 Mar 12 12:27 web-server
# 
```

Delete the failing pod and re-create it:
```
$ kubectl delete pod hostpath-example-pod
pod "hostpath-example-pod" deleted

$ kubectl create -f support-files/hostPath-example-pod.yaml 
pod "hostpath-example-pod" created
```

```
$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
hostpath-example-pod         1/1       Running   0          56s       172.17.0.5   minikube
multitool-5558fd48d4-pqkj5   1/1       Running   0          2d        172.17.0.4   minikube
```


Login to the pod and check if there is any file under `/usr/share/nginx/html` . There shouldn't be any. So lets create an `index.html` file there.

```
$ kubectl exec -it hostpath-example-pod /bin/sh

/ # ls -l /usr/share/nginx/html/
total 0
/ # 

/ # echo "Nginx - This is an example of hostPath data persistence!" > /usr/share/nginx/html/index.html 

/ # cat /usr/share/nginx/html/index.html 
Nginx - This is an example of hostPath data persistence!
/ # 
```


Now, check minikube node for the hostPath directory. It should have this file there.

```
$ minikube ssh

# ls -l /data/web-server/
total 4
-rw-r--r-- 1 root root 57 Mar 12 12:32 index.html

# cat  /data/web-server/index.html 
Nginx - This is an example of hostPath data persistence!
```

It goes without saying that if you access this web server from another container, you will see the same contents. Lets try our trusted multitool.

```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.5
Nginx - This is an example of hostPath data persistence!
bash-4.4# 
```

Now, we check for persistence. Lets delete the pod, and re-create it. It should be able to mount the same directory from the host and serve the same content.

```
$ kubectl delete pod hostpath-example-pod 
pod "hostpath-example-pod" deleted
```

Check the contents of the file on the hostPath on the host. It should exist.

```
$ minikube ssh

# cat  /data/web-server/index.html 
Nginx - This is an example of hostPath data persistence!
```

Good! Lets re-create the same pod:

```
$ kubectl create -f support-files/hostPath-example-pod.yaml 
pod "hostpath-example-pod" created

$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
hostpath-example-pod         1/1       Running   0          4s        172.17.0.6   minikube
multitool-5558fd48d4-pqkj5   1/1       Running   0          2d        172.17.0.4   minikube
```



Lets examine the container from inside it, and also access the web-server container from another container. We should be able to see the same contents.

```
$ kubectl exec -it hostpath-example-pod /bin/sh

/ # ls -l /usr/share/nginx/html/index.html 
-rw-r--r--    1 root     root            57 Mar 12 12:32 /usr/share/nginx/html/index.html

/ # cat  /usr/share/nginx/html/index.html 
Nginx - This is an example of hostPath data persistence!
```

```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash
bash-4.4# 

bash-4.4# curl 172.17.0.6
Nginx - This is an example of hostPath data persistence!
```

Great! so hostPath works, and our data is persistent!


----------

## Kubernetes storage (PV and PVC):

I have two examples. One is PV on a worker node, using hostpath. The other is NFS based PV.

### Persistent Volume / PV - based on hostPath 

```
$ cat support-files/pv-as-hostpath.yaml 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-hostpath
  labels:
    name: pv-hostpath
spec:
  storageClassName: ""
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/opt/data"
```

Make sure to create this hostPath on the worker node. This example assumes that you have only one worker node. This (hostPath) method is meaningless on multi-node cluster.
```
$ minikube ssh

$ sudo -i

# mkdir /opt/data

# ls -l /opt/data/
total 0
```



We will create PV and PVC manually, to show how it works. 
```
$ kubectl create -f support-files/pv-as-hostpath.yaml 
persistentvolume "pv-hostpath" created
```

Note that a PV is global resource. It cannot be confined within a namespace.
```
$ kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM               STORAGECLASS   REASON    AGE
pv-hostpath   100Mi      RWO            Retain           Bound     default/pvc-nginx                            59s 
```


```
$ kubectl describe pv pv-hostpath
Name:            pv-hostpath
Labels:          name=pv-hostpath
Annotations:     <none>
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    
Status:          Available
Claim:           
Reclaim Policy:  Retain
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        100Mi
Node Affinity:   <none>
Message:         
Source:
    Type:          HostPath (bare host directory volume)
    Path:          /opt/data
    HostPathType:  
Events:            <none>
```


Now, lets create a PVC, which takes a slice of storage out of this PV. That PVC will then be used/consumed by a pod. Note that I have set storageClassName to null, because I want the PVC to *not* use any storage classes. Instead I want it to bind to the PV of my choice by using a *selector*. In this example, it is important to set storageclass to null because if it is simply absent, then by default the **default** (or "standard") storage class is used automatically. (This is not what I want - at the moment)


```
$ cat pvc-nginx-manual.yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nginx
  labels:
    name: pvc-nginx
spec:
  # Don't use any storage-class
  storageClassName: ""
  # Use a pre-existing PV 
  selector:
    matchLabels:
      name: "pv-hostpath"
  accessModes:
    # Though accessmode is already defined in pv definition. It is still needed here.
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Mi

```


```
$ kubectl create -f support-files/pvc-nginx-manual.yaml 
persistentvolumeclaim "pvc-nginx" created
```

**Note:** If you create a PVC and it worked and you deleted it and recreated it, and it gets stuck in `Pending` status, see [this issue](https://github.com/kubernetes/kubernetes/issues/48609). 

Verify that the PVC is created and is boud to the PV. Also note that PVC is namespace dependent. PVCs are not visible across different namespaces.

```
$ kubectl get pvc 
NAME        STATUS    VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound     pv-hostpath   100Mi      RWO                           1m
```

Describe the pvc to learn more about it:

```
$ kubectl describe pvc pvc-nginx 
Name:          pvc-nginx
Namespace:     default
StorageClass:  
Status:        Bound
Volume:        pv-hostpath
Labels:        name=pvc-nginx
Annotations:   pv.kubernetes.io/bind-completed=yes
               pv.kubernetes.io/bound-by-controller=yes
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      100Mi
Access Modes:  RWO
VolumeMode:    Filesystem
Events:        <none>
```


Note that by this time, there is no change in the actual hostPath directory on the worker node.

```
$ minikube ssh

$ sudo -i

# ls -la /opt/data/
total 0
drwxr-xr-x 2 root root 0 Mar 12 13:23 .
drwxr-xr-x 5 root root 0 Mar 12 13:23 ..
```

Lets create a pod, which uses this PVC to store it's data.

```
$ cat nginx-pod-using-pvc.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-using-pvc
spec:
  volumes:
  - name: nginx-htmldir
    persistentVolumeClaim:
      claimName: pvc-nginx
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: nginx-htmldir
```



```
$ kubectl create -f nginx-pod-using-pvc.yaml 
pod "nginx-pod-using-pvc" created

$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
multitool-5558fd48d4-pqkj5   1/1       Running   0          2d        172.17.0.4   minikube
nginx-pod-using-pvc          1/1       Running   0          1m        172.17.0.6   minikube
```


Describe the pod to verify that it has mounted the PVC as a volume:
```
$ kubectl describe pod nginx-pod-using-pvc 
Name:               nginx-pod-using-pvc
. . . 
Containers:
  nginx:
. . . 
    Mounts:
      /usr/share/nginx/html from nginx-htmldir (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9m6nh (ro)
. . . 
Volumes:
  nginx-htmldir:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  pvc-nginx
    ReadOnly:   false
. . . 
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  3m    default-scheduler  Successfully assigned default/nginx-pod-using-pvc to minikube
  Normal  Pulled     3m    kubelet, minikube  Container image "nginx:alpine" already present on machine
  Normal  Created    3m    kubelet, minikube  Created container
  Normal  Started    3m    kubelet, minikube  Started container
```

Let's quickly check if there is something on the hostPath on the worker node. There is nothing yet.

```
$ minikube ssh

$ sudo -i

# ls -la /opt/data/
total 0
drwxr-xr-x 2 root root 0 Mar 12 13:23 .
drwxr-xr-x 5 root root 0 Mar 12 13:23 ..
```

Remember, our volume is empty at this time. So nginx will not have any files under `/usr/share/nginx/html` directory inside the container.


```
$ kubectl exec -it nginx-pod-using-pvc /bin/sh
/ # ls -l /usr/share/nginx/html/
total 0
/ # 
```


Let's create a file in this directory, while we are inside the nginx container.

```
/ # echo "Nginx! webcontent to see PV and PVC in action" > /usr/share/nginx/html/index.html 
```

As soon as you create the file in the container, you will see that this file pop up on the hostPath on the worker node:

```
$ minikube ssh

$ ls -la /opt/data/
total 4
drwxr-xr-x 2 root root  0 Mar 12 14:27 .
drwxr-xr-x 5 root root  0 Mar 12 13:23 ..
-rw-r--r-- 1 root root 46 Mar 12 14:27 index.html
```


Check the nginx pod by accessing it through multitool:
```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.6
Nginx! webcontent to see PV and PVC in action
```


Delete the nginx pod:

```
$ kubectl delete pod nginx-pod-using-pvc
pod "nginx-pod-using-pvc" deleted
```


Verify that the PV and PVC still exist:
```
$ kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM               STORAGECLASS   REASON    AGE
pv-hostpath   100Mi      RWO            Retain           Bound     default/pvc-nginx                            15m

$ kubectl get pvc
NAME        STATUS    VOLUME        CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound     pv-hostpath   100Mi      RWO                           15m
```

Verify that the file still exists on the hostPath on the node:
```
$ minikube ssh

$ ls -la /opt/data/
total 4
drwxr-xr-x 2 root root  0 Mar 12 14:27 .
drwxr-xr-x 5 root root  0 Mar 12 13:23 ..
-rw-r--r-- 1 root root 46 Mar 12 14:27 index.html
```



Create the pod again.
```
$ kubectl create -f nginx-pod-using-pvc.yaml 
pod "nginx-pod-using-pvc" created

$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
multitool-5558fd48d4-pqkj5   1/1       Running   0          2d        172.17.0.4   minikube
nginx-pod-using-pvc          1/1       Running   0          8s        172.17.0.5   minikube
```


Check if the files are there:
```
$ kubectl exec -it nginx-pod-using-pvc /bin/sh

/ # ls -l /usr/share/nginx/html/
total 4
-rw-r--r--    1 root     root            46 Mar 12 14:06 index.html

/ # cat  /usr/share/nginx/html/index.html 
Nginx! webcontent to see PV and PVC in action
/ # 
```

Great! So it works! 

Lets cleanup by removing the PV and PVC we created:

```
$ kubectl delete pvc pvc-nginx

$ kubectl delete pv pv-hostpath
```

------

### Data persistence using NFS:

When you have personal or self-installed kubernetes clusters (minikube / kubeadm, etc), then you don't have the option of persistent disks, such as the ones available in GCP, or AWS, or Azure, etc. In those situations, NFS can be very inexpensive and useful service to have on the network. Your pods can then use the NFS storage for persistent storage. Please note that for pods to be able to mount NFS shares from a NFS server, the worker nodes need to have a nfs client package (`nfs-utils`) present / installed. On minikube it is already installed. 

**Note:** On cloud provisioned clusters, NFS client is already enabled / installed on worker nodes. On kubeadm based systems , or any bare-metal installations, which you setup yourself, you will need to install nfs client utilities (`nfs-utils`) on worker nodes - yourself.

In this example, I have a NFS server running on the physical hosts I am running my (k8s) VMs on. It could actually be running anywhere in the network - even inside the kubernetes cluster - but it is much wiser to run it **outside** the kubernetes cluster. Below is how my NFS server is setup.


Note: Each directory inside `/home/nfsshare` will be considered a **PD** (persistent/physical disk), and will therefore be mapped-to/used-by a **PV** (persistent volume).

```
[root@kworkhorse ~]# mkdir -p /home/nfsshare/nginx-data

[root@kworkhorse ~ ]# cat /etc/exports
/home/nfsshare/nginx-data *(rw,no_root_squash)

[root@kworkhorse ~]# ls -l /home/nfsshare
total 4
drwxr-xr-x 2 root root 4096 Mar 13 08:35 nginx-data
```

```
[root@kworkhorse ~]# systemctl restart nfs

[root@kworkhorse ~]# systemctl status nfs
● nfs-server.service - NFS server and services
   Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; disabled; vendor preset: disabled)
  Drop-In: /run/systemd/generator/nfs-server.service.d
           └─order-with-mounts.conf
   Active: active (exited) since Wed 2019-03-13 07:35:49 CET; 1s ago
  Process: 22464 ExecStopPost=/usr/sbin/exportfs -f (code=exited, status=0/SUCCESS)
  Process: 22463 ExecStopPost=/usr/sbin/exportfs -au (code=exited, status=0/SUCCESS)
  Process: 22462 ExecStop=/usr/sbin/rpc.nfsd 0 (code=exited, status=0/SUCCESS)
  Process: 22485 ExecStart=/bin/sh -c if systemctl -q is-active gssproxy; then systemctl reload gssproxy ; fi (code=exited, status=0/SU>
  Process: 22473 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
  Process: 22472 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
 Main PID: 22485 (code=exited, status=0/SUCCESS)

Mar 13 07:35:49 kworkhorse.oslo.praqma.com systemd[1]: Starting NFS server and services...
Mar 13 07:35:49 kworkhorse.oslo.praqma.com systemd[1]: Started NFS server and services.
[root@kworkhorse ~]#
```

Here is the host side information - for completeness:

```
[root@kworkhorse ~]# virsh net-list 
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
 k8s-kubeadm-net      active     yes           yes
 minikube-net         active     yes           yes


[root@kworkhorse ~]# virsh net-info minikube-net
Name:           minikube-net
UUID:           5235a6ca-b19a-4c52-91d2-8cd0797deedf
Active:         yes
Persistent:     yes
Autostart:      yes
Bridge:         virbr1

[root@kworkhorse ~]# ip addr show | grep virbr1
29: virbr1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    inet 192.168.39.1/24 brd 192.168.39.255 scope global virbr1
[root@kworkhorse ~]# 

```

So now we know that from the VM I can safely reach this fixed IP `192.168.39.1` and I will always reach my physical host, and in-turn the nfs service.

 
Now, we will see how to use this NFS share directly inside an NFS pod for persistent storage - without creating a PV or PVC, etc.


```
$ cat support-files/hostPath-example-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nfs-example-pod
spec:
  containers:
  - image: nginx:alpine
    name: web-server
    volumeMounts:
    - name: web-volume
      mountPath: /usr/share/nginx/html
  volumes:
  - name: web-volume
    nfs:
      # directory location on host
      server: 192.168.39.1
      path: /home/nfsshare/nginx-data
```


Create the pod:
```
$ kubectl create -f nfs-example-pod.yaml 
pod "nfs-example-pod" created


$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
multitool-5558fd48d4-pqkj5   1/1       Running   0          3d        172.17.0.4   minikube
nfs-example-pod              1/1       Running   0          3s        172.17.0.5   minikube
```

Examine the pod to make sure that we have NFS share mounted correctly:

```
$ kubectl describe pod nfs-example-pod
Name:               nfs-example-pod
Status:             Running
IP:                 172.17.0.5
Containers:
  web-server:
    Image:          nginx:alpine
. . . 
    Mounts:
      /usr/share/nginx/html from web-volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-9m6nh (ro)
. . . 
Volumes:
  web-volume:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    192.168.39.1
    Path:      /home/nfsshare/nginx-data
    ReadOnly:  false
. . .
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  1m    default-scheduler  Successfully assigned default/nfs-example-pod to minikube
  Normal  Pulled     1m    kubelet, minikube  Container image "nginx:alpine" already present on machine
  Normal  Created    1m    kubelet, minikube  Created container
  Normal  Started    1m    kubelet, minikube  Started container
```

The mount point should be empty right now, both inside the pod, and also in the NFS share on the physical host.

```
$ kubectl exec -it nfs-example-pod /bin/sh

/ # ls -l /usr/share/nginx/html/
total 0
```

```
[root@kworkhorse ~]# ls -l /home/nfsshare/nginx-data/
total 0
```

Lets create a file inside the pod:

```
$ kubectl exec -it nfs-example-pod /bin/sh


/ # echo "NGINX - NFS direct mount example!" > /usr/share/nginx/html/index.html

/ # cat  /usr/share/nginx/html/index.html 
NGINX - NFS direct mount example!
```

On the NFS server, we see that the file is visible:

```
[root@kworkhorse ~]# ls -l /home/nfsshare/nginx-data/
total 4
-rw-r--r-- 1 root root 34 Mar 13 08:59 index.html
```

Check from our trusted multitool:
```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.5
NGINX - NFS direct mount example!
```

Kill the pod:

```
$ kubectl delete pod nfs-example-pod
pod "nfs-example-pod" deleted

$ kubectl get pods 
NAME                         READY     STATUS    RESTARTS   AGE
multitool-5558fd48d4-pqkj5   1/1       Running   0          3d
```


Re-create the pod so we see that it mounts the same NFS share:
```
$ kubectl create -f nfs-example-pod.yaml 
pod "nfs-example-pod" created

$ kubectl get pods -o wide
NAME                         READY     STATUS    RESTARTS   AGE       IP           NODE
multitool-5558fd48d4-pqkj5   1/1       Running   0          3d        172.17.0.4   minikube
nfs-example-pod              1/1       Running   0          7s        172.17.0.5   minikube
```


Checking from multitool should be enough. If it works, we know that the pod has mounted the NFS share correctly!

```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.5
NGINX - NFS direct mount example!
```


It works!

So we have see how we can use NFS share directly inside the pod and have data persistence! But, you have noticed that this can become very complicated if every pod needs to have the name of the NFS server and the share name, and if something changes at NFS level, then everything dependent on this information will need to be changed. So now, we will see how can we create PV and PVC out of this NFS share and use it inside the pod.


-------------- 



### PV and PVC using NFS:


First, setup a new directory on NFS server for this PV.

```
[root@kworkhorse ~]# mkdir /home/nfsshare/pv-nfs

[root@kworkhorse ~]# cat /etc/exports
/home/nfsshare/nginx-data *(rw,no_root_squash)
/home/nfsshare/pv-nfs *(rw,no_root_squash)

[root@kworkhorse ~]# systemctl restart nfs
```



```
$ cat pv-as-nfs.yaml 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nfs
  labels:
    name: pv-nfs
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: ""
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /home/nfsshare/pv-nfs
    server: 192.168.39.1
```


```
$ kubectl create -f support-files/pv-as-nfs.yaml 
persistentvolume/pv-nfs created


$ kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM               STORAGECLASS   REASON   AGE
pv-nfs        100Mi      RWO            Recycle          Available                                               8s


$ kubectl describe pv pv-nfs
Name:            pv-nfs
Labels:          name=pv-nfs
Annotations:     <none>
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    
Status:          Available
Claim:           
Reclaim Policy:  Recycle
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        100Mi
Node Affinity:   <none>
Message:         
Source:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    192.168.39.1
    Path:      /home/nfsshare/pv-nfs
    ReadOnly:  false
Events:        <none>
```

```
$ cat pvc-nginx-nfs.yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nginx
  labels:
    name: pvc-nginx
spec:
  storageClassName: ""
  selector:
    matchLabels:
      name: "pv-nfs"
  accessModes:
    # Though accessmode is already defined in pv definition. It is still needed here.
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Mi
```



```
$ kubectl create -f pvc-nginx-nfs.yaml 
persistentvolumeclaim/pvc-nginx created

$ kubectl get pvc
NAME        STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound    pv-nfs   100Mi      RWO                           2s



$ kubectl describe pvc pvc-nginx
Name:          pvc-nginx
Namespace:     default
StorageClass:  
Status:        Bound
Volume:        pv-nfs
Labels:        name=pvc-nginx
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      100Mi
Access Modes:  RWO
VolumeMode:    Filesystem
Events:        <none>
Mounted By:    <none>

```



Lets use this PVC in a pod. We already have a pod description as yaml file, which uses `pvc-nginx` as a volume. Lets use that.

First, here is the file:

```
$ cat nginx-pod-using-pvc.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-using-pvc
spec:
  volumes:
  - name: nginx-htmldir
    persistentVolumeClaim:
      claimName: pvc-nginx
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: nginx-htmldir
```

```
$ kubectl create -f nginx-pod-using-pvc.yaml 
pod/nginx-pod-using-pvc created

$ kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
multitool-5558fd48d4-pqkj5   1/1     Running   1          10d
nginx-pod-using-pvc          1/1     Running   0          17s
```


Since this is a fresh volume, nginx cannot serve any content from the empty volume. Lets exec into the nginx pod and create some web content under `/usr/share/nginx/html` directory. 

```
$ kubectl exec -it nginx-pod-using-pvc /bin/sh

/ # echo "Hello world! This content resides on NFS share!" > /usr/share/nginx/html/index.html 

/ # ls -l /usr/share/nginx/html/
total 4
-rw-r--r--    1 root     root            48 Mar 20 15:10 index.html
/ # 
```

Lets access this nginx instance from multitool:

```
$ kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
multitool-5558fd48d4-pqkj5   1/1     Running   1          10d     172.17.0.4   minikube   <none>           <none>
nginx-pod-using-pvc          1/1     Running   0          6m52s   172.17.0.5   minikube   <none>           <none>

$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.5
Hello world! This content resides on NFS share!
bash-4.4# 
```

This file exists in my NFS share on the NFS server:

```
[root@kworkhorse ~]# ls -l /home/nfsshare/pv-nfs/
total 4
-rw-r--r-- 1 root root 48 Mar 20 16:10 index.html
```

Lets kill the pod and see if it can re-mount the same volume when the pod is re-created:

```
$ kubectl delete pod nginx-pod-using-pvc
pod "nginx-pod-using-pvc" deleted

$ kubectl create -f nginx-pod-using-pvc.yaml 
pod/nginx-pod-using-pvc created

$ kubectl get pods -o wide
NAME                         READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
multitool-5558fd48d4-pqkj5   1/1     Running   1          10d   172.17.0.4   minikube   <none>           <none>
nginx-pod-using-pvc          1/1     Running   0          30s   172.17.0.6   minikube   <none>           <none>
```

Lets see if it has content!
```
$ kubectl exec -it nginx-pod-using-pvc /bin/sh
/ # ls -l /usr/share/nginx/html
total 4
-rw-r--r--    1 root     root            48 Mar 20 15:10 index.html
```

Exec into multitool and access it from there:
```
$ kubectl exec -it multitool-5558fd48d4-pqkj5 bash

bash-4.4# curl 172.17.0.6
Hello world! This content resides on NFS share!
bash-4.4# 
```

It works!




-------------------

## Automatic / Dynamic provisionsing:
A **storage class** has actual physical storage underneath it - provided/managed by the cloud provider; though, the storage class hides this information, and provides an abstraction layer for you. Normally, all cloud providers have a default storage class created for you, ready to be used.

**Note:** Minikube provides a `standard` storageclass of type `hostPath` out of the box. Kubeadm based clusters do not. 

When you want to provide some persistent storage to your pods, you define/create a persistent volume claim (PVC), which the pod consumes by the mounting this PVC at a certain mount point in the file system. As soon as the PVC is created (using dynamic provisioning), a corresponding persistent volume (PV) is created. The PV in-turn takes a slice of storage from the storage class, and as soon as it acquires this storage slice, it binds to the PVC. 


Lets check what classes are available to us:

```shell
$ kubectl get sc

NAME                 PROVISIONER		AGE
standard (default)   kubernetes.io/gce-pd	1d
```
Good, so we have a storage class named `standard` , which we can use to create PVCs and PVs.


Here is what storageclasses looks like on minikube - just for completeness sake: 
```
$ kubectl get storageclass
NAME                 PROVISIONER                AGE
standard (default)   k8s.io/minikube-hostpath   19d
```

**Note:** It does not matter if you are using GCP, AWS, Azure, minikube, kubeadm, etc. Till the time you have a name for the storage class - which you can use in your PVC claims, you have no concern about the underlying physical storage. So, if you have a pvc claim definition file,(like the one below), and you are using it to create PVC on a **minikube** cluster, you can simply use the same PVC claim file to create a PVC with the same name on a **GCP** cluster. The abstraction allows you to *develop anywhere, deploy anywhere* !


If you do not have a storage class, you can create/define it yourself. All you need to know is type of provisioner to use. Here is a definition file to create a storage class named **slow** on a GCP k8s cluster:

```
$ cat support-files/gcp-storage-class.yaml 

kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: slow
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-standard
  replication-type: none
```

```
$ kubectl create -f support-files/gcp-storage-class.yaml
storageclass "slow" created
```

```
$ kubectl get sc

NAME                 PROVISIONER		AGE
standard (default)   kubernetes.io/gce-pd	1d
slow		     kubernetes.io/gce-pd	1h
```

## Create and use a PVC:

Lets create a claim for a dynamically provisioned volume (PVC) for nginx. Here is the file to create a PVC named `pvc-nginx`:
```shell
$ cat support-files/pvc-nginx.yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nginx
  labels:
    name: pvc-nginx
spec:
  storageClassName: "standard"
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
```

```shell
$ kubectl create -f support-files/pvc-nginx.yaml
```

Check that the PVC exists and is bound:

```shell
$ kubectl get pvc
NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound     pvc-e8a4fc89-2bae-11e8-b065-42010a8400e3   100Mi      RWO            standard       4m
```

There should be a corresponding auto-created persistent volume (PV) against this PVC:

```shell
$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM               STORAGECLASS   REASON    AGE
pvc-e8a4fc89-2bae-11e8-b065-42010a8400e3   100Mi      RWO            Delete           Bound     default/pvc-nginx   standard                 5m
```
**Note:** Above may be confusing, and demands some explanation. The PVC has a name `pvc-nginx` , but since the PV is being created automatically, kubernetes cannot guess it's name, so it gives it a name which begins with **pvc** and gives it an ID. This ID is reflected in the **VOLUME**  column of the `kubectl get pvc` command.


Next, we are going to create a deployment, and it's pod will use this storage. Here is the file `support-files/nginx-persistent-storage.yaml` , which shows how it is defined in yaml format:

```
$ cat support-files/nginx-persistent-storage.yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: nginx-htmldir-volume
        persistentVolumeClaim:
          claimName: pvc-nginx
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 443
        - containerPort: 80
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: nginx-htmldir-volume
```

Create the deployment: 

```shell
$ kubectl create -f support-files/nginx-persistent-storage.yaml
```

After the deployment is created and the pod starts, you should examine it by using `kubectl describe pod nginx`, and look out for volume declarations.

Optionally, create a service (of type ClusterIP) out of this deployment.

Now you access the Nginx pod/service using curl from the multitool pod. You should get a "403 Forbidden". This is because PVC volume you mounted, is empty!

```shell
$ kubectl exec -it multitool-<ID> bash

bash-4.4# curl 10.0.96.7
<html>
<head><title>403 Forbidden</title></head>
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.9.1</center>
</body>
</html>
```

Exit the multitool pod and *exec* into the nginx pod.

Create a file in the html dirrectory, and add some text in it:

```shell
$ kubectl exec -it nginx-deployment-6665c87fd8-cc8k9 -- bash

root@nginx-deployment-6665c87fd8-cc8k9:/# echo "<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage."  > /usr/share/nginx/html/index.html
```

Exit the nginx pod. *exec* into the multitool container, again and run curl - again. This time, you should see the web page:

```shell
$ kubectl exec -it multitool-69d6b7fc59-gbghn bash

bash-4.4#
 curl 10.0.96.7
<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage.
bash-4.4#
```

Ok. Lets kill this nginx pod:

```shell
$ kubectl delete pod nginx-deployment-6665c87fd8-cc8k9
pod "nginx-deployment-6665c87fd8-cc8k9" deleted
```

Since the nginx pod was part of the deployment, it will be re-created, and the PVC volume will be mounted. 

Verify that the pod is up (notice a new pod id):

```shell
$ kubectl get pods -o wide
NAME                                READY     STATUS    RESTARTS   AGE       IP          NODE
multitool-69d6b7fc59-gbghn          1/1       Running   0          10m       10.0.97.8   gke-dcn-cluster-2-default-pool-4955357e-txm7
nginx-deployment-6665c87fd8-nh7bs   1/1       Running   0          1m        10.0.96.8   gke-dcn-cluster-2-default-pool-4955357e-8rnp
```

Again, from multitool, curl the new nginx pod. You should see the page you created in previous step - **not** `403 Forbidden`:

```shell
$ kubectl exec -it multitool-69d6b7fc59-gbghn bash

bash-4.4# curl 10.0.96.8
<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage.
bash-4.4# 
```

This proved that the volume behind the PVC retained the data even though the pod which used it was deleted.

**Note:** Since the goal of dynamic provisioning is to completely automate the lifecycle of storage resources, the default reclaim policy for dynamically provisioned volumes is **delete**. This means that when a PersistentVolumeClaim (PVC) is released, the dynamically provisioned volume is de-provisioned (deleted) on the storage provider and the data is likely irretrievable. If this is not the desired behavior, the user must change the reclaim policy on the corresponding PersistentVolume (PV) object after the volume is provisioned. You can change the reclaim policy by editing the PV object and changing the “persistentVolumeReclaimPolicy” field to the desired value.

## Clean up

```shell
$ kubectl delete deployment multitool
$ kubectl delete deployment nginx-deployment
$ kubectl delete service nginx-deployment
$ kubectl delete pvc pvc-nginx
```


# Storage

This exercise shows how to provide persistent storage to your pods, so your data is safe across pod creation cycles. You will learn ho to create **PVs** and **PVCs** and how to use **storage classes**.

## Automatic / Dynamic provisionsing:
A **storage class** has actual physical storage underneath it - provided/managed by the cloud provider; though, the storage class hides this information, and provides an abstraction layer for you. Normally, all cloud providers have a default storage class created for you, ready to be used.

**Note:** Minikube provides a `standard` storageclass of type `hostPath` out of the box. Kubeadm based clusters do not. 

When you want to provide some persistent storage to your pods, you define/create a persistent volume claim (PVC), which the pod consumes by the mounting this PVC at a certain mount point in the file system. As soon as the PVC is created (using dynamic provisioning), a corresponding persistent volume (PV) is created. The PV in-turn takes a slice of storage from the storage class, and as soon as it acquires this storage slice, it binds to the PVC. 


Lets check what classes are available to us:

```shell
$ kubectl get sc

NAME                 PROVISIONER		AGE
standard (default)   kubernetes.io/gce-pd	1d
```
Good, so we have a storage class named `standard` , which we can use to create PVCs and PVs.


Here is what storageclasses looks like on minikube - just for completeness sake: 
```
$ kubectl get storageclass
NAME                 PROVISIONER                AGE
standard (default)   k8s.io/minikube-hostpath   19d
```

**Note:** It does not matter if you are using GCP, AWS, Azure, minikube, kubeadm, etc. Till the time you have a name for the storage class - which you can use in your PVC claims, you have no concern about the underlying physical storage. So, if you have a pvc claim definition file,(like the one below), and you are using it to create PVC on a **minikube** cluster, you can simply use the same PVC claim file to create a PVC with the same name on a **GCP** cluster. The abstraction allows you to *develop anywhere, deploy anywhere* !


If you do not have a storage class, you can create/define it yourself. All you need to know is type of provisioner to use. Here is a definition file to create a storage class named **slow** on a GCP k8s cluster:

```
$ cat support-files/gcp-storage-class.yaml 

kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: slow
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-standard
  replication-type: none
```

```
$ kubectl create -f support-files/gcp-storage-class.yaml
storageclass "slow" created
```

```
$ kubectl get sc

NAME                 PROVISIONER		AGE
standard (default)   kubernetes.io/gce-pd	1d
slow		     kubernetes.io/gce-pd	1h
```

## Create and use a PVC:

Lets create a claim for a dynamically provisioned volume (PVC) for nginx. Here is the file to create a PVC named `pvc-nginx`:
```shell
$ cat support-files/pvc-nginx.yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nginx
  labels:
    name: pvc-nginx
spec:
  storageClassName: "standard"
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
```

```shell
$ kubectl create -f support-files/pvc-nginx.yaml
```

Check that the PVC exists and is bound:

```shell
$ kubectl get pvc
NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound     pvc-e8a4fc89-2bae-11e8-b065-42010a8400e3   100Mi      RWO            standard       4m
```

There should be a corresponding auto-created persistent volume (PV) against this PVC:

```shell
$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM               STORAGECLASS   REASON    AGE
pvc-e8a4fc89-2bae-11e8-b065-42010a8400e3   100Mi      RWO            Delete           Bound     default/pvc-nginx   standard                 5m
```
**Note:** Above may be confusing, and demands some explanation. The PVC has a name `pvc-nginx` , but since the PV is being created automatically, kubernetes cannot guess it's name, so it gives it a name which begins with **pvc** and gives it an ID. This ID is reflected in the **VOLUME**  column of the `kubectl get pvc` command.


Next, we are going to create a deployment, and it's pod will use this storage. Here is the file `support-files/nginx-persistent-storage.yaml` , which shows how it is defined in yaml format:

```
$ cat support-files/nginx-persistent-storage.yaml

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: nginx-htmldir-volume
        persistentVolumeClaim:
          claimName: pvc-nginx
      containers:
      - name: nginx
        image: nginx:1.9.1
        ports:
        - containerPort: 443
        - containerPort: 80
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: nginx-htmldir-volume
```

Create the deployment: 

```shell
$ kubectl create -f support-files/nginx-persistent-storage.yaml
```

After the deployment is created and the pod starts, you should examine it by using `kubectl describe pod nginx`, and look out for volume declarations.

Optionally, create a service (of type ClusterIP) out of this deployment.

Run a multitool deployment if you don't have it running - to help you access nginx container:
```
$ kubectl run multitool --image=praqma/network-multitool 
```

Now you access the Nginx pod/service using curl from the multitool pod. You should get a "403 Forbidden". This is because PVC volume you mounted, is empty!

```shell
$ kubectl exec -it multitool-<ID> bash

bash-4.4# curl 10.0.96.7
<html>
<head><title>403 Forbidden</title></head>
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.9.1</center>
</body>
</html>
```

Exit the multitool pod and *exec* into the nginx pod.

Create an `index.html` file in the `/usr/share/nginx/html/` dirrectory, and add some text in it:

```shell
$ kubectl exec -it nginx-deployment-6665c87fd8-cc8k9 -- bash

root@nginx-deployment-6665c87fd8-cc8k9:/# echo "<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage."  > /usr/share/nginx/html/index.html
```

Exit the nginx pod. *exec* into the multitool container, again and run curl - again. This time, you should see the web page:

```shell
$ kubectl exec -it multitool-69d6b7fc59-gbghn bash

bash-4.4#
 curl 10.0.96.7
<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage.
bash-4.4#
```

Ok. Lets kill this nginx pod:

```shell
$ kubectl delete pod nginx-deployment-6665c87fd8-cc8k9
pod "nginx-deployment-6665c87fd8-cc8k9" deleted
```

Since the nginx pod was part of the deployment, it will be re-created, and the PVC volume will be mounted. 

Verify that the pod is up (notice a new pod id):

```shell
$ kubectl get pods -o wide
NAME                                READY     STATUS    RESTARTS   AGE       IP          NODE
multitool-69d6b7fc59-gbghn          1/1       Running   0          10m       10.0.97.8   gke-dcn-cluster-2-default-pool-4955357e-txm7
nginx-deployment-6665c87fd8-nh7bs   1/1       Running   0          1m        10.0.96.8   gke-dcn-cluster-2-default-pool-4955357e-8rnp
```

Again, from multitool, curl the new nginx pod. You should see the page you created in previous step - **not** `403 Forbidden`:

```shell
$ kubectl exec -it multitool-69d6b7fc59-gbghn bash

bash-4.4# curl 10.0.96.8
<h1>Welcome to Nginx</h1>This is Nginx with html directory mounted as a volume from GCE Storage.
bash-4.4# 
```

This proved that the volume behind the PVC retained the data even though the pod which used it was deleted.

**Note:** Since the goal of dynamic provisioning is to completely automate the lifecycle of storage resources, the default reclaim policy for dynamically provisioned volumes is **delete**. This means that when a PersistentVolumeClaim (PVC) is released, the dynamically provisioned volume is de-provisioned (deleted) on the storage provider and the data is likely irretrievable. If this is not the desired behavior, the user must change the reclaim policy on the corresponding PersistentVolume (PV) object after the volume is provisioned. You can change the reclaim policy by editing the PV object and changing the “persistentVolumeReclaimPolicy” field to the desired value.

## Clean up

```shell
$ kubectl delete deployment multitool
$ kubectl delete deployment nginx-deployment
$ kubectl delete service nginx-deployment
$ kubectl delete pvc pvc-nginx
```




==============================================================================================================================================================
