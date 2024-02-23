# Ansible role: labocbz.add_ldap_account_auth

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: libnss-ldap](https://img.shields.io/badge/Tech-libnss--ldap-orange)
![Tag: libpam-ldap](https://img.shields.io/badge/Tech-libpam--ldap-orange)
![Tag: ldap-utils](https://img.shields.io/badge/Tech-ldap--utils-orange)

An Ansible role to add a LDAP account auth method on your host.

The Ansible role installs the necessary libraries to set up an LDAP account authentication system for managing user accounts. By configuring a system with this role, users can log in to the machine if their account exists in the specified LDAP directory.

The role simplifies the implementation of LDAP-based authentication, providing a seamless and secure method for user authentication across the system. By defining the LDAP base, protocol, server address, port, version, rootbinddn, and secret, administrators can tailor the role to the specific LDAP environment.

It is essential to note that the role focuses solely on client-side components for LDAP authentication. As such, it does not handle server installation or the management of group hierarchies. The role's primary purpose is to facilitate LDAP queries and authentication requests to validate user credentials against the specified LDAP server.

In summary, the LDAP account authentication role streamlines the installation of necessary libraries to enable LDAP-based authentication on the system. By configuring the appropriate LDAP parameters, administrators can leverage a centralized and secure method for user authentication, enhancing system access controls and providing a more efficient and streamlined authentication process for users.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
add_ldap_account_auth__ldap_base: "dc=example,dc=tld"
add_ldap_account_auth__ldap_protocol: "ldap"
add_ldap_account_auth__ldap_address: "my.ldap.tld"
add_ldap_account_auth__ldap_port: 389
add_ldap_account_auth__ldap_version: 3
add_ldap_account_auth__ldap_rootbinddn: "cn=root,{{ add_ldap_account_auth__ldap_base }}"
add_ldap_account_auth__ldap_secret: "my secret"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_add_ldap_account_auth__ldap_secret: "20XbE@j&*1N4GAj@"
inv_add_ldap_account_auth__ldap_base: "uid=admin,ou=system"
inv_add_ldap_account_auth__ldap_address: "172.17.0.4"
inv_add_ldap_account_auth__ldap_rootbinddn: "{{ inv_add_ldap_account_auth__ldap_base }}"
inv_add_ldap_account_auth__ldap_port: 10389
inv_add_ldap_account_auth__ldap_protocol: "ldap"
inv_add_ldap_account_auth__ldap_version: 3

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.add_ldap_account_auth"
  tags:
    - "labocbz.add_ldap_account_auth"
  vars:
    add_ldap_account_auth__ldap_secret: "{{ inv_add_ldap_account_auth__ldap_secret }}"
    add_ldap_account_auth__ldap_base: "{{ inv_add_ldap_account_auth__ldap_base }}"
    add_ldap_account_auth__ldap_address: "{{ inv_add_ldap_account_auth__ldap_address }}"
    add_ldap_account_auth__ldap_rootbinddn: "{{ inv_add_ldap_account_auth__ldap_rootbinddn }}"
    add_ldap_account_auth__ldap_port: "{{ inv_add_ldap_account_auth__ldap_port }}"
  ansible.builtin.include_role:
    name: "labocbz.add_ldap_account_auth"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2024-02-22: New CICD and fixes

* Added support for Ubuntu 22
* Added support for Debian 11/22
* Edited vars for linting (role name and __)
* Fix idempotency
* New CI, need work on tag and releases
* CI use now Sonarqube

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
