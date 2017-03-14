# vmware-ansible
VMWare Ansible playbooks

cluster-shutdown.yaml: will gracefully shut down a cluster, e.g. in response to a power event.
vars/vars.yaml: our non-sensitive variables
vars/passwords.yaml: our credentials
vars/vcluster.yaml: our site-specific config, including the list of VMs
