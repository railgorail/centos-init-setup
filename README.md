# CentOS Initial Setup (Home Lab Only)

This repository contains configuration files and an Ansible playbook for the initial setup of CentOS systems.

⚠️ **Disclaimer:**
This setup is intended for testing and development environments only.
It is not recommended for production use. Please review, adapt, and harden any configurations before deploying to production.

## Usage

If you already have SSH key access to root and want to safely run this playbook, you can use the following Docker command:

```bash
docker run -it --rm \
  -v ~/.ssh:/root/.ssh \
  -v $(pwd):/apps \
  -w /apps \
  alpine/ansible:2.17.0 \
  ansible-playbook -i inventory.ini setup.yaml
```

## Notes

The playbook will:

*   Update the system packages
*   Create a sudo user with SSH key access
*   Harden basic SSH settings (disable root login, password authentication)
*   Configure automatic updates (optional)

Make sure your `inventory.ini` points to the correct hosts and users.

Always test in a non-production environment first.