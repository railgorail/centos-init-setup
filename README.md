# CentOS Initial Setup (Home Lab Only)

**Русский:** [README.ru.md](README.ru.md)

This repository contains configuration files and an Ansible playbook for the initial setup of CentOS systems, leveraging a dedicated Ansible role.

⚠️ **Disclaimer:**
This setup is intended for testing and development environments only.
It is not recommended for production use. Please review, adapt, and harden any configurations before deploying to production.

## Before you run

1. **`inventory.ini`**  
   Set `ansible_host` to your server’s IP or DNS name. The example group `[servers]` and host alias (`server`) are placeholders—rename them if it helps. Set `ansible_ssh_private_key_file` to the **private** key you use for the **initial root** SSH session (it must match what you will authorize for `new_user` if you rely on the same key pair).

2. **`centos_init_setup/defaults/main.yml`**  
   Replace `new_user` (default `username`) with the account you want to create. Ensure the new user can log in by **either** setting `ssh_public_key` to the public key string **or** leaving / setting `ssh_public_key_file` to a path Ansible can read on the machine where you run the playbook. If you use the Docker command below, keys are mounted at `/root/.ssh`, so a path like `/root/.ssh/id_rsa.pub` inside the container is often clearer than `~/.ssh/...`.

3. **Sanity check**  
   Confirm you can `ssh root@<host>` with that key **before** running the playbook. The role disables root login and password authentication over SSH; after a successful run you should connect as `new_user` only.

4. **Optional**  
   Skim `centos_init_setup/tasks/ssh.yml` and `centos_init_setup/tasks/system.yml` if you need different SSH hardening or automatic updates.

## Usage

If `inventory.ini` and role defaults are ready, and you have SSH key access to root, you can run the playbook with the provided Docker command:

```bash
docker run -it --rm \
  -v ~/.ssh:/root/.ssh \
  -v $(pwd):/apps \
  -w /apps \
  alpine/ansible:2.17.0 \
  ansible-playbook -i inventory.ini setup.yaml
```

The playbook uses the `centos_init_setup` role. Override `new_user`, `ssh_public_key`, or `ssh_public_key_file` in `centos_init_setup/defaults/main.yml`, via `-e "new_user=your_user"`, or with `host_vars` / `group_vars` if you prefer.

## Notes

The playbook will:

*   Update the system packages
*   Create a sudo user with SSH key access
*   Harden basic SSH settings (disable root login, password authentication)
*   Configure automatic updates (by default it's off (commented out); enable by uncommenting the relevant lines in `centos_init_setup/tasks/system.yml`)
*   Configure a pretty PS1 and aliases

Always test in a non-production environment first.

## License

[MIT](LICENSE)