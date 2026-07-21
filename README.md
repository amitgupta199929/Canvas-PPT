Status: In Progress

Completed:

* Created/verified the audytusrsudo account on accessible Linux VMs.
* Configured SSH public key authentication.
* Added the user to the sshusers group (required by AllowGroups).
* Added the user to the wheel group for sudo access where required.
* Verified:
    * PubkeyAuthentication yes
    * AuthorizedKeysFile .ssh/authorized_keys
    * AllowGroups sshusers
    * User/group membership using id audytusrsudo.

Pending:

* Unable to configure the remaining VMs due to lack of server access (primarily Azure-hosted VMs).
* Access request needs to be approved/provided before completing the configuration.

Next Steps:

* Obtain access to the remaining VMs.
* Configure audytusrsudo on those servers.
* Validate Nessus authentication and sudo access on all VMs.
