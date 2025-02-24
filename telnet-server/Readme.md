### CONTAINERIZING AN APPLICATION WITH DOCKER

A container is the running instance of an application based off a container image. Using containers provides you with a predictable and isolated way to create and run code. It allows you to package an application and its dependencies into a portable artifact you can easily distribute and run.
The container layer is created when the container starts, and all changes (such as writing, deleting, and altering existing files) will occur in this writable layer. 
Additionally, a few boundaries and restricted views known as namespaces and cgroups isolate the container from the rest of the Linux host. These are kernel characteristics that restrict what a container on a host can see and utilise. They also make OS-level virtualization a reality.
The main point to remember is that namespaces limit what a container can see, while cgroups limit what it can use.

#### Dockerfile
The Dockerfile contains the instructions that teach the Docker server how to turn an application into a container image.

The most popular instructions are shown below:
FROM: It must be the first command in the file and specifies the parent or base image from which the new image is to be built.
COPY: adds files to a location in the image filesystem from your current directory, which houses the Dockerfile.
RUN: runs a command within the picture.
ADD: copies new directories or files to a destination in the image filesystem from a source or a URL.
ENTRYPOINT: The entry point enables your container to function as an executable, which is equivalent to any Linux.
CMD: command-line program that accepts host arguments.
CMD gives the container a default command or set of parameters (it can be used with ENTRYPOINT).

To build and run the docker image created for this project, run the following:
To build from dockerfile
==> docker build -t devops-vm/telnet-server:v1 .
==> docker run -p 2323:2323 -d --name telnet-server devops-vm/telnet-server:v1
To list existing image
==> docker image ls
To list running and non-running container
==> docker ps
==> docker ps -a
To stop a container
==> docker container stop <container-name>
To run a command inside a container or interact with a container, as if you were logged in to a terminal session.
==> docker exec <container-name> <command>
To remove a stopped container
==> docker rm <container-name>
To remove a docker image
==> docker rmi <container-name>
==> To get low-level information about docker object from a container in json format
==> docker inspect <container-name>
To get the history record of a docker image
==> docker history <image-name or repository-name>
To get a real-time update on the resources a container is using (like top in linux):
==> docker stats --no-stream <container-name>
To see all the logs in a container
==> docker logs <container-name>


### Orchestrating with Kubernetes
Kubernetes is the standard in container orchestration. One or more worker nodes and one or more control plane nodes make up a Kubernetes cluster. A Raspberry Pi, bare-metal racked server, or cloud virtual machine can all be considered nodes. The cluster status, container scheduling, and Kubernetes API requests are all managed by the control plane nodes. The control plane is where the main services (such the scheduler,etcd, and API) operate. The worker nodes execute the resources and containers that the control plane schedules.

Containers are able to communicate with one another both inside and outside the cluster . This occurs while operating a web server that is visible to the public or when microservices are used for internal communication.

A worker node can be configured for a particular use case, such as high connections, and rules can be made to guarantee that the apps that require that functionality are installed on that worker node. This is refer to nodeAffinity.

#### Kubernetes Workload Resources
An object type that contains state and intent is called a resource. Kubernetes resources are defined in a file called a manifest
Pods: They are smallest building elements of Kubernetes, and serve as the basis for all the exciting container-related endeavours. Each container can connect to the other containers, and all containers can share a directory between them by a mounted volume.

ReplicaSet: A ReplicaSet resource is used to maintain a fixed number of identical Pods. If a Pod is killed or deleted, the ReplicaSet will create another Pod to take its place.

Deployments: A Deployment is a resource that manages Pods and ReplicaSets. A Deployment’s main job is to maintain the state that is configured in its manifest. It controls a Pod’s lifecycle—from creation, to updates, to scaling, deletion. You can also roll back to earlier versions of a Deployment if
needed.

StatefulSets: A StatefulSet is a resource for managing stateful applications, such as PostgreSQL, ElasticSearch, and etcd. Similar to a Deployment, it can manage the state of Pods defined in a manifest. Each Pod in a StatefulSet has its own state and data bound to it. If you are adding a stateful application to your cluster, choose a StatefulSet over a Deployment.

