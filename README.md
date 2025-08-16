install_mongodb
=========

![molecule workflow](https://github.com/straysheep-dev/ansible-role-install_mongodb/actions/workflows/molecule.yml/badge.svg) ![ansible-lint workflow](https://github.com/straysheep-dev/ansible-role-install_mongodb/actions/workflows/ansible-lint.yml/badge.svg)

Installs MongoDB Community Edition following the [official instructions](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/).

Requirements
------------

Currently only Ubuntu is supported. The task files are structured so that support for additional systems can be added in the future.

Any other requirements are detailed here: [https://www.mongodb.com/docs/manual/installation/](https://www.mongodb.com/docs/manual/installation/).

> [!IMPORTANT]
> If this VM runs in Proxmox you must use the `host` CPU type ([or one that supports AVX](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#_qemu_cpu_types)), not `x86-64-v2-AES`. MongoDB requires this instruction set to run, otherwise you'll get `code=dumped, signal=ILL` in the service status.
>
> - [MongoDB 8.0 Compatability Issue](https://www.mongodb.com/community/forums/t/mongodb-8-0-compatibility-issue/299704)
> - [MongoDB Platform Support Notes](https://www.mongodb.com/docs/manual/administration/production-notes/#platform-support-notes)
> - [MongoDB code=dumped, signal=ILL](https://www.mongodb.com/community/forums/t/i-cannot-install-mongodb-on-ubuntu-22-04-2-live-server-code-dumped-signal-ill/224111)
> - [Proxmox Forum: Enable AVX](https://forum.proxmox.com/threads/enable-avx.129019/)
> - [Proxmox Docs: QEMU CPU Types](https://pve.proxmox.com/pve-docs/pve-admin-guide.html#_qemu_cpu_types)

Role Variables
--------------

`mongodb_package` currently exists under `defaults/main.yml` and points to the latest stable package "`mongodb-org`".

```yml
mongodb_package: "mongodb-org"
```

This should be updated to support [installing specific verisons](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-server.).

To do this, each component needs installed with the same version string, and finally each package needs pinned to prevent them from updating:

> ```bash
> sudo apt-get install -y \
>    mongodb-org=8.0.12 \
>    mongodb-org-database=8.0.12 \
>    mongodb-org-server=8.0.12 \
>    mongodb-mongosh \
>    mongodb-org-shell=8.0.12 \
>    mongodb-org-mongos=8.0.12 \
>    mongodb-org-tools=8.0.12 \
>    mongodb-org-database-tools-extra=8.0.12
>
> echo "mongodb-org hold" | sudo dpkg --set-selections
> echo "mongodb-org-database hold" | sudo dpkg --set-selections
> echo "mongodb-org-server hold" | sudo dpkg --set-selections
> echo "mongodb-mongosh hold" | sudo dpkg --set-selections
> echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
> echo "mongodb-org-cryptd hold" | sudo dpkg --set-selections
> echo "mongodb-org-tools hold" | sudo dpkg --set-selections
> echo "mongodb-org-database-tools-extra hold" | sudo dpkg --set-selections
> ```

Dependencies
------------

None.

Example Playbook
----------------

```yml
- name: "Default Playbook"
  hosts: all
    #network_nodes
  roles:
    - role: straysheep_dev.install_mongodb
```

Load credentials from an ansible-vault and run the playbook with:

```bash
echo "Enter Vault Password"; read -r -s vault_pass; export ANSIBLE_VAULT_PASSWORD=$vault_pass
# [type vault password here, then enter]
ansible-playbook -i <inventory> -e "@~/vault.yml" --vault-pass-file <(cat <<<$ANSIBLE_VAULT_PASSWORD) -v ./playbook.yml
```

License
-------

- [MIT](./LICENSE)

Author Information
------------------

[straysheep-dev/ansible-configs](https://github.com/straysheep-dev/ansible-configs)