## 1. hosts file
This setup allows Ansible to manage the VM created by Vagrant as if it were any other remote host.
```
[vagrant]
default ansible_host=127.0.0.1 ansible_port=2222 ansible_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/default/virtualbox/private_key
```

##2. ansible.cfg file
- **inventory = hosts**: Sets hosts as the default inventory file.
- **host_key_checking = False**: Disables SSH host key checking, allowing easier connections to new or temporary hosts.
- **retry_files_enabled = False**: Disables the creation of retry files after failed playbook runs, which can help keep the working directory clean.
```
[defaults]
inventory = hosts
host_key_checking = False
retry_files_enabled = False
```

