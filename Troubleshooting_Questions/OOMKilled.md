Question
    How to fix Kubernetes Pod OOMKilled ?

First understand how kubernetes is managed in a multi project or multi namespace environment. 

Every namespace or a project is granted with certain resource quota, so every pod has to be created with a defined requests and limits.
```
---
apiVersion: v1
kind: Pod
metadata:
 name: k8s-troubleshooting-ep-2
spec:
 containers:
 - name: test-av
  image: ruby
  resources:
   requests:
    memory: "128Mi"
    cpu: "250m"
    limits:
    memory: "250Mi"
    cpu: "500m"
```

This error is catagorised in to two types:
    OOMKilled: Limit Overcommit
    OOMKilled: Container Limit Reached

But before we deep dive into each one of them, we need to understand how a linux operating system handles this problem in general.
   
There is a program called OOM (Out of Memory Manager) that tracks memory usage per process. If the system is in danger of running out of available memory, OOM Killer will come in and start killing processes to try to free up memory and prevent a crash. The goal of OOM Killer is to free up as much memory as possible by killing off the least number of processes.

Under the hood, OOM Killer allocates each running process a score. The greater the score, the greater the possibility the process will be killed off. The method it uses to calculate this score is beyond this tutorial, but it’s good to know that Kubernetes takes advantage of the score to help make decisions about which pods to kill.

The kubelet running on the VM monitors memory consumption. If resources on a VM become scarce, the kubelet will start killing pods. Essentially, the idea is to preserve the health of the VM so that all the pods running on it won’t fail. The needs of the many outweigh the needs of the few, and the few get murdered.

OOMKilled: Limit Overcommit
This error occurs when the sum of pod limits is greater than the available memory on the node.

OOMKilled: Container Limit Reached
This error occurs a pod is using more memory than the set limit.

What to do ? How to fix ?

- Application Logs
- Thread Dumps
- Heap Dumps
