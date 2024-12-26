# lvm
Logical Volume Manager (LVM)
+ Logical Volume Manager (LVM) is a disk management system in Linux that allows you to manage storage volumes more flexibly than with standard partitioning. With LVM, you can resize, move, and manage storage space dynamically

+ Architecture :
- Physical Volumes (PVs) : 
* Physical Volumes (PVs) are the lowest layer in the LVM architecture. They represent the actual physical storage devices, which can be entire disks, partitions(Linux LVM type and not formated with a file system), RAID arrays, SAN disk 
* PVs are marked with a special LVM header in the meta data, allowing LVM to manage them as part of the overall storage pool
* PVs consist of PEs (Physical Extents) that are the smallest allocatable units of storage
- Volume Groups (VGs) :
* Volume Groups (VGs) are the core of LVM’s storage pool concept. VGs combine one or more physical volumes into a single storage pool, that act like a disk
* By pooling PVs into a VG, you can allocate and resize storage space flexibly without worrying about physical disk boundaries
- Logical Volumes (LVs) :
* Logical Volumes (LVs) are the user-facing storage units in LVM. They are created from a Volume Group and act like partitions that can be formatted with a file system, mounted, and used for data storage
* LVs are flexible and can be resized, deleted, or moved between PVs without interrupting access to data, depending on the file system type
* LVs made up of Logical Extents (LEs), which map to PEs, By default, each LE corresponds to a single PE, but certain configurations (e.g., mirroring) can modify this mapping

+ commands : 
- Physical Volume (PV) Commands :
* pvs (Lists all PVs)
* pvdisplay (displays informations about PVs)
* pvcreate <pv-path> <pv-path> ... (Initializes a disk or partition as a PV)
* pvmove <pv-path> (Move PEs from that PV to another within the same VG)
* pvremove <pv-path> (Removes LVM metadata from a PV, de-initializing it)
* pvresize <pv-path> (Resizes a PV)
- Volume Group (VG) Commands :
* vgdisplay (Displays detailed informations about VGs)
* vgs (Lists all VGs)
* vgcreate <vg-name> <pv-path> <pv-path> ... (Creates a VG from one or more PVs)
* vgextend <vg-name> <pv-path> (Adds a PV to an existing VG, expanding its size)
* vgreduce <vg-name> <pv-path> (Removes a PV from a VG, shrinking its size)
* vgremove <vg-name> (Deletes a VG, must be empty of LVs first)
* vgrename <old_name> <new_name> (Renames an existing VG)
- Logical Volume (LV) Commands : 
* lvdisplay (Displays detailed informations about LVs)
* lvs (Lists all LVs)
* lvcreate -L <size><unit> -n <lv-name> <vg-name> (create a lv, unit like M,G...)
* lvresize -r -L <new-size> <lv-path> (Resizes an LV to a specific size, -r to resize the file system)
* lvextend -r -L [+]<size><unit> <lv-path> (Increases the size of an LV. Requires available VG space, + mean add the the size to the existing one)
* lvreduce -r -L -size <lv-path> (Reduces the size of an LV, ensure data is backup before shrinking)
* lvremove </dev/vg-name/lv-name> (Deletes an LV)
* lvrename <vg-name> <old-name> <new-name> (Renames an LV within a VG)
- Snapshot Management Commands :
* lvcreate -s -L size -n <snapshot-name> </dev/vg-name/lv-name> (Creates a snapshot of an LV)
* lvremove </dev/vg-name/snapshot-name> (Removes a snapshot when no longer needed)
