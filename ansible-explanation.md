## 1. hosts file
```[vagrant]
default ansible_host=127.0.0.1 ansible_port=2222 ansible_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/default/virtualbox/private_key``` <br />
This setup allows Ansible to manage the VM created by Vagrant as if it were any other remote host.