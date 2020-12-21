# 🦾 Hardened WordPress Ansible Playbook

The `cloud-init.yml` configuration file:

- 👥 Creates a default user `dmitry` with a public key and gives it sudo permissions
- 📦 Configures and enables unattended-upgrades
- 🧱 Enables the firewall and lets SSH through it
- 🐝 Pollinates entropy using Ubuntu's server

The Ansible playbook:

- 📦 Installs and configures _MariaDB_, _Nginx_, _WordPress_, and _Certbot_
  - 🔑 Generated MariaDB _credentials_ are stored in `.credentials` directory
- 📜 Acquires Let's Encrypt _ceritificate_ using `dns-01` challenge with the configured _DNS provider_
  - 🤖 Google Cloud Platform _service account_ credentials in a JSON file ([instructions](https://cloud.google.com/iam/docs/creating-managing-service-accounts))
    - 🔧 Configure the file path as `certbot_dns.google_credentials_file`
  - ☁️ CloudFlare _API token_ ([instructions](https://developers.cloudflare.com/api/tokens/create))
    - 🔧 Configure the token as `certbot_dns.cloudflare_api_token`
  - ❗ The playbook uses Let's Encrypt _staging environment_ by default
      - 🔧 Make sure to override `certbot_server` with the production server
- 🔏 Hardens the system and its running services

## Instructions

Install [devsec.hardening](https://github.com/dev-sec/ansible-collection-hardening) collection before running:

```bash
$ ansible-galaxy collection install devsec.hardening
```

Create a `.vars.yml` file with overriden variable values:

```yaml
ssh_allow_users: john
wordpress_http_host: doe.example.com
certbot_email: john@doe.example.com
certbot_server: https://acme-v02.api.letsencrypt.org/directory
certbot_dns:
  cloudflare_api_token: 0123456789abcdef0123456789abcdef01234567
```

To use Google Cloud Platform DNS configure the service account credentials file path:
```yaml
certbot_dns:
  google_credentials_file: ~/certbot-service-account.json
```

Run the playbook:

```bash
$ ansible-playbook playbook.yml --limit <host-name> --user <remote-user> --extra-vars @.vars.yml
```
