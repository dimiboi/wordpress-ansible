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
  - 🤖 The challenge requires either a Google Cloud Platform _service account_ credentials stored in a file and configured as `certbot_dns.google_credentials_file` or CloudFlare _API token_ configured as `certbot_dns.cloudflare_api_token`
  - ❕ The playbook uses Let's Encrypt _staging environment_ by default; make sure to override `certbot_server` with the production server
- 🔏 Hardens the system and its running services

Install [devsec.hardening](https://github.com/dev-sec/ansible-collection-hardening) collection before running:

```bash
$ ansible-galaxy collection install devsec.hardening
```

Create a `.vars.yml` file with overriden `ssh_allow_users`, `wordpress_http_host`, `certbot_email`, `certbot_dns`, and `certbot_server` variables and run the playbook:

```bash
$ ansible-playbook playbook.yml --limit <host-name> --user <remote-user> --extra-vars @.vars.yml
```
