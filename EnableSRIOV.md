1. First check for the driver used for the nic. Assume name is eno3
```
ethtool -i eno3 | grep ^driver
```
Assume the driver name is ixgbe. Consider you need to enable 4 VFs

```
modprobe igb max_vfs=4
```
Make the VFs Persistent
```
echo "options ixgbe max_vfs=4" >>/etc/modprobe.d/ixgbe.conf
```
The modprobe command enables Virtual Functions on all NICs that use the same kernel module, and makes the change persist through system reboots. It is possible to enable VFs for only a specific NIC, however there are some possible issues that can result. One way to do this is
```
echo 4 > /sys/class/net/eno3/device/sriov_numvfs
```
Add to boot parameters:
grubby --update-kernel=ALL --args="ixgbe.max_vfs=4"
