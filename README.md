# CentOS Initial Setup (Home Lab Only)

This repository contains configuration files and an Ansible playbook for the initial setup of CentOS systems, leveraging a dedicated Ansible role.

⚠️ **Disclaimer:**
This setup is intended for testing and development environments only.
It is not recommended for production use. Please review, adapt, and harden any configurations before deploying to production.

## Usage

Ensure your `inventory.ini` is configured. If you already have SSH key access to root, you can safely run this playbook using the provided Docker command:

```bash
docker run -it --rm \
  -v ~/.ssh:/root/.ssh \
  -v $(pwd):/apps \
  -w /apps \
  alpine/ansible:2.17.0 \
  ansible-playbook -i inventory.ini setup.yaml
```

The playbook uses the `centos_init_setup` role. You can configure the `new_user` and `ssh_public_key` variables in `roles/centos_init_setup/defaults/main.yml`, or override them via command line (`-e "new_user=your_user"`) or host_vars. For `ssh_public_key`, you can provide the key directly or a path to a public key file (e.g., `~/.ssh/id_rsa.pub`).

## Notes

The playbook will:

*   Update the system packages
*   Create a sudo user with SSH key access
*   Harden basic SSH settings (disable root login, password authentication)
*   Configure automatic updates (by default it's off(commented out), but you can enable it by uncommenting the relevant lines in `roles/centos_init_setup/tasks/system.yml`)
*   Configure a pretty PS1 and aliases

Always test in a non-production environment first.