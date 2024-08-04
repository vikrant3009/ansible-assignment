# Ansible 101 by Vikrant - Create minikube cluster, deploy ingress controller and hello world application


This Ansible playbook sets up a Kubernetes cluster using Minikube and deploys the NGINX Ingress Controller along with a sample "Hello World" application. The playbook covers installation and configuration of necessary tools and deployment of Kubernetes resources.


## Project Structure

- `roles/` - Contains the roles for different tasks.
- `playbook.yml` - The main Ansible playbook that orchestrates the tasks.
- `ansible.cfg` - Contains config detail
- `inventory` - Contains inventory details



## Prerequisites

Before running this playbook, ensure you have the following:

- Ansible installed on your control node.
- Access to the target nodes where Minikube will be installed.
- Passwordless ssh connectivity between control node and target machine.

## Playbook Overview

The playbook performs the following tasks:

1. **Installs Minikube**: Sets up Minikube on the target node.
2. **Installs kubectl**: Installs the Kubernetes CLI tool.
3. **Installs Helm**: Installs the Helm package manager for Kubernetes.
4. **Deploys NGINX Ingress Controller**: Installs the NGINX Ingress Controller for managing ingress traffic.
5. **Deploys Hello World Application**: Deploys a simple "Hello World" application to the Kubernetes cluster.
6. **Deploys Hello World Service**: Creates a Kubernetes Service to expose the "Hello World" application.
7. **Deploys Hello World Ingress Resource**: Sets up an Ingress resource for routing traffic to the "Hello World" application.
8. **Sets Up TLS for Hello World**: Configures TLS certificates for secure access to the "Hello World" application.
9. **Verifies Hello World**: Checks the deployment status and ensures the "Hello World" application is accessible.


## Steps to Install and Run the Ansible Playbook


### 1. Create config file `ansible.cfg`
The playbook relies on an `ansible.cfg` configuration file. Below is an explanation of the configuration settings used:

```ini
[defaults]
inventory = inventory
remote_user = root
host_key_checking = False

[privilege_escalation]
become = True
become_method = sudo
```

**[defaults]**

`inventory = inventory`: Specifies the inventory file that contains the list of hosts or nodes where the playbook will be executed. The default file name is inventory.

`remote_user = root`: Defines the user account to use when connecting to remote hosts. In this case, it is set to root.

`host_key_checking = False`: Disables SSH host key checking to avoid prompts for new host keys. This is useful for automated environments but should be used with caution.


**[privilege_escalation]**

`become = True`: Enables privilege escalation, allowing Ansible to execute tasks with elevated privileges (e.g., sudo).

`become_method = sudo`: Specifies the method used for privilege escalation. Here, sudo is used to run commands as a superuser.


### 2. Create Inventory

The playbook relies on an `ansible.cfg` configuration file. Below is an explanation of the configuration settings used:

**`inventory`**

```ini
[all]
192.168.1.15 ingress_host="www.devopsbyvikrant.com" 
```

**Sections Explained**

`[all]`: Defines a group called all, which includes all hosts in the inventory.


`192.168.1.15`: Defines a host with the IP address 192.168.1.15.


`ingress_host="www.devopsbyvikrant.com"`: Sets a host variable named ingress_host with the value "www.devopsbyvikrant.com". This variable can be used within your playbook to configure settings related to the ingress host.


### 3. Create `playbook.yml`

Here is the content of the playbook file named `playbook.yml`:

```yaml
---
- name: Setup Kubernetes Cluster and Deploy NGINX Ingress Controller
  hosts: all
  become: true

  roles:
    #- install_minikube
    #- install_kubectl
    #- install_helm
    #- deploy_nginx_ingress_controller
    - deploy_hello_world_application
    - deploy_hello_world_service
    - deploy_hello_world_ingress_resource
    - setup_tls_for_hello_world
    - verify_hello_world
```

### 3. Create roles with following structure.

![image](https://github.com/user-attachments/assets/6eb02dc7-21a2-4887-b364-7f99c9a2a6c5)
