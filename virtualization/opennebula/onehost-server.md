# Onehost Server

In order to create a VM we need a baremetal server that is attached to open-nebula to store a logs , data and so on. I am using folloeing steps to build a onehost servers:

1. Need to install a openebula node package i.e. qemu-kvm, libvirt and opennebula-node-kvm
2. We need to enable the libvirt service
3. Need to configure network settings according to the company policy
4. At last restart the baremetal server
