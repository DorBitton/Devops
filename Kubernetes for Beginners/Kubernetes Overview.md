## Kubernetes Overview

* Nodes: Kubernetes operates your workload by deploying containers into Pods to execute on Nodes. A node could be either a virtual or physical machine, depending on the cluster's configuration. Each node is managed by the control plane and encompasses the services necessary for running Pods efficiently.
* Cluster: A cluster constitutes a group of Nodes working collectively. In the event of a node failure, the availability of applications is maintained by accessing them from other Nodes within the cluster.
* Master: A Master is a Node configured to serve as the orchestrator for the others. It holds the responsibility for orchestrating all the Nodes within the cluster.

* When you install Kubernetes on a server, you're essentially setting up the following components:
    * API Server: This acts as the front-end interface for users, enabling interaction with the Kubernetes cluster.
    * etcd: This serves as the Key Store, essential for storing all data required to manage the cluster effectively.
    * kubelet: Operating as an agent on each Node in the cluster, the kubelet ensures that containers are running as intended, maintaining their desired state.
    * Container Runtime: This software facilitates the execution of containers. (Traditionally Docker)
    * Controller: Serving as the orchestrator's brain, the Controller is responsible for detecting and responding to Node failures, potentially initiating the creation of new containers as needed.
    * Scheduler: Tasked with distributing workloads across multiple Nodes, the Scheduler acts as a load balancer, ensuring optimal resource utilization within the cluster.

