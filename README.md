# Ansible SSH role

An ansible role that configures SSH daemon.

This role can be run under all versions of Ubuntu and Debian.

## Requirements

None

## Role Variables

Specify which port, protocol, address family and IPs sshd should listen for:

```yaml
sshd_port: 22              # Which port SSH daemon will listen for
sshd_protocol: 2           # Which protocol sshd will bind to (1 or 2)
sshd_address_family: "any" # Use "inet" for IPv4 only or "inet6" for IPv6 only
sshd_listen_address:       # Listen to some particular addresses
  - "::"                   # By default, listen for all IPv6 and IPv4 addresses
  - "0.0.0.0"
```

The next variable specifies whether sshd should separate privileges by creating an unprivileged child process to deal with incoming network traffic. The argument can be "yes", "no" or "sandbox".

```yaml
sshd_use_privilege_separation: "yes"  # Prevent privilege escalation, for sshd version < 7.6
```

The variable `sshd_syslog_facility` defines the facility code that is used for logging messages from sshd. The possible values are: DAEMON, USER, AUTH, LOCAL0, LOCAL1, LOCAL2, LOCAL3, LOCAL4, LOCAL5, LOCAL6 and LOCAL7. The verbosity level of sshd logging messages is set by the `sshd_log_level` variable.

```yaml
sshd_syslog_facility: "AUTH"  # Logging security/authorization messages
sshd_log_level: "INFO"        # Informational messages
```

The ephemeral server key parameters can be specified by the following variables. Set `sshd_key_regeneration_interval` to 0 to prevent the key regeneration. Use the variables for sshd version < 7.6 only.

```yaml
sshd_key_regeneration_interval: 3600  # Regenerate the key after this number of seconds
sshd_server_key_bits: 1024            # Number of bits in the key (512 or more)
```

The variable `sshd_login_grace_time` sets time after which server disconnects if the user has not successfully logged in.

```yaml
sshd_login_grace_time: 120    # Disconnect in 120 seconds
```

Specify whether root can log in using ssh ("yes", "no", "forced-commands-only" or "without-password"):

```yaml
sshd_permit_root_login: "without-password"  # Disable password authentication for root
```

The variable `sshd_strict_mode` sets whether ssh should check users rights in their home directories and .rhosts files:

```yaml
sshd_strict_mode: "yes"    # Check rights (recommended)
```

Use the following parameters to configure the maximum number of authentication attempts and open sessions permitted per connection.

```yaml
sshd_max_auth_tries: 6
sshd_max_sessions: 10
```

The next variable defines whether public key authentication is allowed.

```yaml
sshd_pubkey_authentication: "yes"
```

To set up the host-based authentication use the following variables. The variable `sshd_hostbased_authentication` specifies whether rhosts authentication along with successful public key client host authentication is allowed. If `sshd_ignore_rhosts` is set to "yes", .rhosts and .shosts files won't be used in host-based authentication.

```yaml
sshd_hostbased_authentication: "no"
sshd_ignore_rhosts: "yes"
```

Specify whether authentication by password is used by the following variable. You can also set whether an empty password and challenge-response authentication are allowed.

```yaml
sshd_password_authentication: "no"           # Disable password authentication
sshd_permit_empty_passwords: "no"            # Deny empty passwords (recommended)
sshd_challenge_response_authentication: "no" # Deny challenge-response authentication
```

The next set of variables defines Kerberos authentication parameters:

```yaml
# Validate the password through the Kerberos KDC
sshd_kerberos_authentication: "no"
# Acquire an AFS token before accessing the user's home directory
sshd_kerberos_get_AFS_token: "no"
# If Kerberos authentication fails, use any other local mechanism
sshd_kerberos_or_local_passwd: "yes"
# Automatically destroy the user's ticket cache file on logout
sshd_kerberos_ticket_cleanup: "yes"
```

You are able to allow user's authentication based on GSSAPI and destroy the credentials cache automatically on logout if necessary:

```yaml
sshd_GSSAPI_authentication: "no"
sshd_GSSAPI_cleanup_credentials: "yes"
```

Specify whether X11 forwarding is permitted:

```yaml
sshd_x11forwarding: "no"  # Disable X11 forwarding
```

Set up what should be printed when a user logs in interactively:

```yaml
sshd_print_motd: "no"       # Wether sshd should print /etc/motd
sshd_print_last_log: "yes"  # Print the date and time of the last login
```

Use the next variable if the system should send TCP keepalive messages to the other side:

```yaml
sshd_TCP_keepalive: "yes"
```

You can specify the content of which file is sent to the remote user before authentication is allowed:

```yaml
sshd_banner: "none"   # If "none", no banner is displayed
```

You can allow and deny login for particular user name patterns listed with a space by the following variables. If `sshd_allow_users` is specified, login is allowed only for user names that match one of the patterns. If `sshd_deny_users` is set, login is disallowed for all user names which match at least one of them. The allow/deny directives are processed in the following order: at first "DenyUsers", then "AllowUsers".

```yaml
sshd_allow_users: []  # A list of user name patterns to allow access
sshd_deny_users: []   # A list of user name patterns to deny access
```

The variable `sshd_host_key` specifies a file containing a private host key used by SSH.

```yaml
sshd_host_key:
  - "/etc/ssh/ssh_host_rsa_key"
  - "/etc/ssh/ssh_host_ecdsa_key"
  - "/etc/ssh/ssh_host_ed25519_key"
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

sshd_permit_root_login: "yes"       # Root can log in. The current global authentication scheme is used
sshd_strict_mode: "yes"
sshd_permit_empty_passwords: "no"
sshd_password_authentication: "yes"  # Allow password authentication
```

## License

Licensed under the [MIT License](https://opensource.org/licenses/MIT).
