# ose_pvcreate_lun - Create one of more luns for Openshift Container Platform
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

## Dependency
The tool expects root access, and that targetcli and lvm2

yum -y install targetcli
yum -y install lvm2

## Preparing the Server
The following assumes a RHEL 7.X Server

yum -y install targetcli
systemctl start target
systemctl enable target
systemctl restart target.service
firewall-cmd --permanent --add-port=3260/tcp
firewall-cmd --reload

## Preparing the volume
This assumes you stripe a set of volumes into a volume group

pvcreate /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh /dev/sdi /dev/sdj
vgcreate vg1 /dev/sdc /dev/sdd /dev/sde /dev/sdf /dev/sdg /dev/sdh /dev/sdi /dev/sdj

## Internals:
Internally it automatically creates the base volume as a iscsi target
the first time its executed. It automatically creates volume names, and
uses a hidden file to keep track of a auto incrementing number. 
The script uses recursion in order to create greater than 1 device in a 
single execution. 
The script redirects all messages to system log. 
