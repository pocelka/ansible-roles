# Introduction

This repository contains [Ansible](https://www.ansible.com/) roles; mostly as a learning experience to get familiar with Ansible. These roles are usable for me, however might not be usable for others. Feedback is welcome, and I will try to incorporate changes on best effort basis.

The idea behind these roles is to have some automation to prepare Virtual Machine for various test purposes in automated way; i.e. the process would be:

- Tear down VM
- Restore backup of VM with basic setup; i.e. just OS installed with Ansible user and SSH keys prepared
- Run specific roles against VM
- Do whatever testing might be required

**Ansible version:** 2.10.8

**Roles:**

| Role                 | Description                                         |
| -------------------- | --------------------------------------------------- |
| os_setup             | First system update and some "hardening" activities |
| oracle_19c_sw_only   | Installs Oracle 19c binaries.                       |
| oracle_19c_db_create | Creates single instance database (non-PDB)          |

Role execution: `ansible-playbook -i inventory/hosts [role_name].yml --ask-vault-password`

> These roles are executed against the VM with Oracle Linux 7.7+ as an operating system.

## Roles

This section details any specific changes which should be performed before using role.

### oracle_19c_sw_only

- Download binaries for Linux from Oracle web site and place them to ./roles/oracle_19c_sw_only/files/
- Update group vars for hosts group:

    | Variable               | Description                                 | Example                                  |
    | ---------------------- | ------------------------------------------- | ---------------------------------------- |
    | ora_default_pass       | default password for DBCA                   |
    | ora_oracle_user        | user who owns binaries                      | oracle                                   |
    | ora_install_group      | group for binaries; typically               | oinstall                                 |
    | ora_root_directory     | root directory to be created                | /u01/app                                 |
    | ora_scripts_dir        | folder where various scripts will be copied | /u01/app/script                          |
    | ora_oracle_base        | folder where binaries should be installed   | /u01/app/oracle                          |
    | ora_oracle_inventory   | folder for oracle inventory                 | /u01/app/oraInventory"                   |
    | ora_oracle_home        | oracle home location                        | /u01/app/oracle/product/19.0.0/dbhome_1" |
    | ora_data_dir           | folder for data files                       |
    | ora_binaries_zip       | filename of donwloaded binaries             |

### oracle_19c_db_create

- Update group vars for hosts group:

    | ora_memory_target      | memory for DB in MB                         | 1536                                     |
    | ora_sid                | SID for DB                                  | ORCL                                     |
    | ora_redo_log_files     | number of redo logs to be created           |
    | ora_redo_logs_in_group | number of redo logs in one thread           |
    | ora_redo_size          | redo logs size in MB                        |

    > In example if you specify ora_redo_log_files = 6 and ora_redo_logs_in_group = 3, database will be created with 3 redo logs in thread 1 and 3 redo logs in thread 2.
