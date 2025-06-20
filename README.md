👉 1. If you have a Pod with initContainers that fail, but the main container has restartPolicy: Never, what happens to the Pod status?

Ans. If init container fails, it will not start the main container at all. Kubernetes ensure that init container start first completely before star the main container. 
Main container policy does not effect the init container.

👉 2. When using a StatefulSet with 3 replicas and you delete replica-1, will replica-2 and replica-3 be renamed to maintain sequential ordering?

Answer. No, In Kubernetes , statfulset app  maintain the stable, unique network and persistent storage for each of it pods. The pods area named with predictable ordinal  index.
Let assume out of three myapp-0 is deleted and it will be created with same name

👉 3. Can a DaemonSet Pod be scheduled on a master node that has NoSchedule taint without explicitly adding tolerations?

 Answer. No, a DaemonSet pod cannot be scheduled on a master node (or any node) that has a NoSchedule taint unless it has a matching toleration.

👉 4. If you update a Deployment's image while a rolling update is in progress, will K8s wait for the current rollout to complete or start a new one immediately?

Answer: If you update a Deployment's image while a rolling update is already in progress, Kubernetes will not wait for the current rollout to finish. Instead:
✅ It immediately starts a new rollout with the updated image, effectively cancelling the in progress rollout and replacing with the new one.


👉 5. When a node becomes NotReady, how long does it take for Pods to be evicted, and can this be controlled per Pod?
Answer: When a Kubernetes node becomes NotReady, the eviction of pods from that node is governed by the node controller and pod disruption settings.
By default, the Kubelet and Node Controller work together to evict pods from a NotReady node after:
5 minutes (300 seconds) of continuous NotReady status.


👉 6. Is it possible for a Pod to have multiple containers sharing the same port on localhost, and what happens if they try to bind simultaneously?
 Answer: No, multiple containers in the same pod cannot bind to the same port on localhost simultaneously.
All containers in a pod share the same network namespace. This means they share:
• The same IP address
• The same port space
• The same localhost interface
So, if two containers try to bind to the same port (e.g., 8080) on localhost, the second one will fail with an error like:

listen tcp 127.0.0.1:8080: bind: address already in use


👉 7. If you create a PVC with ReadWriteOnce access mode, can multiple Pods on the same node access it simultaneously?
Ans. Yes, multiple pods on the same node can access a ReadWriteOnce (RWO) PersistentVolumeClaim simultaneously, as long as they are all using the same volume and are scheduled on the same node.

👉 8. When using Horizontal Pod Autoscaler with custom metrics, what happens if the metrics server becomes unavailable during high load?

Ans: If the metrics server is down or unreachable, the HPA cannot get the required metrics to make scaling decisions. And it will remain same

👉 9. Can you run kubectl port-forward to a Pod that's in CrashLoopBackOff state, and will it work?

Answer: Yes, you can run kubectl port-forward to a pod in CrashLoopBackOff, but it will only work if the pod is running at the moment you initiate the command.
CrashLoopBackOff mean: 
	• Pod starts
	• Container crashes (due to an error, misconfiguration, etc.)
	• Kubernetes tries to restart it
	• It crashes again
	• Kubernetes waits a bit longer before trying again (this is the "back-off" part)
	• This cycle continues → CrashLoopBackOff
	
👉 10. If a ServiceAccount is deleted while Pods using it are still running, what happens to the mounted tokens and API access?
Answer: The pod continues  have access  to the cached ServiceAccount token. The token is JWT signed by the Kubernetes API server. . But api server authorization will eventually fail because the service account is deleted . Thei result error 403 Forbidden or unauthorized
		The ServiceAccount token (usually mounted at /var/run/secrets/kubernetes.io/serviceaccount/token)

👉 11. When using anti-affinity rules, is it possible to create a "deadlock" where no new Pods can be scheduled?
Yes, it is possible:
Pod anti-affinity rules prevent pods from being scheduled on the same node (or topology) as other pods with matching labels. If:
• You have strict anti-affinity (e.g., requiredDuringSchedulingIgnoredDuringExecution)
• And there are not enough nodes to satisfy the rule
• Then no new pods can be scheduled, even if there are available resources

affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: my-app
        topologyKey: "kubernetes.io/hostname"


👉 12. Yes, A Job tracks successful completions, not just pod count. If a pod fails and restartPolicy: Never, Kubernetes does not restart that pod.
However, the Job controller sees that the number of successful completions is less than desired, so it creates a new pod to replace the failed one.
but only if the completions field is set


spec:
  parallelism: 3
  completions: 3


👉 13. Can a Pod's resource requests be modified after creation, and what's the difference between requests and limits during OOM scenarios?
No, you cannot modify the resource requests or limits of a running pod directly. Once a pod is created, its resource configuration is immutable.
✅ Workaround:
• You must delete and recreate the pod (or update the Deployment/StatefulSet/ReplicaSet that manages it) with the new resource values.
Request: This is minimum amount of CPU/memory that pod allowed to use
Limit: This is maximum resources that pod can utilize. If pod exceed the limit of memory, it will be killed with error OOM.
If it exceed  the CPU limit, it will be throttled , not killed


👉 14. When using network policies, if you don't specify egress rules, are outbound connections blocked by default?

Ans: By default all traffic is allowed , if no network policy applies to pod
If a Networkpolicy select a pod and includes only ingress rule then only ingress is restricted as per rule and egress remain unrestricted .
No, outbound connections are not blocked by default if you don’t specify egress rules in a NetworkPolicy.
They are allowed unless you explicitly define egress rules.



👉 15. If a Persistent Volume gets corrupted, can multiple PVCs bound to it cause cascading failures across different namespaces?
 yes. It will fail with error "crash due to read/write error" and then enter crashLoopBackOFf

 16. Difference between affinity and anti-affinity and taint
     ![image](https://github.com/user-attachments/assets/ae60c599-5ac7-4757-9699-c98440b999f1)


