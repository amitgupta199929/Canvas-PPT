
---
- name: Configure Linux account on Red Hat servers
  hosts: linux_servers
  become: true

  vars_prompt:
    - name: account_password
      prompt: "Enter password for new Linux account"
      private: true
      confirm: true
      encrypt: sha512_crypt
      salt_size: 16

  vars:
    new_username: linuxopsuser

    sudo_commands:
      - /usr/sbin/dmidecode
      - /usr/sbin/lsof
      - /usr/sbin/fdisk
      - /usr/sbin/dmsetup
      - /usr/sbin/multipath

    read_files:
      - /etc/bashrc
      - /etc/profile
      - /proc/cpuinfo
      - /proc/vmware/sched/ncpu
      - /var/log/dmesg

  tasks:

    - name: Create Linux user
      ansible.builtin.user:
        name: "{{ new_username }}"
        password: "{{ account_password }}"
        shell: /bin/bash
        create_home: true
        state: present

    - name: Ensure home directory permissions
      ansible.builtin.file:
        path: "/home/{{ new_username }}"
        state: directory
        owner: "{{ new_username }}"
        group: "{{ new_username }}"
        mode: "0700"

    - name: Set password to never expire
      ansible.builtin.command:
        argv:
          - chage
          - -M
          - "-1"
          - -I
          - "-1"
          - -E
          - "-1"
          - "{{ new_username }}"
      changed_when: true

    - name: Set password change date to today
      ansible.builtin.shell:
        cmd: "chage -d \"$(date +%F)\" {{ new_username }}"
      changed_when: true

    - name: Create restricted sudoers configuration
      ansible.builtin.copy:
        dest: "/etc/sudoers.d/{{ new_username }}"
        owner: root
        group: root
        mode: "0440"
        content: |
          {{ new_username }} ALL=(root) NOPASSWD: {{ sudo_commands | join(', ') }}
        validate: "/usr/sbin/visudo -cf %s"

    - name: Install ACL package
      ansible.builtin.package:
        name: acl
        state: present

    - name: Check requested files
      ansible.builtin.stat:
        path: "{{ item }}"
      loop: "{{ read_files }}"
      register: file_check

    - name: Grant read access to existing files
      ansible.builtin.command:
        argv:
          - setfacl
          - -m
          - "u:{{ new_username }}:r"
          - "{{ item.item }}"
      loop: "{{ file_check.results }}"
      when: item.stat.exists
      changed_when: true

    - name: Verify user account
      ansible.builtin.command:
        argv:
          - id
          - "{{ new_username }}"
      register: account_info
      changed_when: false

    - name: Display account details
      ansible.builtin.debug:
        var: account_info.stdout
