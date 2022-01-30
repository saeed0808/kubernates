Kubernates networking addresses four concerns:-

- Container within a pod use networking to communicate via loopback.
- Cluster Networking provides Comminication between different pods.
- The service resources lets you expose an application running in pods to be rechable from outside your cluster.
- you can also use services to publish services only for consumption inside your cluster
- container to container communication on same pod happens through localhost within the containers.
--------------------------practice-- communication with a pod with two container via localhost
kubectl apply -f pods.yml
kubectl exec testpod -it -c c00 -- /bin/bash 
--------------
Now try to established communication between two different pods within same machine/node
- Pod to Pod Communication on same worker node through pod IP
- By default pods IP will not be accessible Outside the node
------------service---------
- Each pod gets its own IP address, however in a deployment, the set of pods running in one moments in time could be different from the set of pod running that application a moment alter.
- This leads to a problem: if some set of pods (call them 'backend') provides functionality to other pods  ('call them nackend') inside your cluster, howdo the frontends find out and keep track of which IP address to connect,
  so that the frantend can use the backend part of the workload
- when using RC, pods are terminated and created during scaling or replication operations.
- when using deployments, while updating the image version the pods are terminated and new pods take the place of other pods
- pods are very dynamic i.e they come & go on the k8s cluster and on any of the available nodes & it would be difficult to access the pods as the pods IP changes once its recreated.
- service object is an logical bridge between pods and end users which provides virtual IP (VIP).
- services allows clients to reliably connect to the containers running in the pod using the VIP.
- The VIP is not on actual IP connect to a network interface, but its purpose is purely to forward traffic to one or more pods
- kube proxy is the one which keeps the mapping between the VIP and the pods upto date, which queries the API server to learn about new services in the cluster.

- Although each pod has a unique IP address, those IP's are not exposed outside the cluster.
- service helps to expose the VIP mapped to pods & allows application to receive tarffic.
- Labels are used to select which are the pods to be put under a service.
- creating a service  will create an endpiont to access the pods/application in it.
- services can be exposed in different ways by specifying a type in the service spec.
  -> Cluster IP 
  -> NodePort
  -> LoadBalancer: Created by cloud provides that will route external tarffic to exery node on the Nodeport (eg ELB on aws)
- Headless -> Creates several endpoints that are used to produce DNS records each DNS record is bound to a Pod.
- By default service can run only between ports 30,000- 32,767
- The set of pods targeted by a service is usually determined by a selector
----> Cluster IP
- Expose VIP only rechable from within the cluster
- Mainly used to communicate between components of microservices

==========service how it works lab====Cluster Ip==NodePort==LoadBalaner
kubectl apply -f deployhttpd.yml
kubectl get pods -o wide
curl <podip>
kubectl apply -f service.yml
kubectl get svc
curl <clusterip>





every object it will give one ip


