# Kubernetes

K8s : Orchestrator of microservices apps (application that's made up of lots of small and independant services that work together to create usefull application)



- Stuff that runs on the masters are called 'the cluster control plane' (also known as head nodes) (monitors it, make the changes, schedules the work, reponds to events)
- Nodes are where we run our user or our business applications (report back to the masters, watch for new instructions)

- Aim for multi master control planes for maximum availability (3 is the magic number) the more there are, the more time is needed to reach consensus (only one master is elected to make changes to the custer)

## Masters buzzwords:

- Cluster store: The only persistent compoinent of the entire control plane where the config and the satate of the cluster itself as well as any apps running in it gets stored. The containers running in it **share** the environment. Based on etcd (look up about it)
- Kube-controller-manager: Controller of controllers; Theres a controller for everything in the cluster 
- Kube scheduler: Watches API  Server (kubectl) for new work tasks, Assigns work to cluster nodes;

**Overview**: 	Commands and queries come into the api server by usually using 'kubectl' command line tool, they get autenticated and authorized, the desired state gets written to the cluster store as a record of intent, and the scheduler farms the work out to nodes in the cluster. 

Once thats done, various controllers sits in watch loops observing the state of the cluster and making sure that it matches what we've asked for.

## Nodes buzzwords:

- Kubelet and Nodes are the same thing
- Kubelet: 
		- Main k8s agent that runs on every cluster nodes;  
		- Registers node with cluster; 
		- Watch the API server on the master for any new pods assigned to it, when it sees one it pulls the spec and 		      runs the pod (execute pods)
		- Reports back to masters

## Container runtime:

- Often Docker or containerd

## Pods:
A thin wrapper that K8s every container needs. A shared execution environment (a collection of things that an app needs in order to run).
- One of more containers packaged together as a single deployable unit
- Container for Docker, Pod for K8s
- Mapped using ports because they use the same IP

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
- 
## Declarative model:
- The declarative model and the concept of desired state: 
	- Describe what you want in a manifest file
	- Manifest is posted to the API Server

## Other thoughts:
- K8s doesn't rest until it has obtained the desired state (desired state vs observed state)
