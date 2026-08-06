
CentOS Test VM Build & Validation Checklist
Purpose: Build a new CentOS VM matching test-ihestia-docker01 (without application) and validate configuration.

Step 1 - Verify Current State
• lsblk
• pvs
• vgs
• lvs
• ip addr
Step 2 - Create Physical Volumes
• pvcreate /dev/sdb
• pvcreate /dev/sdc
Step 3 - Create Volume Groups
• vgcreate vg_system /dev/sdb
• vgcreate vg_docker /dev/sdc
Step 4 - Create Logical Volumes
• lvcreate -L 2G -n lv_home vg_system
• lvcreate -L 1G -n lv_audit vg_system
• lvcreate -L 1G -n lv_tmp vg_system
• lvcreate -l 100%FREE -n lv_log vg_system
• lvcreate -l 100%FREE -n lv_docker vg_docker
Step 5 - Create Filesystems
• mkfs.xfs /dev/vg_system/lv_home
• mkfs.xfs /dev/vg_system/lv_audit
• mkfs.xfs /dev/vg_system/lv_tmp
• mkfs.xfs /dev/vg_system/lv_log
• mkfs.xfs /dev/vg_docker/lv_docker
Step 6 - Network Configuration
• vi /etc/sysconfig/network-scripts/ifcfg-ens192
• TYPE=Ethernet
• BOOTPROTO=none
• DEVICE=ens192
• NAME=ens192
• ONBOOT=yes
• IPADDR=<Assigned IP>
• PREFIX=24
• GATEWAY=10.227.18.1
• DNS1=10.227.18.1
• DOMAIN=hestia.polska
• systemctl restart network
Step 7 - Validation Commands (Reference VM)
• hostnamectl
• cat /etc/centos-release
• uname -r
• ip addr
• ip route
• cat /etc/resolv.conf
• nmcli device status
• nmcli connection show
• lsblk
• df -hT
• blkid
• pvs
• vgs
• lvs
• cat /etc/fstab
• free -h
• lscpu
• systemctl status network
• systemctl status sshd
• systemctl list-unit-files --state=enabled
• cat /etc/passwd
• cat /etc/group
• cat /etc/sudoers
• ls /etc/sudoers.d/
• cat /etc/ssh/sshd_config
• timedatectl
• docker --version
• docker info
• systemctl status docker
• rpm -qa | sort
• getenforce
• sestatus
• systemctl status firewalld
• mount
• df -h
• lsblk
