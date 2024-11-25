# Ceph CSI CephFS (Managed by Argo CD 🦑)

Container Storage Interface (CSI) driver, provisioner, snapshotter and resizer for CephFS (Ceph File System)

## Preparing a CephFS

> [!CAUTION]
> Ensure you are in the **Proxmox shell** to execute these steps

### Set the Name of the CephFS

```sh
export FS_NAME=ops-cluster
```

Replace `ops-cluster` with your desired filesystem name

### Create Metadata and Data Pools

```sh
ceph osd pool create ${FS_NAME}-metadata
ceph osd pool create ${FS_NAME}-data
```

### Create the CephFS Filesystem

```sh
ceph fs new ${FS_NAME} ${FS_NAME}-metadata ${FS_NAME}-data
```

### Verify Filesystem Creation

List the filesystem to ensure it was created successfully

```sh
ceph fs ls
```

### Set Up MDS Directory and Key

Create an MDS directory for storing the daemon's keyring

```sh
mkdir /var/lib/ceph/mds/ceph-${FS_NAME}
ceph auth get-or-create mds.${FS_NAME} mon 'profile mds' mgr 'profile mds' mds 'allow *' osd 'allow *' > /var/lib/ceph/mds/ceph-${FS_NAME}/keyring
```

### Start MDS Service

```sh
systemctl start ceph-mds@${FS_NAME}
```

### Configure MDS Filesystem Association

```sh
ceph config set mds.${FS_NAME} mds_join_fs ${FS_NAME}
```

### Create a CSI Subvolume Group

```sh
ceph fs subvolumegroup create ${FS_NAME} csi
```

### Verify Subvolume Group Creation

Ensure the subvolume group was created successfully

```sh
ceph fs subvolumegroup ls ${FS_NAME}
```

### Check Ceph Health and Monitor Endpoints

```sh
ceph -s
```

> [!IMPORTANT]
> The cluster health result should be **HEALTH_OK**

### Retrieve Admin Key

Use for authenticate with Ceph

```sh
ceph auth get-key client.admin
```

## Next Steps

After completing these steps, proceed to configure Argo CD Apps and Argo CD App Projects on [values.yaml](../argo-apps/values.yaml) to deploy the CSI driver for CephFS. Configure the necessary values to manage and deploy the application effectively

---

**References:**
- https://artifacthub.io/packages/helm/ceph-csi/ceph-csi-cephfs
- https://github.com/ceph/ceph-csi/tree/v3.11.0/charts/ceph-csi-cephfs