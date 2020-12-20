# 🦾 Hardened WordPress Ansible Playbook

The `cloud-init.yml` configuration file:

- 👥 Creates a default user `dmitry` with a public key and gives it sudo permissions
- 📦 Configures and enables unattended-upgrades
- 🧱 Enables the firewall and lets SSH through it
- 🐝 Pollinates entropy using Ubuntu's server

The Ansible playbook:

- 📦 Installs and configures _MariaDB_, _Nginx_, _WordPress_, and _Certbot_
  - 🔑 Generated MariaDB _credentials_ are stored in `.credentials` directory
- 📜 Acquires Let's Encrypt _ceritificate_ using `dns-01` challenge
  - 🤖 The challenge requires a Google Cloud Platform _service account_ credentials
- 🔏 Hardens the system and its running services

Install [devsec.hardening](https://github.com/dev-sec/ansible-collection-hardening) collection before running:

```bash
$ ansible-galaxy collection install devsec.hardening
```
