# nfs-client-provisioner

The [nfs-client](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs-client) is an automatic provisioner (`nfs.io/nfs-client-provisioner`) for Kubernetes that uses your **already configured** NFS server, automatically creating Persistent Volumes. To install in a namespace other than kube-system, un-check `Run as system-cluster-critical`. The recommended default requires installing the helm chart in kube-system with `Run as system-cluster-critical` checked.

## Introduction

This charts installs a custom [StorageClass](https://kubernetes.io/docs/concepts/storage/storage-classes/) into a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager. It also installs an [NFS client provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs-client) into the cluster which dynamically creates persistent volumes from single NFS share. 

## Architecture Support

This chart uses multi-architecture images that support s390x (IBM LinuxONE and IBM Z) and amd64 (x86).

## Installation

The recommendation is to run this chart in the kube-system namespace with `Run as system-cluster-critical` checked to ensure that storage is available for the cluster. If you want to run this chart in a different namespace, set `Run as system-cluster-critical` to false to allow it to run in a namespace other than kube-system. 

**Note: If you don't `Run as system-cluster-critical` (i.e. you leave the box un-checked) to run in a namespace other than kube-system, the nfs client provisioner won't be prioritized over other pods in terms of evictions.** 

## Prerequisites

- Kubernetes 1.9+
- Existing NFS Share


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of this chart and their default values.

| Parameter                         | Description                                 | Default                                                   |
| --------------------------------- | -------------------------------------       | --------------------------------------------------------- |
| `replicaCount`                    | Number of provisioner instances to deployed |          `1`                                              |
| `Run as system-cluster-critical`  | Use system-cluster-critical priority class  |          `true`                                           |
| `storageClassName`                | Name of the StorageClass                    |          `managed-nfs-storage`                            |
| `Make StorageClass the default`   | Set as the default StorageClass             |          `true`	                                          |
| `archiveOnDelete`                 | Archive pvc when deleting                   |          `false`                                          |
| `NFS Server Hostname or IP`       | Hostname or IP of the NFS server            |          `null (ip or hostname)`                          |
| `NFS Server Mount Path`           | Basepath of the mount point to be used      |          `/export/nfs-dynamic-k8s`                        |
| `nfs.mountOptions`                | Mount options for nfs (e.g. 'nfsvers=3')    |          `null`                                           |