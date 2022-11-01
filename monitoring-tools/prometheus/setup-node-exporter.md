# Setup Node-exporter

Simply, Node exporter enables us to measure servers resources like memory, disk and CPU utalization.&#x20;

Need to download node\_exporter and copy those to /tmp file, unzip it and install that file. Below we are using ansible script

```
- name: check node_exporter exist
  stat:
    path: /usr/local/bin/node_exporter
  register: node_exporter
  tags:
    - debug

- name: download node exporter
  get_url:
    url: https://github.com/prometheus/node_exporter/releases/download/v{{ node_exporter_version }}/node_exporter-{{ node_exporter_version }}.linux-amd64.tar.gz
    dest: /tmp
  when: node_exporter.stat.exists == False

- name: unarchive node exporter
  unarchive:
    remote_src: yes
    src: /tmp/node_exporter-{{ node_exporter_version }}.linux-amd64.tar.gz
    dest: /tmp
  when: node_exporter.stat.exists == False

- name: move node exporter to /usr/local/bin
  copy:
    src: /tmp/node_exporter-{{ node_exporter_version }}.linux-amd64/node_exporter
    dest: /usr/local/bin/node_exporter
    remote_src: yes
    owner: root
    group: root
    mode: 0755
  when: node_exporter.stat.exists == False

- name: install unit file to systemd
  template:
    src: files/node_exporter/systemd/node_exporter.service
    dest: /etc/systemd/system/node_exporter.service
    owner: root
    group: root
    mode: 0600
  when:
    - ansible_distribution_major_version == "7"

- name: configure systemd to use service
  systemd:
    daemon_reload: yes
    enabled: yes
    state: started
    name: node_exporter.service
  when:
    - ansible_distribution_major_version == "7"

- name: install unit file to systemd
  template:
    src: files/node_exporter/init.d/node_exporter
    dest: /etc/init.d/node_exporter
    owner: root
    group: root
    mode: 0700
  when:
    - ansible_distribution_major_version == "6"
  tags:
    - install

- name: configure init.d to use service
  shell: |
    chkconfig node_exporter on
    /etc/init.d/node_exporter start
  when:
    - ansible_distribution_major_version == "6"
  tags:
    - install

```
