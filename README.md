# Ansible role: labocbz.install_kibana

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Kibana](https://img.shields.io/badge/Tech-Kibana-orange)
![Tag: Cluster](https://img.shields.io/badge/Tech-Cluster-orange)

An Ansible role install and configure Kibana your server.

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
install_kibana_major_version: "8"

install_kibana_config_path: "/etc/kibana"
install_kibana_host: "0.0.0.0"
install_kibana_port: 5601
install_kibana_cluster_name: "my.kibana-cluster.tld"

install_kibana_rewrite_base_path: false
install_kibana_base_path: ""
#install_kibana_public_base_url: ""

install_kibana_ssl_path: "{{ install_kibana_config_path }}/ssl"
install_kibana_p12_password: "myPassword"

install_kibana_elasticsearch_port: 9200
install_kibana_elastic_user: "elastic"
install_kibana_elastic_password: "myVeryStringP@ssword"
install_kibana_elastic_client_auth: false
install_kibana_ssl_authorities: "/etc/ssl/cacert"

#install_kibana_service_account_token: "myToken"
install_kibana_service_account_token_basename: "TOKEN-TO-BE-CREATED"
install_kibana_elastic_protocol: "http"
install_kibana_elastic_hosts:
  - "localhost"

install_kibana_group: "kibana"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_kibana_major_version: "8"

inv_install_kibana_config_path: "/etc/kibana"
inv_install_kibana_port: 5601
inv_install_kibana_cluster_name: "my.kibana-cluster.tld"
inv_install_kibana_group_name: "all"

inv_install_kibana_rewrite_base_path: false
#inv_install_kibana_base_path: ""
#inv_install_kibana_public_base_url: "https://localhost:{{ inv_install_kibana_port }}"

inv_install_kibana_ssl_path: "{{ inv_install_kibana_config_path }}/ssl"
inv_install_kibana_p12_password: "secret"

inv_install_kibana_elasticsearch_port: 9200
inv_install_kibana_elastic_user: "elastic"
inv_install_kibana_elastic_password: "myVeryStringP@ssword"
#inv_install_kibana_service_account_token: ""
inv_install_kibana_service_account_token_basename: "ANSIBLE-{{ ansible_date_time.iso8601_micro.replace(':', '-').replace('.', '-') }}"
inv_install_kibana_elastic_client_auth: true
inv_install_kibana_ssl_authorities: "{{ inv_install_kibana_ssl_path }}/My-Local-CA-Authority/My-Local-CA-Authority.crt"
inv_install_kibana_elastic_protocol: "https"
inv_install_kibana_elasticsearch_group_name: "elasticsearch"
inv_install_kibana_elastic_hosts: "{{ groups[inv_elasticsearch_group_name] }}"
#  - "molecule-local-instance-1-install-kibana"
#  - "molecule-local-instance-2-install-kibana"
#  - "molecule-local-instance-3-install-kibana"

inv_install_kibana_group: "kibana"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_kibana"
  tags:
    - "labocbz.install_kibana"
  vars:
    install_kibana_major_version: "{{ inv_install_kibana_major_version }}"
    install_kibana_config_path: "{{ inv_install_kibana_config_path }}"
    install_kibana_port: "{{ inv_install_kibana_port }}"
    install_kibana_cluster_name: "{{ inv_install_kibana_cluster_name }}"
    install_kibana_rewrite_base_path: "{{ inv_install_kibana_rewrite_base_path }}"
    install_kibana_base_path: "{{ inv_install_kibana_base_path }}"
    install_kibana_public_pase_url: "{{ inv_install_kibana_public_pase_url }}"
    install_kibana_ssl_src: "{{ inv_install_kibana_ssl_src }}"
    install_kibana_ssl_path: "{{ inv_install_kibana_ssl_path }}"
    install_kibana_p12_password: "{{ inv_install_kibana_p12_password }}"
    install_kibana_elasticsearch_port: "{{ inv_install_kibana_elasticsearch_port }}"
    #install_kibana_service_account_token: "{{ inv_install_kibana_service_account_token }}"
    install_kibana_service_account_token_basename: "{{ inv_install_kibana_service_account_token_basename }}"
    install_kibana_elastic_password: "{{ inv_install_kibana_elastic_password }}"
    install_kibana_elastic_user: "{{ inv_install_kibana_elastic_user }}"
    install_kibana_elastic_protocol: "{{ inv_install_kibana_elastic_protocol }}"
    install_kibana_elastic_hosts: "{{ inv_install_kibana_elastic_hosts }}"
    install_kibana_group: "{{ inv_install_kibana_group }}"
    install_kibana_elastic_client_auth: "{{ inv_install_kibana_elastic_client_auth }}"
    install_kibana_ssl_authorities: "{{ inv_install_kibana_ssl_authorities }}"
  ansible.builtin.include_role:
    name: "labocbz.install_kibana"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-21: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* This role use custom SSL files for Kibana and for Elasticsearch, SSL files can be the sames
* This role use fact storing in the node only for idempotency, in the real world you already have a token for Kibana, or an account
* If you dont have any Kibana credentiall the role car use the elastic user to create a token based on the timestamp, only one token is created for each Kibana nodes
* You can provide a list of Elasticsearch node or provide a group, like the inventory file example
* Verify is based on the 401 response of Kibana and the cluster / instance name on the body of the response (with SSL/TLS file check and service state to)
* This role can handle Elasticearch clustering
* An instance of Elasticsearch running on the host is not mandatory, but its better to have a working cluster / standalone Elasticsearch service to perform test

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
