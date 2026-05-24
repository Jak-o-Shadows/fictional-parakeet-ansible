# fail2ban role

This role is designed for AlmaLinux / RHEL-based systems.

## Notes

- Installs `fail2ban` and `epel-release`.
- Deploys `/etc/fail2ban/jail.local` with an AlmaLinux/CentOS-style log path:
  - `logpath = /var/log/secure`
- Requires root privileges to install packages and write the configuration file.

## Usage

Ensure your playbook enables privilege escalation, for example:

```yaml
- hosts: storage
  become: true
  roles:
    - fail2ban
```

If you later add Debian/Ubuntu support, update the package and `logpath` values accordingly.