Services: Services allow you to expose applications running in a Pod or group of Pods within the Kubernetes cluster or over the internet.
service types include:
1. ClusterIP:  This is the default type when you create a Service. It is assigned an internal routable IP address that proxies connections to one or more Pods. You can access a ClusterIP only from within the Kubernetes cluster.
2. Headless  This does not create a single-service IP address. It is not load balanced.
3. NodePort  This exposes the Service on the node’s IP addresses and port.
4. LoadBalancer  This exposes the Service externally. It does this either by using a cloud provider’s component, like AWS’s Elastic Load Balancing (ELB), or a bare-metal solution, like MetalLB.
5. ExternalName  This maps a Service to the contents of the externalName field to a CNAME record with its value.

Volumes: A Volume is basically a directory, or a file, that all containers in a Pod can access, with some caveats. Volumes provide a way for containers to share and store data between them. If a container in a Pod is killed, the Volume and its data will survive; if the entire Pod is killed, the Volume and its contents will be removed. Thus, if you need storage that is not linked to a Pod’s lifecycle, use a Persistent Volume (PV) for your application. A PV is a resource in a cluster just like a node. Pods can use the PV resource, but the PV does
not terminate when the Pod does. If your Kubernetes cluster is running in AWS, you can use Amazon Elastic Block Storage (Amazon EBS) as your PV. This makes Pod catastrophes easier to survive.

Secrets: Secrets are convenient resources for safely and reliably sharing sensitive information (such as passwords, tokens, SSH keys, and API keys) with Pods. You can access Secrets either via environment variables or as a Volume
mount inside a Pod.

ConfigMaps: Mounting nonsensitive configuration files inside a container is made possible using ConfigMaps. The ConfigMap can be accessed by a pod's containers as a file in a Volume mount, an environment variable, or command line arguments. There are two primary advantages to including any configuration files your application may have in a ConfigMap manifest. First, you don't need to relaunch your entire application in order to update or publish a new manifest file. Second, your application will be able to reload the configuration without requiring a restart if it is programmatically designed to monitor changes in a configuration file.

Namespaces: A Kubernetes cluster can be split up into multiple smaller virtual clusters using the Namespace resource. Even if the resources may be located on the same nodes, a Namespace gives them a logical distinction. A resource will inherit the witty default Namespace if you don't provide a Namespace when establishing it.

Use Minikube's Docker Daemon
To make sure your locally built image is accessible, build the image inside Minikube's Docker daemon:
==> eval $(minikube docker-env)
==> docker build -t devops-vm/telnet-server:v1 .

Scaling a Pod (To scale the deployment to 3)
==> kubectl scale --current-replicas=2 --replicas=3 deployment/telnet-server


### Deploying Code with CI/CD Pipelines
CI and CD are software development methodologies that describe the way code is built, tested, and delivered usually via a version control system. The CI steps cover the testing and building of code and configuration changes, while the CD steps automate the deployment (or delivery) of new code.
When source code changes occur, CI/CD pipelines are typically set up to detect them and initiate a sequence of actions to deploy the changes to a development and or production environment on a virtual machine (VM) or a Kubernetes cluster.

The testing procedures typically include security scans, unit tests, and integration tests to ensure that the application operates as intended, whether in isolation or in conjunction with other components in the stack. Security scans typically look for known vulnerabilities in the application's software dependencies or for vulnerable base container images that it is importing. Following the testing procedures, the new artefact(container image) is created and uploaded to a shared repository so that the CD stage can access it.
During the CD step, artifact(container image) is extracted from a repository and subsequently deployed, typically to production or development infrastructure. CDs can use different strategies to release code. These strategies are usually either 
1. Canary(rolls out new code so only a small subset of users can access it), 
2. Rolling(deploys new codes one by one, alongside the current code in production, until it is fully released.), or 
3. Blue-Green(a production service (blue) takes traffic while the new service (green) is tested. If the green code is operating as expected, the green service will replace the blue service, and all customer requests will funnel through it).

Following a successful deployment, the new code needs to be observed in a monitoring step to ensure that nothing has escaped the CI process.
#### Setting Up My Pipeline
For creating pipeline for this project, i will be using two tools:
i. Skaffold: For continuous development for Kubernetes native applications. (https://skaffold.dev/docs/install/)
ii. Container-structure-test: This command-line tool verifies the structure of the container image after it has been created. It can verify whether a particular file exists or run a command and check the results to see if the image was built correctly. It can also be used to confirm that the ports and environment variables that was provide in a Dockerfile were included in the container image. (https://github.com/GoogleContainerTools/container-structure-test/)