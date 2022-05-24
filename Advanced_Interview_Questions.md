**Why does Kubernetes have 3 master nodes for HA ? Why not 2 nodes ?**

Having multiple master nodes ensures that services remain available should master node(s) fail. In order to facilitate availability of master services, they should be deployed with odd numbers (e.g. 3,5,7,9 etc.) so quorum (master node majority) can be maintained should one or more masters fail. In the HA scenario, Kubernetes will maintain a copy of the etcd databases on each master, but hold elections for the control plane function leaders kube-controller-manager and kube-scheduler to avoid conflicts. The worker nodes can communicate with any master’s API server through a load balancer.
Deploying with multiple masters is the minimum recommended configuration for most production clusters.
