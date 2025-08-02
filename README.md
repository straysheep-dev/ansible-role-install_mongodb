install_mongodb
=========

![ansible-lint workflow](https://github.com/straysheep-dev/ansible-role-install_mongodb/actions/workflows/ansible-lint.yml/badge.svg)

Installs MongoDB Community Edition following the instructions [here](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/).

Requirements
------------

Currently only Ubuntu is supported, though the task files are structure to expand this role to cover more systems in the future.

Any requirements are detailed in the [official documentation](https://www.mongodb.com/docs/manual/installation/).

Role Variables
--------------

`mongodb_package` currently exists under `defaults/main.yml` and points to the latest stable package "`mongodb-org`".

```yml
mongodb_package: "mongodb-org"
```

This should be updated to support [installing specific verisons](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-server.), which requires specifying the version you need for each component, and finally pinning the packages to prevent them from updating:

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