## What is ansible-sshd? [![Build Status](https://secure.travis-ci.org/nickjj/ansible-sshd.png)](http://travis-ci.org/nickjj/ansible-sshd)

It is an [Ansible](http://www.ansible.com/home) role to:

- Install OpenSSH Server
- Configure OpenSSH Server

## Why would you want to use this role?

Often times you want to disable root logins and password based logins. This
role sets those options by default but it also exposes most config values found
in the `sshd_config` file.

## Supported platforms

- Ubuntu 16.04 LTS (Xenial)
- Debian 8 (Jessie)
- Debian 9 (Stretch)

## Role variables

```
# To view what these variables do, check out:
# http://www.openssh.com/cgi-bin/man.cgi?query=sshd_config

sshd_port: "22"
sshd_listen_address: ["::", "0.0.0.0"]
sshd_allow_groups: []
sshd_protocol: "2"
sshd_host_keys: ["ed25519", "rsa", "ecdsa"]
sshd_kex_algorithms:
  - "curve25519-sha256@libssh.org"
  - "ecdh-sha2-nistp521"
  - "ecdh-sha2-nistp384"
  - "ecdh-sha2-nistp256"
  - "diffie-hellman-group-exchange-sha256"
sshd_ciphers:
  - "chacha20-poly1305@openssh.com"
  - "aes256-gcm@openssh.com"
  - "aes128-gcm@openssh.com"
  - "aes256-ctr"
  - "aes192-ctr"
  - "aes128-ctr"
sshd_macs:
  - "hmac-sha2-512-etm@openssh.com"
  - "hmac-sha2-256-etm@openssh.com"
  - "umac-128-etm@openssh.com"
  - "hmac-sha2-512"
  - "hmac-sha2-256"
  - "umac-128@openssh.com"
sshd_use_privilege_separation: "yes"
sshd_key_regeneration_interval: "3600"
sshd_server_key_bits: "1024"
sshd_syslog_facility: "AUTH"
sshd_log_level: "INFO"
sshd_login_grace_time: "120"
sshd_permit_root_login: "no"
sshd_strict_modes: "yes"
sshd_rsa_authentication: "yes"
sshd_pubkey_authentication: "yes"
sshd_authorized_keys_file: "%h/.ssh/authorized_keys"
sshd_ignore_rhosts: "yes"
sshd_rhosts_rsa_authentication: "no"
sshd_host_based_authentication: "no"
sshd_ignore_user_known_hosts: "no"
sshd_permit_empty_passwords: "no"
sshd_challenge_response_authentication: "no"
sshd_password_authentication: "no"
sshd_gss_api_authentication: "no"
sshd_gss_api_cleanup_credentials: "yes"
sshd_x11_forwarding: "yes"
sshd_x11_display_offset: "10"
sshd_print_motd: "no"
sshd_print_last_log: "yes"
sshd_tcp_keep_alive: "yes"
sshd_max_startups: "10:30:100"
sshd_banner: "none"
sshd_accept_env: "LANG LC_*"
sshd_subsystem: "sftp /usr/lib/openssh/sftp-server"
sshd_use_pam: "yes"

```

## Example usage

For the sake of this example let's assume you have a group called **app** and
you have a typical `site.yml` file.

To use this role edit your `site.yml` file to look something like this:

```
---

- name: "Configure app server(s)"
  hosts: "app"
  become: True

  roles:
    - { role: "nickjj.sshd", tags: "sshd" }
```

Let's say you want to white list only users belonging to the `deploy` group, you
can do this by opening or creating `group_vars/app.yml` which is located
relative to your `inventory` directory and then make it look something like this:

```
---

sshd_allow_groups = ["deploy"]
```

Now you would run `ansible-playbook -i inventory/hosts site.yml -t sshd`.

## Installation

`$ ansible-galaxy install nickjj.sshd`

## Ansible Galaxy

You can find it on the official
[Ansible Galaxy](https://galaxy.ansible.com/nickjj/sshd/) if you want to rate it.

## License

MIT
