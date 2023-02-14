# ReplicaSets
  - Take me to [Video Tutorial](https://kodekloud.com/topic/replicasets/)

In this section, we will take a look at the below
- Replication Controller
- ReplicaSet

#### Controllers are brain behind kubernetes

## What is a Replica and Why do we need a replication controller?

  ![rc](../../images/rc.PNG)

  > Note: Even with one pod, we can leverage replication controller. Once the pod is down, replication controller will quickly spin up a new pod. Keep high availability.

  ![rc1](../../images/rc1.PNG)

  > Note: Controller works across multiple nodes. Or put in another way, controller works in the entire cluster.

## Difference between ReplicaSet and Replication Controller
- **`Replication Controller`** is the older technology that is being replaced by a **`ReplicaSet`**.
- **`ReplicaSet`** is the new recommended way to setup replication.

## Creating a Replication Controller

## Replication Controller Definition File
  
   ![rc2](../../images/rc2.PNG)
  
```
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
```
  - To Create the replication controller
    ```
    $ kubectl create -f rc-definition.yaml
    ```
  - To list all the replication controllers
    ```
    $ kubectl get replicationcontroller
    ```
  - To list pods that are launch by the replication controller
    ```
    $ kubectl get pods
    ```
    ![rc3](../../images/rc3.PNG)
    
## Creating a ReplicaSet
  
## ReplicaSet Definition File

  ![rs](../../images/rs.PNG)

```
  apiVersion: apps/v1
  kind: ReplicaSet
  metadata:
    name: myapp-replicaset
    labels:
      app: myapp
      type: front-end
  spec:
   template:
      metadata:
        name: myapp-pod
        labels:
          app: myapp
          type: front-end
      spec:
       containers:
       - name: nginx-container
         image: nginx
   replicas: 3
   selector:
     matchLabels:
      type: front-end
```
- :warning: **ReplicaSet requires a selector definition when compare to Replication Controller.**
- :question: With the selector section, ReplicaSet will take the previously created pods into consideration. In this case, will replicaset create more the exactly number of replicas I want? 
  - For example, I already have 1 pod running. Then I want to use replicas set create 3 more pods. In this case, will the replicaset create 3 pods or 2 pods?
    - :key: 2 pods will be created. If we have 3 pods existed, then no pod will be created.
- :question: How ReplicaSet/ReplicaController kowns which pod to watch?
  - :key: Using pod labels to identify pods.
- :question: How ReplicaSet make sure there is a desired number of replicas running in healthy status?
  - :key: 
   
- To Create the replicaset
  ```
  $ kubectl create -f replicaset-definition.yaml
  ```
- To list all the replicaset
  ```
  $ kubectl get replicaset
  ```
- To list pods that are launch by the replicaset
  ```
  $ kubectl get pods
  ```

  ![rs1](../../images/rs1.PNG)

## Labels and Selectors
- :question: **What is the deal with Labels and Selectors? Why do we label pods and objects in kubernetes?**
  - :key: ReplicaSet will use pod labels to identify which pod to watch. 
- :question: **When we have multiple ReplicaSet, do we have multiple processes running (multiple pods in the kube-system namespace) for monitoring?**
  - :key: 

  ![labels](../../images/labels.PNG)
  
## How to scale replicaset
- There are multiple ways to scale replicaset
  - Update the number of replicas in the `replicaset-definition.yaml` definition file. E.g replicas: 6 and then run 
```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 6
     selector:
       matchLabels:
        type: front-end
```

  ```
  $ kubectl replace -f replicaset-definition.yaml
  ```
  - First way to use.
  ```
  $ kubectl apply -f replicaset-definition.yaml
  ```
  - Second way is to use **`kubectl scale`** command.
  ```
  $ kubectl scale --replicas=6 -f replicaset-definition.yaml
  ```
  - Third way is to use **`kubectl scale`** command with type and name
  ```
  $ kubectl scale --replicas=6 replicaset myapp-replicaset
  ```
  - :warning: Fourth way. The above command will not change the replicaset defintion file. **It will rollback the changes in next deployment.**
  ![rs2](../../images/rs2.PNG)

#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
