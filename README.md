# Vertical Pod Autoscaler Operator
Deploy Vertical Pod Auto-scaler operator and Implement Auto Scaling of compute resources


## Vision and Goals of the project

In Kubernetes, while actual workloads run within containers, Pods are in fact the smallest deployable unit of computing. This means that all compute resources (CPU/Memory) for any workload are specified at the Pod level. In Kubernetes, both forms of compute resources, CPU and Memory, are specified in similar ways and are done so in a static manner. When managing workloads via Pod, a human operator will first specify what compute resources this Pod will require to operate. It is not uncommon, however, for a workload to demand more resources at peak times, while remaining mostly dormant during off-peak hours. As one might expect, this results in either waste of compute resources or reduced pod performance.
 
The solution here is to move away from manually specifying the compute resources for a pod that are static, and opt for a more automated scaling solution. The ideal solution would identify when a Pod requires additional resources, and adjust its requirements accordingly during the lifetime of the Pod. The Vertical Pod Autoscaler (VPA) was designed to specifically do this. The VPA will automatically scale the resources for a pod based on its usage trends. 

The goal of this project is to transform Operate First OCP clusters from manually specifying compute resource requirements for pods and to use the VPA instead. Thus having all workloads automatically scale up/down their resources on a need basis.

## Users & Personas of the project

Our end-users are:

1. DevOps engineers - monitoring and managing resource utilization. 
2. Developers - deploying kubernetes/OpenShift applications or clusters.
3. Cluster administrators - managing every cluster in a multi-cluster environment.

The purpose here is to have better CPU and memory utilization. DevOps developers and operators are touching clusters and they need to make sure to manage their resources efficiently.

VPA scales pod utilization by sending CPU and memory requests based on utilization. With the help of the monitorization of resources, users can also track to see how VPA contributed to effectiveness.


## Scope and Features of the project

The scope of the project includes configuring Vertical Pod Autoscaler(VPA) through GitOps and tracking the efficiency in CPU/Memory utilization of an application deployed on the Kubernetes cluster with the fluctuating workload.

### Features/Major Milestones
- [ ] Install the VPA autoscaler via GitOps
- [ ] Identify an application to serve as a candidate for VPA testing
- [ ] Create a POC in a demo k8s namespace showcasing VPA working
- [ ] Implement VPA for a live running service for heavy/fluctuating compute workloads
- [ ] Track CPU/Memory utilization in Grafana, confirming results
- [ ] Proceed to implement VPA for all compute heavy workloads

### Out of Scope:

- Monitoring resource metrics and implementation of VPA recommendations are not in scope, we will be using the existing VPA operator.
- Improving the Resource Efficiency for OpenShift Clusters Via Trimaran Schedulers is for future work, which is not in scope.

## Solution Concept

### 1. GitOps Workflow
GitOps approach to Continuous Deployment on Kubernetes is to continuously monitor the cluster's actual state and if it deviates from the desired state, it takes action to correct it. This uses Git repositories as a single source of truth to deliver infrastructure as code. 

In this project, we are enabling the configuration of VPA through GitOps. The following diagram roughly explains how our workflow will be utilizing GitOps:

![GitOps workflow](images/GitOpsFlow.png)

### 2. Vertical Pod Autoscaler (VPA)

VPA ensures that a container’s resources are not under or over-utilized; recommends optimized CPU and memory requests/limits values, and can also automatically update them so that the cluster resources are efficiently used.

Below are the building blocks of VPA:
- **VPA Recommender**: This component reads the pod metrics from Metrics Collector (kube-state-metrics) and suggests resource recommendations to the VPA operator.
- **VPA Admission Controller**: Applies values suggested by the VPA Recommender. 
- **VPA Updater**: Evict and restart the pod, if it is not running in the recommended settings. 

The following Architecture diagram shows how these components interact with each other to autoscale a pod:
    <br>
    <br>
![VPA Architecture](images/vpa_architecture.png)

VPA Flow [Image Source](https://banzaicloud.com/blog/k8s-vertical-pod-autoscaler/)

## Acceptance Criteria

- [ ] To have at least one workload that has users and is autoscaled by the VPA.
- [ ] Creating an application to serve as a candidate for VPA testing, with varying workloads so that CPU/Memory utilization can be tracked in scenarios before VPA and after enabling VPA. 
- [ ] Grafana dashboards are built for visualizing these metrics.

## Release Planning

### Sprint 1
- Install the VPA autoscaler via GitOps

### Sprint 2 
- Create a POC in a demo k8s namespace showcasing VPA working
  - Create and configure a k8s cluster
  - Create and deploy a sample application
  - Install VPA in the cluster via GitOps

### Sprint 3
- Implement VPA for a live running service for heavy/fluctuating compute workloads
  - Find a way to simulate fluctuating compute workloads
  - Run experiment on live service to see VPA in action
  - Analyse results

### Sprint 4
- Track CPU/Memory utilization in Grafana, confirming results
  - Create Grafana dashboard with metrics
  - Configure application to publish metrics
  - Track metrics when VPA is in action

### Sprint 5 
- Implement VPA for all compute-heavy/fluctuating workloads
- Enable VPA via GitOps to all clusters


## Technologies/Frameworks 

- OpenShift/Kubernetes, kube-state-metrics (KSM), Containers
- Grafana, Prometheus
- GitOps, ArgoCD

## Resources
- Github repository: https://github.com/operate-first/apps
- Github issue for the project: https://github.com/operate-first/support/issues/305
- VPA: https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler

## Mentors
- Humair Khan (hukhan@redhat.com)
- Anand Sanmukhani (asanmukh@redhat.com)
- Tom Coufal (tcoufal@redhat.com)
