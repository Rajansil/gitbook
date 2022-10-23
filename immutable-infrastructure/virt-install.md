#  Virt-Install

&#x20;   **I.      Download iso**\
&#x20;    **II.     Create vm with `virsh create` command (ks.cfg)**\
&#x20;    **III.    Turn off vm**\
&#x20;    **IV.    Upload image to minio storage**\
\
**Creating  Basic OS**\
&#x20;                   ****                    We will download a .qcow2 image from a Centos official page. Copy the basic kickstart file in the same dir that we have our .qcow2 image. Now, we will create a VM using virt-install command to bring VM up. Once VM is up and running we will \


```
- hosts: HostName
2  become: true
3  vars_files:
4    - vars/ansible-user.yaml //ansible_user
5  vars:
6    work_dir: "/tmp"
7    opennebula_tmp_dir: "/var/tmp"
8    iso_image_path: "CentOS-7-x86_64-Minimal-2009.iso" #79
9    os_name: CentOS-79
10
11  tasks:
12    - name: clean up previos vm
13      shell: |
14        virsh destroy {{ os_name }}
15        virsh undefine {{ os_name }}
16        rm -rf {{ work_dir }}/{{ os_name }}*.qcow2
17      ignore_errors: yes
18      tags:
19        - cleanup
20
21    - name: iso exist
22      stat:
23        path: "{{ work_dir }}/{{ iso_image_path }}"
24      register: iso_image
25
26    - name: print debug when iso image is not exist
27      debug:
28        msg: "{{iso_image_path}} is not exist please download "
29      when: not iso_image.stat.exists
30
31    - name: iso image should exist
32      debug:
33        msg: "{{ work_dir }}/{{iso_image_path}} exist"
34      when: iso_image.stat.exists
35
36    - name: Downloading CentOS Iso Image
37      shell: |
38        mc cp minio/images/CentOS-ISO/{{ iso_image_path }} {{ work_dir }}
39      when: not iso_image.stat.exists
40
41    - name: copy ks
42      copy:
43        src: files/ks.cfg
44        dest: "{{ work_dir }}/ks.cfg"
45        owner: root
46        group: root
47        mode: 0644
48
49    - name: create empty image
50      shell: |
51        qemu-img create -f qcow2 {{ work_dir }}/{{ os_name }}.qcow2 3G #should be min 3G
52
53    - name: install centos with ks (virt install)
54      shell: |
55        virt-install \
56          --virt-type kvm \
57          --name {{ os_name }} \
58          --ram 20480 \
59          --vcpus 12 \
60          --disk {{ work_dir }}/{{ os_name }}.qcow2,format=qcow2 \
61          --network network=default \
62          --graphics vnc,listen=0.0.0.0 \
63          --noautoconsole \
64          --os-type=linux \
65          --os-variant=centos7.0 \
66          --location={{ work_dir }}/{{ iso_image_path }} \
67          --initrd-inject ks.cfg \
68          --extra-args "inst.ks=file:/ks.cfg console=tty0 console=ttyS0,115200n8"
69      args:
70        chdir: "{{ work_dir }}"
71
72    - name: wait for the vm to shut down
73      virt:
74        command: status
75        name: "{{ os_name }}"
76      register: vmstatus
77      until: vmstatus.status == 'shutdown'
78      retries: 1500
79      delay: 10
```

\
&#x20;            **Copying Base OS to MINIO for Backup and** \
&#x20;        ****         In this process we will copy our base OS to minio for a backup so that we don't need this proceess from next time. In the below process we will push our VM into a opennebula\


```
- name: upload image to minio
82      shell: |
83        mc cp {{ work_dir }}/{{ os_name }}.qcow2 minio/images/CentOS-OS/{{ os_name }}.qcow2
84      tags:
85        - minio
86
87    - name: delete previous file
88      file:
89        path: "{{ opennebula_tmp_dir }}/{{ item }}.qcow2"
90        state: absent
91      with_items:
92        - "{{ os_name }}"
93      ignore_errors: yes
94      delegate_to: HostName
95      tags:
96        - add-image-to-opennebula
97
98    - name: minio
99      shell: |
100        mc cp minio/images/CentOS-OS/{{ item }}.qcow2 {{ opennebula_tmp_dir }}/
101      with_items:
102        - "{{ os_name }}"
103      ignore_errors: yes
104      delegate_to: HostName
105      tags:
106        - add-image-to-opennebula
107
108    - name: create Base image on OpenNebula
109      shell: |
110        oneimage create --name {{ item }} --path {{ opennebula_tmp_dir }}/{{ item }}.qcow2 --prefix vd --datastore default --driver qcow2
111      with_items:
112        - "{{ os_name }}"
113      ignore_errors: yes
114      delegate_to: HostName
115      tags:
116        - add-image-to-opennebula
```

