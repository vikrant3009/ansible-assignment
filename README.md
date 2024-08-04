# Ansible 101 by Vikrant - Create minikube cluster, deploy ingress controller and hello world application

This project automates the deployment of a Hello World application on a Kubernetes cluster using Ansible. The deployment includes setting up Minikube, installing necessary tools, deploying the Nginx ingress controller, and securing it with a self-signed TLS certificate.

## Project Structure

- `roles/` - Contains the roles for different tasks.
- `playbook.yml` - The main Ansible playbook that orchestrates the tasks.
- `ansible.cfg` - Contains config detail
- `inventory` - Contains inventory details


### Prerequisites

- Ubuntu system with sudo privileges
- Internet connection
