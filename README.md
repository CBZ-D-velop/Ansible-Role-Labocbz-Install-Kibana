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

The Ansible role for installing Kibana is a powerful and versatile automation solution that caters to various aspects of the installation process. With a focus on security and ease of use, this role enables the seamless deployment of Kibana on the target machine while offering robust SSL management capabilities.

Key features of the role include SSL setup for Kibana, ensuring secure communication between Kibana and Elasticsearch. The role can also handle SSL with Client Authentication (mTLS), providing an added layer of security during communication. Furthermore, the role facilitates the installation of Kibana from scratch, eliminating the need to store admin Elastic login credentials, which enhances security by reducing exposure.

Another valuable feature is the ability to set a time-dated configuration token, allowing for eventual deletion or expiration of sensitive configurations. This ensures a more controlled and secure environment, particularly when managing access tokens or credentials.

In addition to its security prowess, the role caters to Elasticsearch clustering. By specifying the desired cluster name with the "install_kibana__cluster_name" variable, users can easily manage Elasticsearch clusters in conjunction with their Kibana installation.

Overall, this Ansible role for installing Kibana provides a comprehensive and flexible solution, empowering users to automate Kibana deployment with SSL, mTLS, and Elasticsearch clustering support, all while ensuring a secure and hassle-free installation process.

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
install_kibana__major_version: "8"

install_kibana__config_path: "/etc/kibana"
install_kibana__host: "0.0.0.0"
install_kibana__port: 5601
install_kibana__cluster_name: "my.kibana-server.tld"

install_kibana__loglevel: "info"
install_kibana__log_path: "/var/log/kibana"

install_kibana__rewrite_base_path: false
install_kibana__base_path: ""
#install_kibana__public_base_url: ""

install_kibana__ssl_path: "{{ install_kibana__config_path }}/ssl"
install_kibana__p12_password: "myPassword"

install_kibana__elasticsearch_port: 9200
install_kibana__elastic_user: "elastic"
install_kibana__elastic_password: "myVeryStringP@ssword"

install_kibana__ssl: true
install_kibana__elastic_client_auth: false
install_kibana__ssl_key: "/etc/ssl/myCert.key"
install_kibana__ssl_crt: "/etc/ssl/myCert.crt"
install_kibana__ssl_authorities: "/etc/ssl/myCert.key"

install_kibana__elastic_ssl_authorities: "/etc/ssl/myCert.key"
install_kibana__elastic_ssl_crt: "/etc/ssl/myCert.crt"
install_kibana__elastic_ssl_key: "/etc/ssl/myCert.key"

#install_kibana__service_account_token: "myToken"
install_kibana__service_account_token_basename: "TOKEN-TO-BE-CREATED"
install_kibana__elastic_protocol: "http"
install_kibana__elastic_hosts:
  - "localhost:9200"

install_kibana__user: "kibana"
install_kibana__group: "kibana"

install_kibana__elastic_ssl_authorities: "/etc/ssl/myCert.key"
install_kibana__elastic_ssl_key: "/etc/ssl/myCert.key"
install_kibana__elastic_ssl_crt: "/etc/ssl/myCert.crt"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host__system_users:
  - login: "kibana"
    group: "kibana"
  - login: "elasticsearch"
    group: "elasticsearch"

inv_install_kibana__major_version: "8"

inv_install_kibana__config_path: "/etc/kibana"
inv_install_kibana__port: 5601
inv_install_kibana__cluster_name: "my-kibana-server.domain.tld"

inv_install_kibana__rewrite_base_path: false
#inv_install_kibana__base_path: ""
#inv_install_kibana__public_base_url: "https://localhost:{{ inv_install_kibana__port }}"

inv_install_kibana__ssl_path: "{{ inv_install_kibana__config_path }}/ssl"

inv_install_kibana__elasticsearch_port: 9200
inv_install_kibana__elastic_user: "elastic"
inv_install_kibana__elastic_password: "myVeryStringP@ssword"
#inv_install_kibana__service_account_token: ""
inv_install_kibana__service_account_token_basename: "ANSIBLE-{{ ansible_date_time.iso8601_micro.replace(':', '-').replace('.', '-') }}"
inv_install_kibana__elastic_client_auth: true
inv_install_kibana__ssl_key: "{{ inv_install_kibana__ssl_path }}/{{ inv_install_kibana__cluster_name }}/{{ inv_install_kibana__cluster_name }}.pem.key"
inv_install_kibana__ssl_crt: "{{ inv_install_kibana__ssl_path }}/{{ inv_install_kibana__cluster_name }}/{{ inv_install_kibana__cluster_name }}.pem.crt"

inv_install_kibana__ssl_authorities: "{{ inv_install_kibana__ssl_path }}/{{ inv_install_kibana__cluster_name }}/ca-chain.pem.crt"
inv_install_kibana__elastic_protocol: "https"
inv_install_kibana__elastic_hosts:
  - "molecule-local-instance-1-install-kibana:9200"

inv_install_kibana__elastic_ssl_authorities: "{{ inv_install_kibana__ssl_authorities }}"
inv_install_kibana__elastic_ssl_key: "{{ inv_install_kibana__ssl_key }}"
inv_install_kibana__elastic_ssl_crt: "{{ inv_install_kibana__ssl_crt }}"

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
    install_kibana__major_version: "{{ inv_install_kibana__major_version }}"
    install_kibana__config_path: "{{ inv_install_kibana__config_path }}"
    install_kibana__port: "{{ inv_install_kibana__port }}"
    install_kibana__cluster_name: "{{ inv_install_kibana__cluster_name }}"
    install_kibana__rewrite_base_path: "{{ inv_install_kibana__rewrite_base_path }}"
    install_kibana__base_path: "{{ inv_install_kibana__base_path }}"
    install_kibana__public_pase_url: "{{ inv_install_kibana__public_pase_url }}"
    install_kibana__ssl: "{{ inv_install_kibana__ssl }}"
    install_kibana__ssl_path: "{{ inv_install_kibana__ssl_path }}"
    install_kibana__elasticsearch_port: "{{ inv_install_kibana__elasticsearch_port }}"
    #install_kibana__service_account_token: "{{ inv_install_kibana__service_account_token }}"
    install_kibana__service_account_token_basename: "{{ inv_install_kibana__service_account_token_basename }}"
    install_kibana__elastic_password: "{{ inv_install_kibana__elastic_password }}"
    install_kibana__elastic_user: "{{ inv_install_kibana__elastic_user }}"
    install_kibana__elastic_protocol: "{{ inv_install_kibana__elastic_protocol }}"
    install_kibana__elastic_hosts: "{{ inv_install_kibana__elastic_hosts }}"
    install_kibana__elastic_client_auth: "{{ inv_install_kibana__elastic_client_auth }}"
    install_kibana__ssl_authorities: "{{ inv_install_kibana__ssl_authorities }}"
    install_kibana__ssl_key: "{{ inv_install_kibana__ssl_key }}"
    install_kibana__ssl_crt: "{{ inv_install_kibana__ssl_crt }}"
    install_kibana__elastic_ssl_authorities: "{{ inv_install_kibana__elastic_ssl_authorities }}"
    install_kibana__elastic_ssl_key: "{{ inv_install_kibana__elastic_ssl_key }}"
    install_kibana__elastic_ssl_crt: "{{ inv_install_kibana__elastic_ssl_crt }}"
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

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-15: System users

* Role can now use system users and address groups

### 2023-25-12: Elastic custom certs

* Role handle multiples certs for  Kibana / Elastic

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
