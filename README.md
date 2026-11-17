# Ansible Automated Compliance Enforcement

An Ansible-based Linux security compliance automation project designed to audit security configurations across multiple Ubuntu servers and apply selected remediation tasks.

The project uses a reusable role-based structure to assess security controls, identify configuration gaps, and improve consistency across managed infrastructure.

## Project Objectives

- Automate Linux security configuration audits.
- Identify security and compliance gaps across multiple servers.
- Provide reusable Ansible roles for security checks.
- Apply selected security remediations.
- Support repeatable infrastructure security assessments.

## Architecture

- **Control node:** Ubuntu on Windows Subsystem for Linux (WSL)
- **Automation:** Ansible
- **Managed infrastructure:** Ubuntu Linux servers hosted on AWS EC2
- **Authentication:** SSH key-based authentication
- **Privilege escalation:** sudo
- **Version control:** Git

## Security Audit Coverage

The compliance checks cover eight areas:

1. SSH security configuration
2. Firewall configuration
3. User account security
4. File and directory permissions
5. Legacy and unnecessary network services
6. System package updates
7. System logging configuration
8. Network security configuration

## Remediation Coverage

The remediation role currently implements tasks for:

- SSH security
- Firewall security
- File permissions
- Network security

Auditing all eight categories does not mean all eight are automatically remediated.

## Repository Structure

```text
ansible-automated-compliance-enforcement/
├── ansible.cfg
├── inventory/
│   ├── hosts.ini
│   └── group_vars/
│       └── all/
│           └── vars.yml
├── playbooks/
│   └── compliance.yml
├── roles/
│   ├── compliance_checks/
│   │   ├── defaults/
│   │   └── tasks/
│   └── compliance_remediation/
│       └── tasks/
├── requirements.yml
├── .gitignore
└── README.md
```

## Prerequisites

- Ansible installed on the control node
- SSH access to managed Ubuntu servers
- A dedicated SSH private key
- sudo privileges on managed hosts
- Python 3 on managed hosts

## Installation

Install the required Ansible collection:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Inventory Configuration

The committed inventory is a sanitized example.

Create a private inventory file named `inventory/hosts.private.ini` containing your actual server details. This file is excluded from Git.

Example:

```ini
[compliance_servers]
server-01 ansible_host=YOUR_SERVER_IP_1
server-02 ansible_host=YOUR_SERVER_IP_2

[compliance_servers:vars]
ansible_user=ubuntu
ansible_python_interpreter=/usr/bin/python3
```

Run the playbook using your private inventory and SSH key:

```bash
ansible-playbook -i inventory/hosts.private.ini \
  playbooks/compliance.yml \
  --private-key ~/.ssh/cloud-horla.pem
```

## Validation

Check inventory structure:

```bash
ansible-inventory -i inventory/hosts.private.ini --graph
```

Test Ansible connectivity:

```bash
ansible compliance_servers -i inventory/hosts.private.ini \
  -m ping --private-key ~/.ssh/cloud-horla.pem
```

## Security Considerations

- Private SSH keys must never be committed.
- Real server addresses should remain in private inventory files.
- Credentials and secrets must not be stored in plaintext within public repository files.
- Review configuration changes before applying remediation.
- Validate SSH and firewall changes carefully to avoid losing remote access.

## Current Project Status

The project contains reusable audit and remediation roles covering the security areas listed above. Further enhancements can include structured compliance reporting, additional remediation controls, automated testing, and CI/CD integration.

## Author

**Jamiu Olatunji Bakre**

Cloud & DevOps Engineer | AWS | Linux | Ansible | Terraform | Kubernetes

- [DEV.to](https://dev.to/jamiu_cloud)
- [Portfolio](https://jahmeeu-cloud.github.io/portfolio-project/)
- [Docker Hub](https://hub.docker.com/u/horlacloud)
