# ansible-playbooktesting
---
- hosts: all
  become: yes
  tasks:
    - name: edit host file
      lineinfile:
        path: /etc/hosts
        line: "169.168.0.1 ansible.xyzcorp.com"
    - name: install elinks
      package:
        name: elinks
        state: latest
    - name: create audit user
      user:
        name: xyzcorp_audit
        state: present
    - name: update motd
      copy:
        src: /home/ansible/motd
        dest: /etc/motd
    - name: update issue
      copy:
        src: /home/ansible/issue
        dest: /etc/issue
- hosts: network
  become: yes
  tasks:
    - name: install netcat
      yum:
        name: nmap-ncat
        state: latest
    - name: create network user
      user:
        name: xyzcorp_network
        state: present
- hosts: sysadmin
  become: yes
  tasks:
    - name: copy tarball
      copy:
        src: /home/ansible/scripts.tgz
        dest: /mnt/storage/
