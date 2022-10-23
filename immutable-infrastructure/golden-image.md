# 4⃣ Golden-Image

Golden Image means we will have some basic stuffs of company and all the required packages that we have in Centos OS. \
Once we create a Base Image, now we will do that following to make our golden image.

1. **Create vm instance**
2. **Run ansible `tasks/golden-server-config.yaml`**
3. **Terminate vm**
4. **Save image**
5. **Upload to minio**
6. **Delete vm instance**\
   ****

**`Golden-server-config.yaml has ome basic package of company which won't be change so often. installing some yum packages, Disbaling ipv4/6, node_exporter, editing sudoers file and so on`** \
**``**

```
- hosts: HostName
  become: true
  gather_facts: false
  vars_files:
    - vars/ansible-user.yaml // ansible_user

  vars:
    network_id: 4
    os_name: "CentOS-79"
    template_id: ID

  tasks:
    - name: Create VM with Base Image // automatic we can do manually 
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        attributes:
          NAME: "vmtemplate-{{ os_name }}-eqix-sv5"
        template_id: "{{ template_id }}"
        disk_size: 4 GB
        memory: 4 GB
        vcpu: 4
        count: 1
        networks:
          - NETWORK_ID: "{{ network_id }}"
            MAC: "02:00:00:00:00:00" # dhcp will return 172.22.13.101
      register: vm
      tags:
        - create_vm

    - name: Wait 30 seconds for vm to become reachable/usable
      wait_for_connection:
      delegate_to: localhost
      timeout: 30
      ignore_errors: true
      tags:
        - golden_image

    - name: terminate instances
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        state: poweredoff
        # disk_saveas:
        #   name: "{{ os_name }}-Golden"
      tags:
        - golden_image

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
      tags:
        - snapshot

    - name: save image
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        disk_saveas:
          name: "{{ os_name }}-Golden"
      tags:
        - golden_image

    - name: get saved image info
      one_image:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        name: "{{ os_name }}-Golden"
      register: saved_image
      tags:
        - golden_image

    - name: export image
      shell: |
        oneimage show {{ saved_image.id }} | grep SOURCE | awk '{print $3}'
      register: image_source
      delegate_to: HostName
      tags:
        - golden_image

    - name: image source info debug
      debug:
        var: image_source
      delegate_to: HostName
      tags:
        - golden_image

    - name: upload image to minio
      shell: |
        mc cp {{ image_source.stdout }} minio/images/CentOS-Base/{{ os_name }}-Base.qcow2
      delegate_to: HostName
      tags:
        - golden_image

    - name: delete instances
      one_vm:
        api_url: "{{ api_url }}"
        api_username: "{{ api_username }}"
        api_password: "{{ api_password }}"
        instance_ids: "{{ vm.instances[0].vm_id }}"
        state: absent
      tags:
        - golden_image
```

Final Step for Golden Image is:\
&#x20;       **1.   Download golden image from minio**\
&#x20;         **2.  Import to opennebula**

```
- hosts: all # should be leader for opennebula
  become: true
  gather_facts: false
  vars_files:
    - vars/ansible-user.yaml

  vars:
    opennebula_tmp_dir: "/var/tmp"
    today: "{{ lookup('pipe', 'date +%Y%m%d') }}"
    # os_name: "CentOS-74"
    # os_name: "CentOS-76"
    # os_name: "CentOS-78"
    os_name: "CentOS-79"

  tasks:
    - name: download Golden Image to opennebula
      shell: |
        rm -rf {{ opennebula_tmp_dir }}/{{ os_name }}*.qcow2
        mc cp minio/images/CentOS-Golden/{{ os_name }}-Golden.qcow2 {{ opennebula_tmp_dir }}/

    - name: create Golden Image On OpenNebula
      shell: |
        oneimage create --name {{ os_name }}-Golden-{{ today }} --path {{ opennebula_tmp_dir }}/{{ os_name }}-Golden.qcow2 --prefix vd --datastore default --driver qcow2
      tags:
        - add_image 
```

