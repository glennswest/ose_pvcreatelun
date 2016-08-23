# ose_pvcreate_lun - Create one of more luns for kubernetes persistent storage
## A Easy way of creating storage volumes for kubernetes persistent volume

## General Usage
The tool can be used to create multiple groups of iscsi storage volumes
based on the needs of persistent storage in Openshift/Kubernetes

# Usage ./ose_pvcreate_lun vg1 1G 
Create single Volume within lvm vg1 with a size of 1G, and share it as a 
iscsi target

# Usage ./ose_pvcreate_lun vg1 1G 10
Create 10 Volumes within lvm vg1 with a size of 1G, and share them as a 
iscsi target

# Usage ose_pvcleanup
Destroy iscsi luns and delete all entries in volume group, and reset auto
sequence used by ose_pvcreate_lun

