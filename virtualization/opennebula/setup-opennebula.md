# Setup OpenNebula

We are using MYSQLlite to manage and store a data of opennebula. Keeping data base outside the opennebula.

1. Build a VM or Baremetal server to load Opennebula,
2. Install a required package you can vist official page for package name : [https://opennebula.io](https://opennebula.io)
3. Need to install a opennebula repo&#x20;
4. Configure **/etc/one/oned.conf and ssh**&#x20;
5. I am using ansible to build a opennebula



```
---
- name: disable selinux
  selinux:
    state: disabled
	
- name: Install opennebula yum repo
  template: src=opennebula.repo.j2
    dest=/etc/yum.repos.d/opennebula.repo
    owner=root
    mode=644	

- name: install yum depend packages
  yum:
    name:
      - epel-release
      - centos-release-scl-rh
      - ruby-devel
      - make
      - gcc
      - curl-devel
      - expat-devel
      - libxml2-devel
      - libxslt-devel
      - rubygem-rake
      - gcc-c++
      - sqlite-devel
      - mysql-devel
      - python36
      - python36-pip
    state: present

- name: install Opennebula server packages
  yum:
    name:
      - opennebula
      - opennebula-tools
      - opennebula-sunstone
      - opennebula-fireedge
      - opennebula-gate
      - opennebula-flow
      # - opennebula-migration
      # - opennebula-migration-community
      - opennebula-provision
      - opennebula-provision-data
      - opennebula-node-kvm
      # - opennebula-node-firecracker
      # - opennebula-node-lxc
      # - opennebula-node-lxd
      # - opennebula-lxd-snap
      # - opennebula-guacd
      - opennebula-rubygems
      - opennebula-libs
      - opennebula-common
      - opennebula-common-onecfg
      - python3-pyone
      - docker-machine-opennebula
    state: present

- name: install pip module
  pip:
    name:
      - cryptography<3.4
      - "ansible >=2.8.0,<2.10.0"
      - Jinja2>=2.10.0
    executable: pip3

# - name: Downloading Terraform
#   get_url:
#     url: https://releases.hashicorp.com/terraform/0.14.7/terraform_0.14.7_linux_amd64.zip
#     dest: /tmp/terraform.zip

# - name: install terraform
#   shell: |
#     unzip /tmp/terraform.zip -d /usr/bin
#     chmod +x /usr/bin/terraform

- name: Edit /etc/one/oned.conf so it listens in all interfaces (default is localhost only)
  lineinfile: dest=/etc/one/oned.conf
    backup=yes
    regexp="^LISTEN_ADDRESS = \"127.0.0.1\""
    line="LISTEN_ADDRESS = \"0.0.0.0\""

# recommand skip this step on manual
# - name: Install required ruby gems
#   shell: yes | /usr/share/one/install_gems

- name: Copy oneadmin user ssh config
  template:
    src: oneadmin_ssh_config
    dest: /var/lib/one/.ssh/config
    owner: oneadmin
    mode: 600

- name: Enable and start services
  service:
    name: "{{ item }}"
    state: started
    enabled: yes
  with_items:
    - opennebula
    - opennebula-sunstone
```
