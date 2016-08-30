# ose_pvcreate_lun - Create one of more luns for Openshift Container Platform
## A Easy way of creating storage volumes for kubernetes persistent volume
In Openshift Origin, Openshift Enterprise, Kubernetes, all have a concept of persistant volumes.
When these are implemented on iscsi, u need to create a large number of storage volumes, that are
shared via iscsi. This scripts assist in doing that. 

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

## Example
# Create four 1 gig devices and export them
./ose_pvcreate_lun vg1 1G 4
[root@iscsitest ~]# tail /var/log/messages  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Allocating group tables: 0/8#010#010#010   #010#010#010done  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Writing inode tables: 0/8#010#010#010   #010#010#010done  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Creating journal (8192 blocks): done  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Writing superblocks and filesystem accounting information: 0/8#010#010#010   #010#010#010done  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun:   
Aug 22 18:35:07 iscsitest root: Created block storage object vol43x1G_server using /dev/vg1/vol43x1G.  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Created LUN 42.  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Created LUN 42->42 mapping in node ACL iqn.2016-02.local.azure.nodes  
Aug 22 18:35:07 iscsitest ./ose_pvcreate_lun: Configuration saved to /etc/target/saveconfig.json  

## Listing the iscsi devices
[root@iscsitest ~]# targetcli / ls
o- / ......................................................................................................................... [...]
  o- backstores .............................................................................................................. [...]
  | o- block ................................................................................................. [Storage Objects: 43]
  | | o- vol10x1G_server ......................................................... [/dev/vg1/vol10x1G (1.0GiB) write-thru activated]
  | | o- vol11x1G_server ......................................................... [/dev/vg1/vol11x1G (1.0GiB) write-thru activated]
  | | o- vol12x1G_server ......................................................... [/dev/vg1/vol12x1G (1.0GiB) write-thru activated]
  | | o- vol13x1G_server ......................................................... [/dev/vg1/vol13x1G (1.0GiB) write-thru activated]
  | | o- vol14x1G_server ......................................................... [/dev/vg1/vol14x1G (1.0GiB) write-thru activated]
  | | o- vol15x1G_server ......................................................... [/dev/vg1/vol15x1G (1.0GiB) write-thru activated]
  | | o- vol16x1G_server ......................................................... [/dev/vg1/vol16x1G (1.0GiB) write-thru activated]
  | | o- vol17x1G_server ......................................................... [/dev/vg1/vol17x1G (1.0GiB) write-thru activated]
  | | o- vol18x1G_server ......................................................... [/dev/vg1/vol18x1G (1.0GiB) write-thru activated]
  | | o- vol19x1G_server ......................................................... [/dev/vg1/vol19x1G (1.0GiB) write-thru activated]
  | | o- vol1x1G_server ........................................................... [/dev/vg1/vol1x1G (1.0GiB) write-thru activated]
  | | o- vol20x1G_server ......................................................... [/dev/vg1/vol20x1G (1.0GiB) write-thru activated]
  | | o- vol21x1G_server ......................................................... [/dev/vg1/vol21x1G (1.0GiB) write-thru activated]
  | | o- vol22x1G_server ......................................................... [/dev/vg1/vol22x1G (1.0GiB) write-thru activated]
  | | o- vol23x1G_server ......................................................... [/dev/vg1/vol23x1G (1.0GiB) write-thru activated]
  | | o- vol24x1G_server ......................................................... [/dev/vg1/vol24x1G (1.0GiB) write-thru activated]
  | | o- vol25x1G_server ......................................................... [/dev/vg1/vol25x1G (1.0GiB) write-thru activated]
  | | o- vol26x1G_server ......................................................... [/dev/vg1/vol26x1G (1.0GiB) write-thru activated]
  | | o- vol27x1G_server ......................................................... [/dev/vg1/vol27x1G (1.0GiB) write-thru activated]
  | | o- vol28x1G_server ......................................................... [/dev/vg1/vol28x1G (1.0GiB) write-thru activated]
  | | o- vol29x1G_server ......................................................... [/dev/vg1/vol29x1G (1.0GiB) write-thru activated]
  | | o- vol2x1G_server ........................................................... [/dev/vg1/vol2x1G (1.0GiB) write-thru activated]
  | | o- vol30x1G_server ......................................................... [/dev/vg1/vol30x1G (1.0GiB) write-thru activated]
  | | o- vol31x1G_server ......................................................... [/dev/vg1/vol31x1G (1.0GiB) write-thru activated]
  | | o- vol32x1G_server ......................................................... [/dev/vg1/vol32x1G (1.0GiB) write-thru activated]
  | | o- vol33x1G_server ......................................................... [/dev/vg1/vol33x1G (1.0GiB) write-thru activated]
  | | o- vol34x1G_server ......................................................... [/dev/vg1/vol34x1G (1.0GiB) write-thru activated]
  | | o- vol35x1G_server ......................................................... [/dev/vg1/vol35x1G (1.0GiB) write-thru activated]
  | | o- vol36x1G_server ......................................................... [/dev/vg1/vol36x1G (1.0GiB) write-thru activated]
  | | o- vol37x1G_server ......................................................... [/dev/vg1/vol37x1G (1.0GiB) write-thru activated]
  | | o- vol38x1G_server ......................................................... [/dev/vg1/vol38x1G (1.0GiB) write-thru activated]
  | | o- vol39x1G_server ......................................................... [/dev/vg1/vol39x1G (1.0GiB) write-thru activated]
  | | o- vol3x1G_server ........................................................... [/dev/vg1/vol3x1G (1.0GiB) write-thru activated]
  | | o- vol40x1G_server ......................................................... [/dev/vg1/vol40x1G (1.0GiB) write-thru activated]
  | | o- vol41x1G_server ......................................................... [/dev/vg1/vol41x1G (1.0GiB) write-thru activated]
  | | o- vol42x1G_server ......................................................... [/dev/vg1/vol42x1G (1.0GiB) write-thru activated]
  | | o- vol43x1G_server ......................................................... [/dev/vg1/vol43x1G (1.0GiB) write-thru activated]
  | | o- vol4x1G_server ........................................................... [/dev/vg1/vol4x1G (1.0GiB) write-thru activated]
  | | o- vol5x1G_server ........................................................... [/dev/vg1/vol5x1G (1.0GiB) write-thru activated]
  | | o- vol6x1G_server ........................................................... [/dev/vg1/vol6x1G (1.0GiB) write-thru activated]
  | | o- vol7x1G_server ........................................................... [/dev/vg1/vol7x1G (1.0GiB) write-thru activated]
  | | o- vol8x1G_server ........................................................... [/dev/vg1/vol8x1G (1.0GiB) write-thru activated]
  | | o- vol9x1G_server ........................................................... [/dev/vg1/vol9x1G (1.0GiB) write-thru activated]
  | o- fileio ................................................................................................. [Storage Objects: 0]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2016-02.local.store1 .......................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 1]
  |     | o- iqn.2016-02.local.azure.nodes ....................................................................... [Mapped LUNs: 43]
  |     |   o- mapped_lun0 ........................................................................ [lun0 block/vol1x1G_server (rw)]
  |     |   o- mapped_lun1 ........................................................................ [lun1 block/vol2x1G_server (rw)]
  |     |   o- mapped_lun2 ........................................................................ [lun2 block/vol3x1G_server (rw)]
  |     |   o- mapped_lun3 ........................................................................ [lun3 block/vol4x1G_server (rw)]
  |     |   o- mapped_lun4 ........................................................................ [lun4 block/vol5x1G_server (rw)]
  |     |   o- mapped_lun5 ........................................................................ [lun5 block/vol6x1G_server (rw)]
  |     |   o- mapped_lun6 ........................................................................ [lun6 block/vol7x1G_server (rw)]
  |     |   o- mapped_lun7 ........................................................................ [lun7 block/vol8x1G_server (rw)]
  |     |   o- mapped_lun8 ........................................................................ [lun8 block/vol9x1G_server (rw)]
  |     |   o- mapped_lun9 ....................................................................... [lun9 block/vol10x1G_server (rw)]
  |     |   o- mapped_lun10 ..................................................................... [lun10 block/vol11x1G_server (rw)]
  |     |   o- mapped_lun11 ..................................................................... [lun11 block/vol12x1G_server (rw)]
  |     |   o- mapped_lun12 ..................................................................... [lun12 block/vol13x1G_server (rw)]
  |     |   o- mapped_lun13 ..................................................................... [lun13 block/vol14x1G_server (rw)]
  |     |   o- mapped_lun14 ..................................................................... [lun14 block/vol15x1G_server (rw)]
  |     |   o- mapped_lun15 ..................................................................... [lun15 block/vol16x1G_server (rw)]
  |     |   o- mapped_lun16 ..................................................................... [lun16 block/vol17x1G_server (rw)]
  |     |   o- mapped_lun17 ..................................................................... [lun17 block/vol18x1G_server (rw)]
  |     |   o- mapped_lun18 ..................................................................... [lun18 block/vol19x1G_server (rw)]
  |     |   o- mapped_lun19 ..................................................................... [lun19 block/vol20x1G_server (rw)]
  |     |   o- mapped_lun20 ..................................................................... [lun20 block/vol21x1G_server (rw)]
  |     |   o- mapped_lun21 ..................................................................... [lun21 block/vol22x1G_server (rw)]
  |     |   o- mapped_lun22 ..................................................................... [lun22 block/vol23x1G_server (rw)]
  |     |   o- mapped_lun23 ..................................................................... [lun23 block/vol24x1G_server (rw)]
  |     |   o- mapped_lun24 ..................................................................... [lun24 block/vol25x1G_server (rw)]
  |     |   o- mapped_lun25 ..................................................................... [lun25 block/vol26x1G_server (rw)]
  |     |   o- mapped_lun26 ..................................................................... [lun26 block/vol27x1G_server (rw)]
  |     |   o- mapped_lun27 ..................................................................... [lun27 block/vol28x1G_server (rw)]
  |     |   o- mapped_lun28 ..................................................................... [lun28 block/vol29x1G_server (rw)]
  |     |   o- mapped_lun29 ..................................................................... [lun29 block/vol30x1G_server (rw)]
  |     |   o- mapped_lun30 ..................................................................... [lun30 block/vol31x1G_server (rw)]
  |     |   o- mapped_lun31 ..................................................................... [lun31 block/vol32x1G_server (rw)]
  |     |   o- mapped_lun32 ..................................................................... [lun32 block/vol33x1G_server (rw)]
  |     |   o- mapped_lun33 ..................................................................... [lun33 block/vol34x1G_server (rw)]
  |     |   o- mapped_lun34 ..................................................................... [lun34 block/vol35x1G_server (rw)]
  |     |   o- mapped_lun35 ..................................................................... [lun35 block/vol36x1G_server (rw)]
  |     |   o- mapped_lun36 ..................................................................... [lun36 block/vol37x1G_server (rw)]
  |     |   o- mapped_lun37 ..................................................................... [lun37 block/vol38x1G_server (rw)]
  |     |   o- mapped_lun38 ..................................................................... [lun38 block/vol39x1G_server (rw)]
  |     |   o- mapped_lun39 ..................................................................... [lun39 block/vol40x1G_server (rw)]
  |     |   o- mapped_lun40 ..................................................................... [lun40 block/vol41x1G_server (rw)]
  |     |   o- mapped_lun41 ..................................................................... [lun41 block/vol42x1G_server (rw)]
  |     |   o- mapped_lun42 ..................................................................... [lun42 block/vol43x1G_server (rw)]
  |     o- luns ......................................................................................................... [LUNs: 43]
  |     | o- lun0 ........................................................................ [block/vol1x1G_server (/dev/vg1/vol1x1G)]
  |     | o- lun1 ........................................................................ [block/vol2x1G_server (/dev/vg1/vol2x1G)]
  |     | o- lun2 ........................................................................ [block/vol3x1G_server (/dev/vg1/vol3x1G)]
  |     | o- lun3 ........................................................................ [block/vol4x1G_server (/dev/vg1/vol4x1G)]
  |     | o- lun4 ........................................................................ [block/vol5x1G_server (/dev/vg1/vol5x1G)]
  |     | o- lun5 ........................................................................ [block/vol6x1G_server (/dev/vg1/vol6x1G)]
  |     | o- lun6 ........................................................................ [block/vol7x1G_server (/dev/vg1/vol7x1G)]
  |     | o- lun7 ........................................................................ [block/vol8x1G_server (/dev/vg1/vol8x1G)]
  |     | o- lun8 ........................................................................ [block/vol9x1G_server (/dev/vg1/vol9x1G)]
  |     | o- lun9 ...................................................................... [block/vol10x1G_server (/dev/vg1/vol10x1G)]
  |     | o- lun10 ..................................................................... [block/vol11x1G_server (/dev/vg1/vol11x1G)]
  |     | o- lun11 ..................................................................... [block/vol12x1G_server (/dev/vg1/vol12x1G)]
  |     | o- lun12 ..................................................................... [block/vol13x1G_server (/dev/vg1/vol13x1G)]
  |     | o- lun13 ..................................................................... [block/vol14x1G_server (/dev/vg1/vol14x1G)]
  |     | o- lun14 ..................................................................... [block/vol15x1G_server (/dev/vg1/vol15x1G)]
  |     | o- lun15 ..................................................................... [block/vol16x1G_server (/dev/vg1/vol16x1G)]
  |     | o- lun16 ..................................................................... [block/vol17x1G_server (/dev/vg1/vol17x1G)]
  |     | o- lun17 ..................................................................... [block/vol18x1G_server (/dev/vg1/vol18x1G)]
  |     | o- lun18 ..................................................................... [block/vol19x1G_server (/dev/vg1/vol19x1G)]
  |     | o- lun19 ..................................................................... [block/vol20x1G_server (/dev/vg1/vol20x1G)]
  |     | o- lun20 ..................................................................... [block/vol21x1G_server (/dev/vg1/vol21x1G)]
  |     | o- lun21 ..................................................................... [block/vol22x1G_server (/dev/vg1/vol22x1G)]
  |     | o- lun22 ..................................................................... [block/vol23x1G_server (/dev/vg1/vol23x1G)]
  |     | o- lun23 ..................................................................... [block/vol24x1G_server (/dev/vg1/vol24x1G)]
  |     | o- lun24 ..................................................................... [block/vol25x1G_server (/dev/vg1/vol25x1G)]
  |     | o- lun25 ..................................................................... [block/vol26x1G_server (/dev/vg1/vol26x1G)]
  |     | o- lun26 ..................................................................... [block/vol27x1G_server (/dev/vg1/vol27x1G)]
  |     | o- lun27 ..................................................................... [block/vol28x1G_server (/dev/vg1/vol28x1G)]
  |     | o- lun28 ..................................................................... [block/vol29x1G_server (/dev/vg1/vol29x1G)]
  |     | o- lun29 ..................................................................... [block/vol30x1G_server (/dev/vg1/vol30x1G)]
  |     | o- lun30 ..................................................................... [block/vol31x1G_server (/dev/vg1/vol31x1G)]
  |     | o- lun31 ..................................................................... [block/vol32x1G_server (/dev/vg1/vol32x1G)]
  |     | o- lun32 ..................................................................... [block/vol33x1G_server (/dev/vg1/vol33x1G)]
  |     | o- lun33 ..................................................................... [block/vol34x1G_server (/dev/vg1/vol34x1G)]
  |     | o- lun34 ..................................................................... [block/vol35x1G_server (/dev/vg1/vol35x1G)]
  |     | o- lun35 ..................................................................... [block/vol36x1G_server (/dev/vg1/vol36x1G)]
  |     | o- lun36 ..................................................................... [block/vol37x1G_server (/dev/vg1/vol37x1G)]
  |     | o- lun37 ..................................................................... [block/vol38x1G_server (/dev/vg1/vol38x1G)]
  |     | o- lun38 ..................................................................... [block/vol39x1G_server (/dev/vg1/vol39x1G)]
  |     | o- lun39 ..................................................................... [block/vol40x1G_server (/dev/vg1/vol40x1G)]
  |     | o- lun40 ..................................................................... [block/vol41x1G_server (/dev/vg1/vol41x1G)]
  |     | o- lun41 ..................................................................... [block/vol42x1G_server (/dev/vg1/vol42x1G)]
  |     | o- lun42 ..................................................................... [block/vol43x1G_server (/dev/vg1/vol43x1G)]
  |     o- portals .................................................................................................... [Portals: 1]
  |       o- 0.0.0.0:3260 ..................................................................................................... [OK]
  o- loopback ......................................................................................................... [Targets: 0]
[root@iscsitest ~]# 


## Internals:
Internally it automatically creates the base volume as a iscsi target
the first time its executed. It automatically creates volume names, and
uses a hidden file to keep track of a auto incrementing number. 
The script uses recursion in order to create greater than 1 device in a 
single execution. 

On creation of the lun, it creates the needed yml file to register the lun for
openshift, copyies it using scp (ssh), and then executes the openshift oc command remotely
via ssh.

The script redirects all messages to system log. 
