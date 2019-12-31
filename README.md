Test001
This is test repo

#This is just an test Part

![] (Test001/Test1/)


==============================================================================================================================================================

==============================================================================================================================================================

# Below are some of the list of problem if we are using only Docker.

. Multiple application stacks

. Large deployments

. Deployment / Updating / Scheduling

. Replication / Availability

Resilience
Storage
Ingress
Benefit of using Kubernetes
Availability - scale as defined in your desired state
Resilience - if a container exits/dies, a new one is created
Storage - Local, NFS, iSCSI, GCEP, AWS EBS and more
Deployments - with Canary pattern
Scheduling - with Resource Limitations
Updates - with Rolling Updates
Networking and Cluster DNS
Service Discovery
Ingress
Below Services runs on Master
API Server
Control Manager
Scheduler
Etcd Cluster - (Is an advance and fast type of database. This keep the desire state information)
Kube-proxy
Kubelet (a.k.s. agent)

Docker
All component communicate with only API Sever

Also onlyt API Server can communicate to Etcd Cluster

Also the user or administrator communicate to API Server

Pod : -
. Is a collection of containers. Which live togeather and die togeather

. The process which manage the Pod is Kubelet.

. Kube-Proxy manage the Manage the Network communication between all the Pod

. Pod is an worker unit.

. Each Pod get an IP address

. Visialize Pod as an Virtual Machine.

Pod Workflow :-
User tell Kubernetes to run a service (e.g. Tomcat)

Kubernetes goes to Kublet

Kublet communicate to Docker and request to run an container with an Image of "Pause" (Pause is an name of Google Image which is empty, this is given by Kublet to Dockr. Pause just takes the IP from the Docker an wait. )

Kublet also instruct the Docker to give name to this Image "Tomcat"(As we had requested Tomcat Image) and return the IP of the container)

Docker give's the the IP address and Name of the Container to Kublet

After that Kublet gives actual Image of the application (e.g. Tomcat Service) and Instruct Docker to run this application inside the Name Space which it has recently created with the same Ip (In our case it is Tomcat)

Now we have two container (Pause and Tomcat Service) in the same IP and Namespace (i.e. Tomcat) (This is how Pod is created. It now contain multiple container)

After that again Kublet request Docker to run one more image of "Proxy" in the same IP and NameSpace (i.e. Tomcat). Proxy Container is used to Proxy the request.

Now we have 3 imges (Pause, Tomcat Service and Proxy)in the same NameSpace and IPAddress

After that again Kublet request Docker to run one more image of "init Container" in the same IP and NameSpace (i.e. Tomcat). Init Container is fist container which get started when we start a Pod, becasue it contain all the initial files and instruction (like Jar file, etc).

Now we have 4 imges (Pause, Tomcat Service, Proxy and Init)in the same NameSpace and IPAddress

Once you run command "Kubectl -p" it will should you the Tomcat 3/3. You will not see the Pause Container whene you run in Kublet beacuse itn't important. But if you log-In to the Docker and run the command then you will see all the 4 Container

Any one container out of 4 die's the whole Container die's

==============================================================================================================================================================
