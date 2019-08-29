# nfs-full-provisioner

The [nfs-full](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs) is an out-of-tree dynamic provisioner for Kubernetes. You can use it to quickly & easily deploy shared storage that works almost anywhere. Or it can help you write your own out-of-tree dynamic provisioner by serving as an example implementation of the requirements detailed in [the proposal](https://github.com/kubernetes/kubernetes/pull/30285). Go [here](./docs/demo) for a demo of how to use it and [here](../docs/demo/hostpath-provisioner) for an example of how to write your own.

It works just like in-tree dynamic provisioners: a `StorageClass` object can specify an instance of nfs-provisioner to be its `provisioner` like it specifies in-tree provisioners such as GCE or AWS. Then, the instance of nfs-provisioner will watch for `PersistentVolumeClaims` that ask for the `StorageClass` and automatically create NFS-backed `PersistentVolumes` for them. For more information on how dynamic provisioning works, see [the docs](http://kubernetes.io/docs/user-guide/persistent-volumes/) or [this blog post](http://blog.kubernetes.io/2016/10/dynamic-provisioning-and-storage-in-kubernetes.html).

## Introduction

This charts installs a custom [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/) into a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. It also installs an [nfs.io/nfs-full-provisioner provisioner](https://quay.io/repository/kubernetes_incubator/nfs-provisioner) into the cluster which dynamically creates persistent volumes from a mount point on one of the Kubernetes cluster nodes.

## Architecture Support

This chart uses multi-architecture images that support s390x (IBM LinuxONE and IBM Z) and amd64 (x86).

## Installation

The recommendation is to run this chart in the kube-system namespace with `Run as system-cluster-critical` checked to ensure that storage is available for the cluster. If you want to run this chart in a different namespace, set `Run as system-cluster-critical` to false to allow it to run in a namespace other than kube-system. 

**Note: If you don't `Run as system-cluster-critical` (i.e. you leave the box un-checked) to run in a namespace other than kube-system, the nfs full provisioner won't be prioritized over other pods in terms of evictions.** 

## Prerequisites

- Kubernetes 1.9+
- MountPath on Kubernetes Node with Backing Storage of Linux FileSystem such as ext4,xfs, etc. **CAN'T use NFS volume** If you want to use an existing NFS share on a node running outside of your cluster instead, see the NFS-Client-Provisioner multi-architecture IBM Cloud Private Package here: https://github.com/IBMRedbooks/SG248458-Implementation-Guide-for-IBM-Blockchain-Platform-for-Multicloud/releases

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```
helm delete my-release --tls --purge
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of this chart and their default values.

| Parameter                         | Description                                 | Default                                                   |
| --------------------------------- | -------------------------------------       | --------------------------------------------------------- |
| `Run as system-cluster-critical`  | Use system-cluster-critical priority class  |          `true`                                           |
| `storageClassName`                | Name of the StorageClass                    |          `fully-managed-nfs-storage`                      |
| `Make StorageClass the default`   | Set as the default StorageClass             |          `true`	                                          |
| `archiveOnDelete`                 | Archive pvc when deleting                   |          `false`                                          |
| `NFS Server Worker Node Hostname` | Hostname or IP - k8s node with server pod   |          `null (ip or hostname)`                          |
| `NFS Server Mount Path`           | Basepath of storage used by nfs server      |          `/export/nfs-dynamic-k8s`                        |
| `NFS Mount options`               | Mount options for nfs (e.g. `- vers=4.1`)   |          `- vers=4.1`
| `NFS Mount options (continued)`   | Mount options for nfs (e.g. `- noatime`)    |          `- noatime`   
