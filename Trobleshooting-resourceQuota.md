When your Kubernetes resource quota is full, it prevents new workload or resources from being created in the affected namespace
Here are steps to troubleshoot

Check the resource quota first
Kubectl get resourcequota -n <namespace> 

To get detailed usage and limits
Kubectl describe resourcequota <quotaname>

This will show
	- Current usage
	- Hard limit
	- Resource type (e.g pods, cpu, memory, PVC etc)

Identify which resource is full:
   From describe output, look for nay resources where USED Is equal to or exceeds hard, common limits

	-  Pods (reached to define limit)
	- CPU
	- Memory
	- Services
	- PVC
	
	List Existing resources consuming the quota:
	  -  kubctl get pods 
	To inspect resource usage
		- Kubectl top pod
	Check PVC if those are the limiting factor
	Kubectl get pvc
	
	Cleanup up unused resources
	Free up the quota by deleting unused or unnecessary resources
	Kubectl delete pod podName
	Kubctl delete pvc <pvc-name>
	Kubectl delete service
	
	
	Check for resources leaks or orphaned resource
	 Look for: 
	     - Pods stuck in terminating state
	     - PVC not delete after pod is gone
	     - completed job still lingring
	
	
	Request a quota increase 
	
	
	
	To avoid the quota increase:
	-  We should define request/limit resource control
	-  USE HPA to scale properly .
	- Implement monitoring /alerts for quota threshold
	
	

![image](https://github.com/user-attachments/assets/2c88f5f0-b5cd-478e-9fef-f3a07f94c870)
