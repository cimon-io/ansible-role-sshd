# Ansible SSH role

An ansible role that configures SSH daemon.

This role can be run under all versions of Ubuntu and Debian.

## Requirements

None

## Role Variables

Specify which port, protocol and IPs sshd should listen for:

```yaml
sshd_port: 22           # Which port SSH daemon will listen for
sshd_protocol: 2        # Which protocol sshd will bind to (1 or 2)

sshd_listen_address:    # Listen to some particular addresses
  - "::"                # By default, listen for all IPv6 and IPv4 addresses
  - "0.0.0.0"
```

Set security parameters:

```yaml
# Specify whether root can log in using ssh ("yes", "no", "forced-commands-only" or "without-password"):
sshd_permit_root_login: "without-password"      # Disable password authentication for root

# Specifies whether ssh should check users rights in their home directories and .rhosts files:
sshd_strict_mode: "yes"                         # Check rights (recommended)

# Specify if an empty password is allowed (not recommended):
sshd_permit_empty_passwords: "no"               # Deny empty passwords

# Specify whether authentication by password is used
sshd_password_authentication: "no"              # Disable password authentication
```

For more options, see `templates/sshd_config`.

## Dependencies

None

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: sshd
```

Variables example is given below:

```yaml
sshd_port: 224                      # Change default port to increase safety
sshd_protocol: 2
sshd_listen_address:
  - "192.168.1.1"

sshd_permit_root_login: "yes"       # Root can log in. The current global authentication scheme is used.
sshd_strict_mode: "yes"
sshd_permit_empty_passwords: "no"
sshd_password_authentication: "yes"  # Allow password authentication
```

## License

Licensed under the [MIT License](https://opensource.org/licenses/MIT).
