# General Notes about using ansible

If you understand the implications and wish to disable this behavior, you can do so by editing `/etc/ansible/ansible.cfg` or `~/.ansible.cfg`:

```sh
[defaults]
host_key_checking = False
```

Alternatively this can be set by an environment variable:

```sh
export ANSIBLE_HOST_KEY_CHECKING=False
```