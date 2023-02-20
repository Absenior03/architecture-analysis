<![endif]-->

**Longhorn**

**Introduction :**

Longhorn is free, open source software. Originally developed by Rancher Labs, it is now being developed as a incubating project of the Cloud Native Computing Foundation.

**Project Summary :**

Website

https://www.cncf.io/projects/longhorn/

Organization/Foundation Name

Cloud Native Computing Foundation

License

[https://www.apache.org/licenses/LICENSE-2.0.txt](https://www.apache.org/licenses/LICENSE-2.0.txt)

Proprietary

The Linux Foundation

Source Path

<![if !supportLists]>· <![endif]>[**longhorn/backing-image-manager**](https://github.com/longhorn/backing-image-manager)

<![if !supportLists]>· <![endif]>[**longhorn/longhorn-engine**](https://github.com/longhorn/longhorn-engine)

<![if !supportLists]>· <![endif]>[**longhorn/longhorn-instance-manager**](https://github.com/longhorn/longhorn-instance-manager)

<![if !supportLists]>· <![endif]>[**longhorn/longhorn-manager**](https://github.com/longhorn/longhorn-manager)

<![if !supportLists]>· <![endif]>[**longhorn/longhorn-share-manager**](https://github.com/longhorn/longhorn-share-manager)

<![if !supportLists]>· <![endif]>**longhorn/longhorn-ui**

Brief Description

Longhorn is a lightweight, reliable and easy-to-use distributed block storage system for Kubernetes.

**Project Details :**

**Key Features -**

**Highly available persistent storage for Kubernetes**

In the past, ITOps and DevOps have found it hard to add replicated storage to Kubernetes clusters. As a result many non-cloud-hosted Kubernetes clusters don’t support persistent storage. External storage arrays are non-portable and can be extremely expensive.

Longhorn delivers simplified, easy to deploy and upgrade, 100% open source, cloud-native persistent block storage without the cost overhead of open core or proprietary alternatives.

**Easy incremental snapshots and backups**

Longhorn’s built-in incremental snapshot and backup features keep the volume data safe in or out of the Kubernetes cluster.

Scheduled backups of persistent storage volumes in Kubernetes clusters is simplified with Longhorn’s intuitive, free management UI.

**Cross-cluster disaster recovery**

External replication solutions will recover from a disk failure by re-replicating the entire data store. This can take days, during which time the cluster performs poorly and has a higher risk of failure.

Using Longhorn, you can control the granularity to the maximum, easily create a disaster recovery volume in another Kubernetes cluster and fail over to it in the event of an emergency.If your main cluster fails, you can bring up the app in the DR cluster quickly with a defined RPO and RTO.

**Architecture -**

The Longhorn Manager Pod runs on each node in the Longhorn cluster as a Kubernetes [**DaemonSet.**](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) It is responsible for creating and managing volumes in the Kubernetes cluster, and handles the API calls from the UI or the volume plugins for Kubernetes. It follows the Kubernetes controller pattern, which is sometimes called the operator pattern.

The Longhorn Manager communicates with the Kubernetes API server to create a new Longhorn volume [**CRD.**](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) Then the Longhorn Manager watches the API server’s response, and when it sees that the Kubernetes API server created a new Longhorn volume CRD, the Longhorn Manager creates a new volume.

When the Longhorn Manager is asked to create a volume, it creates a Longhorn Engine instance on the node the volume is attached to, and it creates a replica on each node where a replica will be placed. Replicas should be placed on separate hosts to ensure maximum availability.

The multiple data paths of the replicas ensure high availability of the Longhorn volume. Even if a problem happens with a certain replica or with the Engine, the problem won’t affect all the replicas or the Pod’s access to the volume. The Pod will still function normally.

The Longhorn Engine always runs in the same node as the Pod that uses the Longhorn volume. It synchronously replicates the volume across the multiple replicas stored on multiple nodes.

The Engine and replicas are orchestrated using Kubernetes.

In the figure below,

<![if !supportLists]>· <![endif]>There are three instances with Longhorn volumes.

<![if !supportLists]>· <![endif]>Each volume has a dedicated controller, which is called the Longhorn Engine and runs as a Linux process.

<![if !supportLists]>· <![endif]>Each Longhorn volume has two replicas, and each replica is a Linux process.

<![if !supportLists]>· <![endif]>The arrows in the figure indicate the read/write data flow between the volume, controller instance, replica instances, and disks.

<![if !supportLists]>· <![endif]>By creating a separate Longhorn Engine for each volume, if one controller fails, the function of other volumes is not impacted.

<![if !vml]>![](file:///C:\Users\user\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)<![endif]>

**Current Usage -**

With a total of 158 ongoing projects of companies having a total networth of $7.1T and funding of $13.8B.

Some major companies associated are Apple, Amazon Cloud Services, AT&T, Alibaba Cloud, etc.

**Technical Details -**

### Supported Kubernetes Versions

Please ensure your Kubernetes cluster is at least v1.21 before upgrading to Longhorn v1.4.0 because the supported Kubernetes version has been updated (>= v1.21 and <= v1.26) in v1.4.0.

### Recurring Jobs

After the upgrade, the recurring job settings of volumes will be migrated to new recurring job resources, and the `RecurringJobs` field in the volume spec will be deprecated. [[**doc**](https://longhorn.io/docs/1.4.0/deploy/upgrade/#4-automatically-migrate-recurring-jobs)]

### Backup Migration Time

When upgrading from a Longhorn version >= 1.2.0 and <= v1.2.3 to v1.4.0, if your cluster has many backups, you may expect to have a long upgrade time (with 22000 backups, it could take a few hours). Helm upgrade may timeout and you may need to re-run the upgrade command or set a longer timeout. This is [**a known issue**](https://github.com/longhorn/longhorn/issues/3890).

### Longhorn Uninstallation

To prevent Longhorn from being accidentally uninstalled (which leads to data lost), we introduce a new setting, [**deleting-confirmation-flag**](https://longhorn.io/docs/1.4.0/references/settings/#deleting-confirmation-flag). If this flag is **false**, the Longhorn uninstallation job will fail. Set this flag to **true** to allow Longhorn uninstallation. See more in the [**uninstall**](https://longhorn.io/docs/1.4.0/deploy/uninstall) section.

**Other Information -**

**Longhorn 1.4.0** is just released on December 30, 2022! It comes out with tons of new features, improvements, bug fixes, experimental features generally available, Kubernetes 1.25 support, and so on. In this post, I will brief the primary achievements we have done for 1.4.0
