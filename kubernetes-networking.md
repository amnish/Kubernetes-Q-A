1. **Pod to Pod networking**
		a. Every pods gets its own IP address
		b. Pods can communicate with each other across the node without NAT
		c. This is enabled by CNI(container networking interface) plugin
2. **Service networking**: A service in Kubernetes is an abstraction that defines the stable Ip and DNS name to access the                           group of pods(typically selected by labels), even pods themsefl change over time.
		a. Cluster IP (default): Accessible only with in cluster
		b. Node Port : Expose service on static port on each node
		c. Load balancer: Provision an external load balancer(cloud enviorment)
		d. External name: map the service to an external DNS
          
3.  **Ingress Networking**
		a. Ingress manages the external HTTP/HTTPS access to services.
		b. Use ingress controller(like Nginx, Traefik) to route the traffic based upon on rules like path/host
		c. Support TLS termination, path based routing etc.
4. **Network policies:**
		a. You can define rule for traffic flow from one pod to another pod
		b. You can control who can talk whom
		Example: allow only frontend app to talk with back end. 
		
5. **CNI plugin:**
		These implement the actual networking:
	CNI Plugin	Description
	Calico	Network policies, scalable, supports BGP
	Flannel	Simple overlay network, easy to set up
	Cilium	eBPF-based, advanced security and observability
	Weave	Mesh-based, easy to use for small clusters
	When you install kubernetes (for example with kubeadm), you must install a CNI plugin before pod can be scheduled. Until then, pods will be  in pending state.
	
6.** DNS:**
		a. Kubernetes has build.in DNS service(Core DNS)
		b. Every service geta DNS name like my-service.my-namespace.svc.cluster.local


**Kubernetes Default Networking Model: **
	Kubernetes defined set of networking requirement and its up to CNI plugin to implement them
Default Networking Behaviour (Model):

	Every Pod gets its own IP address.
	All Pods can communicate with all other Pods in the cluster without NAT.
	All nodes can communicate with all Pods (and vice versa).
	DNS-based service discovery is available via CoreDNS.
	

