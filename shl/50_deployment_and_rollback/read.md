15:08
Replocation controller &  replica set is not able to do updates & rollback apps in the cluster

- A deployment object act as a supervisor for pods, giving you fine-grained Control Over how and when a new pod is rolled out, updated ot rolled back to a previous state
- When using deployment object, we first define the state of the app, then k8s cluster schedules mentioned app instance onts specific individual nodes
- k8s then monitors, if the node hosting on instance goes down or pod is deleted the deployment controller replaces it
- This provides a self-healing mechanism to address machine failure or maintaince.

- The following are typical use cases of deployment:-
  - Create a deployment to rollout a Replicaset -> The replicaset creates pods in the Background check the status of the    rollout to see if it sucess or not 
  - Declare the new state of the pods -> By updating the podTemplatespec of the deploymnet A new replicaset is created and the deploymnet manages moving the pods from the
    old replicaset to the new one at a controlled role each new replcaset updates the revision of the deployment
  - Rollback to an earlier Deployment Revision-> if the current state of the deployments is not stable each rollback upadtes the revision of the deployment.
  - Scale up the deploymnet to facilitates more load.
  - Pause the Deployment to apply multiple fixes to its PodTemplateSpec and then resume it to start a new Rollout.
  - Cleanup older replicaSets that you dont need anymore.

- If there are problems in the deployment, kubernates will automatically rollback to the previsous version, however you can also explicitly rollback to a specific revision, as in out case to Revision1 (the orginal Pod version)
- you can rollback to a specific version by specifying it with --to-revision
                Fot eg-> kubectl rollout
                         deploy/mydeployments  --to-revision=2
- when you inspect the deploymets in your cluster, the following field are display
  Name-> List the name of the deployments in the namespace
  Ready-> Display how many replicas of the application are available to your users if follows the pattern ready/Desired
  UP-TO-DATE-> Dispaly the number of replica that have updated to achove the desired state.
  AVILABLE-> Displays how many replicas of the application are available to your users.
  AGE-> Display the amount of time that the application has been running.
==========================================================================LAB=====================

kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     ## tell controller which pods to watch/belongs to
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5; done"]

-------------------------------------------
to check deployments was created or nor
kubectl get deploy
# to check how deployments creates RS & pods
kubectl describe deploy mydeployments
kubectl get rs
# to sacle up or scale down
kubectl scale --replicas=1 deploy mydeployments

To check, what is running inside container

kubectl logs -f <podname>

kubectl rollout status deployment mydeployments

kubectl rollout history deployment mydeployments

kubectl rollout undo deploy/mydeployments

--------------------------Faild Deployments-------------------
Your deploymets may get stuck trying to deploy its newest ReplicaSet without ever completing thgis can occur due to some of the following factors.
 1. Insufficent Quota
 2. Readiness probe failure
 3. Image pull errors
 4. Insufficient permission
 5. Limit Ranges
 6. Application runtime misconfiguration










