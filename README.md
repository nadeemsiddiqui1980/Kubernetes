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

