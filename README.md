# Ansible Project: Deploy Flask App with Docker Compose

This project is a simple Ansible-based deployment setup that automates the installation and setup of a Flask application on a remote Ubuntu server using Docker Compose.

It demonstrates the basics of:

- Infrastructure as Code (IaC)
- Remote server configuration with Ansible
- SSH-based connectivity
- Docker installation and container orchestration
- Application deployment automation

---

## Project Overview

This project automates the deployment of a Flask application on a remote Ubuntu server using Ansible and Docker Compose.

It contains:

- `inventory.ini` — server connection details
- `playbook.yml` — deployment automation script
- `keys/` — SSH key storage
- `ansible-assignment-1.docx` — assignment/reference note

The playbook does the essential DevOps work for you: it installs Docker, downloads the app from GitHub, creates the Compose configuration, starts the app, and checks whether it is responding successfully.

---

## Quick Start

Before anything else, make sure the server details and SSH key are correct in `inventory.ini`.

Then run:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

This is the most important command in the project.

---

## Folder Structure

```bash
ansible/
├── inventory.ini
├── playbook.yml
├── keys/
├── ansible-assignment-1.docx
└── README.md
```

### Files explained

#### `inventory.ini`
This file tells Ansible which remote machine to use.

```ini
[servers]
node1 ansible_host=your-server-host-or-ip

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/ec2-server-key.pem
```

It contains:

- the host group: `servers`
- the target server hostname or IP
- the SSH user: `ubuntu`
- the private key path used for authentication

> The exact server address can be changed as needed, but the setup flow stays the same.

#### `playbook.yml`
This is the main Ansible automation playbook. It performs the following tasks:

1. Updates the system packages
2. Installs Git and required tools
3. Installs Docker
4. Installs Docker Compose
5. Ensures Docker is running
6. Clones the Flask app repository
7. Creates a `docker-compose.yml` if needed
8. Stops any old containers
9. Builds and starts the app
10. Checks if the app is serving on HTTP port 80

---

## What I Learned

This project helped me understand the following:

- Ansible is used to automate server setup and software deployment.
- Hosts are managed using an inventory file.
- Playbooks define tasks to be executed remotely.
- SSH keys are important for secure remote access.
- Docker Compose makes deployment easier for multi-container or single-service apps.
- Automation helps reduce manual setup errors and saves time.
- Idempotent tasks help keep infrastructure consistent and reusable.

---

## Prerequisites

Before running this project, make sure you have:

- Ansible installed on your local machine
- SSH access to the target Ubuntu server
- A valid private key for the EC2 instance
- Internet access to download Docker and the GitHub repository
- A remote Ubuntu machine with port 80 open or a valid app port mapping

---

## Installation

### 1. Install Ansible

On Ubuntu / Debian:

```bash
sudo apt update
sudo apt install ansible -y
```

On macOS:

```bash
brew install ansible
```

On Windows, you can use:

- WSL (recommended)
- Git Bash
- or install Ansible in a Linux environment

---

### 2. Prepare the SSH key

Place your EC2 private key in a secure folder, for example:

```bash
mkdir -p ~/keys
cp /path/to/your/private-key.pem ~/keys/
chmod 400 ~/keys/private-key.pem
```

Then update the key path in `inventory.ini` if needed:

```ini
ansible_ssh_private_key_file=/home/ubuntu/keys/ec2-server-key.pem
```

> Make sure this path matches the machine where Ansible is running.

---

### 3. Verify the inventory

Open `inventory.ini` and check the target server details.

```ini
[servers]
node1 ansible_host=your-server-host-or-ip
```

Update the hostname or IP, username, and SSH key path as needed before running the playbook.

---

### 4. Run the playbook

From the project folder, run:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

If you want to see detailed output:

```bash
ansible-playbook -i inventory.ini playbook.yml -vvv
```

---

## Brief Explanation of the Playbook

The playbook works like this:

### 1. System Setup
It updates package caches and installs essential tools like Git and curl.

### 2. Docker Installation
It installs Docker and Docker Compose so the app can run in a containerized environment.

### 3. Repository Setup
It clones the Flask GitHub repository into `/opt/flask-app`.

### 4. Container Configuration
It creates a `docker-compose.yml` file and defines the service configuration for the app.

### 5. Deployment
It runs:

```bash
docker compose -p flask-app up -d --build
```

This builds the Docker image and starts the container in the background.

### 6. Health Check
Finally, it checks whether the app is reachable on port 80 and returns HTTP `200`.

---

## Why This Is Useful

This project shows a practical example of:

- deploying code to a remote server
- using automation instead of manual commands
- ensuring the environment is consistent across deployments
- managing software with containers

---

## Troubleshooting

### SSH connection issue

Check that:

- the target server value is correct
- the user is correct
- the key file permissions are correct
- port 22 is open on the server

### Docker installation fails

Try:

```bash
sudo apt update
sudo apt install docker.io docker-compose-plugin -y
```

### App not reachable

Check:

- the app service is running
- port 80 is open
- Docker Compose configuration is valid
- the repo cloned successfully

Use:

```bash
docker ps
sudo docker logs <container_name>
```

---

## Final Notes

This project is a good beginner-level Ansible deployment example. It combines remote server management, automation, containerization, and application deployment in a simple and practical workflow.

It is a great starting point for learning:

- Ansible playbooks
- inventory management
- Linux server administration
- Docker deployment
- devops automation

---

## Useful Commands Summary

```bash
ansible --version
ansible-playbook --syntax-check playbook.yml
ansible-playbook -i inventory.ini playbook.yml
ssh -i ~/keys/private-key.pem ubuntu@43.205.110.105
sudo docker ps
sudo docker logs flask-app
```

---

## License

This project is intended for learning and educational use.
