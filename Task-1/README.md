# Task Session 1: Multi-Container Pods (Init Container & Sidecar)

## 📌 Overview
This task demonstrates how multiple containers within a single Kubernetes Pod can coordinate and share files at the OS level **without using Kubernetes Volumes**. 

A 3-replica nginx deployment runs on a 3-node Minikube cluster. Each Pod runs three containers:
1. **`init-done` (Init Container):** Prints `done` and exits with code `0` before any main containers are allowed to start.
2. **`nginx` (Main Container):** Generates an initial `index.html` file with the current timestamp and serves it on 8081:80 .
3. **`time-updater` (Sidecar Container):** Dynamically updates the `index.html` file inside the `nginx` container every 5 seconds.


### ❓ Questions & Answers

#### 1. Which status does the new Pod show?
The newly created surge Pod initially shows Init:Error and quickly transitions into Init:CrashLoopBackOff with READY 0/2.

#### 2. Do `nginx` and `time-updater` start?
No. they are stuck in the `Waiting` (`Reason: PodInitializing`) state. Kubernetes guarantees that application and sidecar containers will not start until all Init Containers in the Pod complete successfully with exit code `0`.

#### 3. Is the page still reachable, and why?
Yes, the page remains 100% reachable and continues updating the timestamp every 5 seconds, because K8s paused the the roll out as the new pods are not initialized so no pods are terminated (rolling update).
