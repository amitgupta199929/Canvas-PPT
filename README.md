RHEL/CentOS OS Upgrade POC – Status Update

* Initiated the OS upgrade POC to validate migration of the existing CentOS 7 Docker environment to RHEL 9.6.
* Reviewed the existing CentOS 7 Docker test environment and validated the current Docker configuration.
* Successfully created and configured the required CentOS 7 test VMs.
* Verified the existing Docker environment and Docker Swarm configuration on CentOS 7.
* Confirmed that the existing Docker services/workloads are running successfully on the CentOS environment.
* Created the required RHEL 9.6 VM for upgrade/migration testing.
* Completed the required VM hardware and disk configuration.
* Configured the required filesystem layout, including a dedicated /var/lib/docker 50 GB volume for Docker data.
* Configured required mount points such as /home, /var/log, /var/log/audit, /var, /tmp, /boot and /boot/efi.
* Completed network configuration and assigned the required IP/hostname.
* Troubleshot and resolved the firewall/network connectivity issue.
* Successfully completed RHEL VM installation and initial OS configuration.
* Successfully registered the RHEL VM with Red Hat.
* Verified access to the required Red Hat BaseOS and AppStream repositories.
* Confirmed repository/content access through Simple Content Access (SCA).
* Validated system details including RHEL version, hostname, IP connectivity, disks and filesystem utilization.
* Identified the requirement to configure the Docker repository on RHEL.
* Validated that the Docker version on the existing test environment needs to be matched on the RHEL test VM for compatibility.
* Next step: Configure the Docker repository and install the same Docker version as the existing CentOS test environment.
* Perform Docker service validation and Docker Swarm/application testing on RHEL.
* Validate application connectivity, networking, volumes, containers/services and overall workload functionality.
* Based on the POC results, document the compatibility, issues, remediation steps and final upgrade procedure for the production OS upgrade.
