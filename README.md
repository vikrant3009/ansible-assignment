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


### 4. Run the playbook.yml
```bash
   ansible-playbook playbook.yaml
```

### Output will look like:-
![image](https://github.com/user-attachments/assets/30616850-9073-4c98-a31d-2f6f123b8960)

---------------------------

## Troubleshooting 

This guide provides troubleshooting steps for common issues encountered when running the Ansible playbook for setting up a Kubernetes cluster and deploying the NGINX Ingress Controller, along with the "Hello World" application.

### Common Issues and Solutions

#### 1. **Connection Issues**

**Symptom**: Ansible fails to connect to the target host(s).

**Possible Causes**:
- Incorrect IP address or hostname in the inventory file.
- SSH key or credentials issues.

**Solutions**:
- Verify that the IP address or hostname in the `inventory` file is correct and reachable.
- Ensure the SSH key or credentials specified in the `ansible.cfg` file are correct and that the key has the appropriate permissions (e.g., `chmod 600`).

#### 2. **Permission Denied Errors**

**Symptom**: Errors related to permission denial during playbook execution.

**Possible Causes**:
- Insufficient privileges for the `remote_user`.
- Issues with privilege escalation settings.

**Solutions**:
- Ensure that the `remote_user` in the `ansible.cfg` has sufficient permissions to execute tasks on the target nodes.
- Verify that `become` and `become_method` are correctly configured in `ansible.cfg` to allow privilege escalation.

#### 3. **Role Execution Failures**

**Symptom**: Specific roles fail to execute or return errors.

**Possible Causes**:
- Missing or improperly configured roles.
- Dependencies or prerequisites not met.

**Solutions**:
- Ensure that all roles listed in the playbook are available in the `roles` directory or installed from Ansible Galaxy.
- Check the role documentation for any dependencies or prerequisites and ensure they are met.

#### 4. **Application Deployment Issues**

**Symptom**: The "Hello World" application does not deploy or is not accessible.

**Possible Causes**:
- Errors in the deployment configuration.
- Issues with Kubernetes resources.

**Solutions**:
- Review the logs for the deployment and service to identify any errors.
- Verify the Kubernetes resources such as deployments, services, and ingress are correctly configured.
- Check the status of the pods using `kubectl get pods` and ensure they are running as expected.

#### 5. **TLS Configuration Problems**

**Symptom**: TLS certificates are not properly configured or the application is not accessible over HTTPS.

**Possible Causes**:
- Incorrect TLS certificate configuration.
- Issues with the Ingress resource configuration.

**Solutions**:
- Verify the TLS certificate configuration and paths in the `setup_tls_for_hello_world` role.
- Check the Ingress resource to ensure it is correctly set up to use the TLS certificates.

#### 6. **Failed to import the required Python library (kubernetes) on minikube's Python**

**Symptom**: Playbooked failed with below error
![image](https://github.com/user-attachments/assets/8c7e2de1-263a-4f0e-a6ad-add7d7c48384)
**Possible Causes**:
- Pip 3 and python not installed
- pyYAML, kubernete and jsonpatch not installed

**Solutions**:
- Install required packages on target machine
  ```bash
    apt install python3-pip
    pip install pyYAML
    pip install kubernetes
    pip install jsonpatch
  ```

#### 6. **General Debugging Tips**

- **Enable Verbose Output**: Run the playbook with the `-v` (verbose) option to get more detailed output. For example:

  ```bash
  ansible-playbook -i inventory setup-kubernetes.yml -v
  ```

## Conclusion

This playbook provides a comprehensive solution for setting up a Kubernetes cluster and deploying essential components such as the NGINX Ingress Controller and a sample "Hello World" application. By following the steps outlined, you should be able to deploy a fully functional Kubernetes environment that meets your needs.
