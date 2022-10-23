# 3⃣ Base Image

In this process we already have our basic OS in our opennebula. Now need to create a VM in OpenNebula and install some configuration along with contexturization features of opennebula\
\
**1.   Create vm on opennebula**\
**2.   Install opennebula-context to vm**\
**3.   Poweroff vm**\
**4.   Save image to other name**\
**5.   Upload new image to minio storage**\
**6.   Delete vm**\
****\
****

```
- hosts: HostName
  become: true
  gather_facts: false
  vars_files:
    - vars/ansible-user.yaml// ansibleuser
 

  vars:
    work_dir: "/tmp"
    os_name: "CentOS-79"
    template_id: Template_number
    network_id: ID_number

  tasks:
    - name: Create VM with OS image
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        attributes:
          NAME: "vmtemplate-{{ os_name }}-sufix-sv1"
        template_id: "{{ template_id }}"
        disk_size: 3 GB
        memory: 4 GB
        vcpu: 4
        count: 1
        networks:
          - NETWORK_ID: "{{ network_id }}"
            MAC: "02:00:00:00:00:00" # dhcp will return
      register: vm

    - name: Wait 30 seconds for vm to become reachable/usable
      wait_for_connection:
      delegate_to: localhost
      timeout: 30
      ignore_errors: true

    - name: setup opennebula context
      shell: |
        echo "hello world" > /tmp/hello.txt
        ip route add default via ip_address
        curl -sLO https://github.com/OpenNebula/addon-context-linux/releases/download/v5.6.0/one-context-5.6.0-1.el7.noarch.rpm
        yum remove -y NetworkManager
        yum localinstall -y one-context-5.6.0-1.el7.noarch.rpm --nogpgcheck
        yum install -y epel-release --nogpgcheck
      delegate_to: vmtemplate-eqix-sv5

    - name: terminate instances
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        state: poweredoff

    - name: wait for the vm to shut down
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
      register: vm
      until: vm.instances[0].state == 'POWEROFF'
      retries: 1500
      delay: 10

    - name: save image
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        disk_saveas:
          name: "{{ os_name }}-Base"

    - name: get saved image info
      one_image:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        name: "{{ os_name }}-Base"
      register: saved_image

    - name: export image
      shell: |
        oneimage show {{ saved_image.id }} | grep SOURCE | awk '{print $3}'
      register: image_source
      delegate_to: HostName

    - name: upload image to minio
      shell: |
        mc cp {{ image_source.stdout }} minio/images/CentOS-Base/{{ os_name }}-Base.qcow2
      delegate_to: HostName

    - name: delete instances
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        state: absent
```
