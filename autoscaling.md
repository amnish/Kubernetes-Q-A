To configure Horizontal Pod Autoscaling (HPA) in Kubernetes, you need to follow these general steps:

✅ 1. Prerequisites
	a. A Kubernetes cluster with the Metrics Server installed (for CPU/memory-based autoscaling).
	b. For custom metrics, you need a custom metrics adapter (like Prometheus Adapter).
	c. Your deployment should have resource requests/limits defined.
✅ 2. Define Resource Requests and Limits
In your deployment YAML, make sure to define CPU/memory requests and limits:
resources:
  requests:
    cpu: "100m"
    memory: "200Mi"
  limits:
    cpu: "500m"
    memory: "500Mi"


✅ 3. Create the HPA
You can create an HPA using kubectl or a YAML file.

🔹 Using CPU Utilization (example):
kubectl autoscale deployment my-app --cpu-percent=50 --min=2 --max=10

🔹 Using YAML:
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50

✅ 4. Apply the HPA
kubectl apply -f my-app-hpa.yaml

✅ 5. Verify HPA
kubectl get hpa
kubectl describe hpa my-app-hpa

✅ 6. For Custom Metrics (Optional)
Install Prometheus and Prometheus Adapter.
Expose custom metrics via your app or sidecar.
Update the HPA to use type: Pods or type: External metrics.

