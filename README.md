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

