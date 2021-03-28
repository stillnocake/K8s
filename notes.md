
# Kubernetes

K8s : Orchestrator of microservices apps (application that's made up of lots of small and independant services that work together to create usefull application)



- Stuff that runs on the masters are called 'the cluster control plane' (also known as head nodes) (monitors it, make the changes, schedules the work, reponds to events)
- Nodes are where we run our user or our business applications (report back to the masters, watch for new instructions)

- Aim for multi master control planes for maximum availability (3 is the magic number) the more there are, the more time is needed to reach consensus (only one master is elected to make changes to the custer)

## Masters buzzwords:

- Cluster store: The only persistent component of the entire control plane where the config and the state of the cluster itself as well as any apps running in it gets stored. The containers running in it **share** the environment. Based on etcd dictionnary(key/value store) for configurations
- Kube-controller-manager: Controller of controllers; Theres a controller for everything in the cluster
	- Node Controller: Responsible for noticing and responding when nodes go down.
	- Replication Controller: Responsible for maintaining the correct number of pods for every replication controller objects in the system.
	- Endpoint Controller: Populates the endpoint object (that is, joins Service & Pods),
	- Service Acount & Token Controllers: Create default accounts and API access tokens for new namespaces.
- Kube scheduler: Watches API  Server (kubectl) for new work tasks, Assigns work to cluster nodes; In other words, this is the mecanism responsible for allocating pods to available nodes. The scheduler is responsible for workload utilization and allocating pod to new node.
- Kube-apiserver: Designed to scale horizontally -- thats is, it scaled by deploying more instances. You can run several instances of kube-apiserver and balance traffic between those instances.
- Kubeconfig: Package along with the server side tools that can be used for communication. It exposes K8s API so that others can reach it.
- Cloud-controller-manager: Runs controllers that interact with the underlying cloud providers. Cloud-controller-manager allows the cloud vendor's code andd the K8s code to evolve independently of each other.
	- Node controller: For checking the cloud provider to determine if a ode has been deleted in the cloud after it stops responding;
	- Route controller: For setting up routes in the underlying cloud infrastructure;
	- Service controller: For creating, updating and deleting cloud provider load balancers;
	- Volume controller: For creating, attaching and mounting volumes by interacting with the cloud providers to orchestrate volumes

**Overview**: 	Commands and queries come into the api server by usually using 'kubectl' command line tool, they get autenticated and authorized, the desired state gets written to the cluster store as a record of intent, and the scheduler farms the work out to nodes in the cluster. 

Once thats done, various controllers sits in watch loops observing the state of the cluster and making sure that it matches what we've asked for.

## Nodes buzzwords:

- Container runtime: Software responsible for running containers. Kubernetes supports several container runtimes: Docker, containerd, cri-o, rktlet and any implementation of the Kubernetes CRI (Container Runtime Interface)
- Kubelet Service: 
		- Main k8s agent that runs on every cluster nodes;  
		- Registers node with cluster; 
		- Watch the API server on the master for any new pods assigned to it, when it sees one it pulls the spec and runs the pod (execute pods)
		- Reports back to masters
		- Small service in each node responsible for relaying information to and from control plane services. It interacts with etcd store to read configuration details and right values;
		- Communicates with the master component to receive commands and work. The kubelet process the takes responsability for maintaining the state of work and the node server. It manages network rules, port forwarding, etc.
- Kubernetes Proxy Service: Runs on each node and helps in making services available to the extternal host. Itt helps in forwading the rrequests to correct containers and is capable of performing primitive load balancing. I makes sure the the networking environment is predictable and accessible and, at the same time, it is isolated. It manages pods oon nodes, volumes, secrets, creating new container's health checkup, etc.

## Pods:
A thin wrapper that K8s every container needs. A shared execution environment (a collection of things that an app needs in order to run).
- One of more containers packaged together as a single deployable unit
- Container for Docker, Pod for K8s
- Mapped using ports because they use the same IP

## Add-ons:
Add-ons use Kubernetes resources (DaemonSet, Deployment, etc.) to implement cluster features. The provide cluster-level deatures and namespaced resources for add-ons that belong withing the kube-system namespace
- DNS: Cluster DNS is a DNS server, in addition to the other DNS server(s) in our environement, which serves DNS records for K8S services. Containers started by K8s automatically include this DNS server in their DNS searches
- WEB UI(Dashboard) : Dashboard is a general-purpose, web-based UI for Kubernetes clusters, It allows users to manage and troubleshoot applications running in the clustter, as well as the cluster itself
- Container resource monitoring: Recourds generic time series metrics about containers in a central database and provides a UI for browsing that data
- Cluster-level Logging: A cluster-levvel logging mechanism is responsible for saving container logs to a centtral log store with a search/browsing interface
- ** kubectl **: The kubectl command-line tool interacts with kube-api-server and send commands to the master node. Heres, each command is converted into and API call

## Kube-Proxy:

- Network brains of the node, it makes sure that every pod gets its own unique IP (one IP per pod); 
- Plays a major role in load balancing traffic.

## Virtual kubelet:
- Nodeless k8s (from cloud providers)

## Service object:
- A service acting as a loadd balancer that updates its IP list when hcnage happen to pods network configuration like IPs
- Never changes the stable and reliable IP and DNS name associated to it
- The way that a Pod belongs to a service or makes it onto the list of pods is via **labels**
- Services only sends trafic to healthy pods
- Can do session affinity
- Can send traffic to endpoints outside the cluster (using labels)
- Can do TCP and UDP

## Deployments:
- Deployment controller / Reconciliation loop
	- Watches API server for new deployments and implements them
	- Constantly compares observed state with desired state
## Declarative model:
- The declarative model and the concept of desired state: 
	- Describe what you want in a manifest file
	- Manifest is posted to the API Server

## K8s API and API Server
- API: Catalog of features or services with a definition of how each one works
- API Server: control plane feature that exposes the API over a secure RESTful endpoint (it's just the way that we reach and communicate with the API)

## Commands
kubectl get pods --all-namespaces
kubectl create - f nameoffile.yaml
kubectl get pods
kubectl describe pods

## Other thoughts:
- K8s doesn't rest until it has obtained the desired state (desired state vs observed state)
