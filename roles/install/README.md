<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.postfix.install
Version: 1.0.1

This role installs and does initial configuration for Postfix.

## Requirements

| Platform | Versions |
| -------- | -------- |
| Debian | <ul><li>bullseye</li><li>bookworm</li></ul> |
| EL | <ul><li>8</li><li>9</li></ul> |
| Ubuntu | <ul><li>focal</li><li>jammy</li><li>noble</li></ul> |

## Dependencies

None.

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| postfix_configure_firewall | <p>Whether to configure the firewall to allow SMTP traffic.</p> | bool | no |  | True |
| postfix_firewall_type | <p>The type of firewall to configure.</p><p>On EL and Debian systems, this defaults to `firewalld`.</p><p>On Ubuntu systems, this defaults to `ufw`.</p> | str | no | <ul><li>firewalld</li><li>ufw</li></ul> |  |
| postfix_compatibility_level | <p>The level of backward compatibility to maintain with older versions of Postfix.</p><p>On EL systems, this defaults to `2`.</p><p>On Debian-based systems, this defaults to the package major version (e.g. package version `3.7.11-0` would default to a compatibility level of `3.7`).</p> | float | no |  |  |
| postfix_soft_bounce_enabled | <p>Whether to enable soft bounce to keep messages in the queue for later delivery.</p><p>This is useful for debugging and troubleshooting.</p> | bool | no |  | False |
| postfix_queue_directory | <p>The directory where Postfix stores its mail queue.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /var/spool/postfix |
| postfix_command_directory | <p>The directory where Postfix stores its command binaries.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /usr/sbin |
| postfix_daemon_directory | <p>The directory where Postfix stores its daemon binaries.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p><p>On EL systems, this defaults to `/usr/libexec/postfix`.</p><p>On Debian-based systems, this defaults to `/usr/lib/postfix/sbin`.</p> | path | no |  |  |
| postfix_data_directory | <p>The directory where Postfix stores its data files.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /var/lib/postfix |
| postfix_mail_owner | <p>The user that owns the Postfix mail queue and data directories.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | str | no |  | postfix |
| postfix_default_privs | <p>The default privileges for the Postfix daemon.</p><p>If not specified, this will default to `nobody`.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | str | no |  |  |
| postfix_myhostname | <p>The FQDN of the mail server used as default value for other settings in Postfix.</p> | str | no |  | {{ ansible_fqdn }} |
| postfix_mydomain | <p>The domain name of the mail server used as default value for other settings in Postfix.</p> | str | no |  | {{ ansible_domain | default('localdomain') }} |
| postfix_myorigin | <p>The domain name that Postfix uses as the origin for mail sent from the server.</p> | str | no |  | {{ ansible_domain | default('localdomain') }} |
| postfix_inet_interfaces_loopback_only | <p>Whether Postfix should listen on the loopback interface only.</p><p>If set to `true`, Postfix will listen on the loopback interface only and ignore the `postfix_inet_interfaces_all` and `postfix_inet_interfaces` options.</p><p>If set to `false`, the `postfix_inet_interfaces_all` and `postfix_inet_interfaces` option will be used.</p> | bool | no |  | True |
| postfix_inet_interfaces_all | <p>Whether Postfix should listen on all interfaces.</p><p>If *postfix_inet_interfaces_loopback_only* is set to `true`, this option is ignored.</p><p>If set to `true`, Postfix will listen on all interfaces and ignore the `postfix_inet_interfaces` options.</p><p>If set to `false`, `postfix_inet_interfaces` option will be used.</p> | bool | no |  | False |
| postfix_inet_interfaces | <p>The list of network interface IP addresses that Postfix should listen on.</p><p>If *postfix_inet_interfaces_all* or *postfix_inet_interfaces_loopback_only* is set to `true`, this option is ignored.  Otherwise, it is required.</p> | list of 'str' | no |  |  |
| postfix_inet_protocols | <p>The IP address protocols that Postfix should use.</p><p>If set to `all`, Postfix will use both IPv4 and IPv6.</p> | str | no | <ul><li>all</li><li>ipv4</li><li>ipv6</li><li>ipv4, ipv6</li></ul> | all |
| postfix_proxy_interfaces | <p>The list of network interface IP addresses that Postfix should consider as proxy interfaces.</p><p>If not specified, Postfix will not consider any interfaces as proxy interfaces.</p> | list of 'str' | no |  |  |
| postfix_mydestination | <p>The list of domains that Postfix considers local and will not relay upstream.</p><p>The values `$myhostname` and `$mydomain` are replaced with the values of the `postfix_myhostname` and `postfix_mydomain` options, respectively.</p> | list of 'str' | no |  | ['$myhostname', 'localhost.$mydomain', 'localhost'] |
| postfix_local_recipient_maps | <p>The list of maps that Postfix uses to determine if a recipient is local.</p><p>If not specified, Postfix will not use any maps to determine local recipients.</p> | list of 'str' | no |  |  |
| postfix_unknown_local_recipient_reject_code | <p>The SMTP error code that Postfix should return when a message is sent to an unknown local recipient.</p> | int | no |  | 550 |
| postfix_mynetworks_style | <p>Defines which networks Postfix considers trusted by default.</p><p>If set to `host`, only the local machine is trusted.</p><p>If set to `subnet`, all machines on the local subnet are trusted.</p><p>If set to `class`, all machines in the same class A/B/C network are trusted.</p> | str | no | <ul><li>host</li><li>subnet</li><li>class</li></ul> | host |
| postfix_mynetworks | <p>The list of IP addresses or CIDR ranges that Postfix considers trusted.</p><p>This is list is used in conjunction with the `postfix_mynetworks_style` option.</p> | list of 'str' | no |  | ['127.0.0.0/8'] |
| postfix_relay_domains | <p>The list of domains that Postfix should relay mail for.</p><p>If not specified, Postfix will not relay mail for any domains.</p> | list of 'str' | no |  |  |
| postfix_smtpd_recipient_restrictions | <p>The list of restrictions that Postfix should apply to incoming mail recipients.</p><p>If not specified, Postfix will not apply any restrictions to incoming mail recipients.</p> | list of 'str' | no |  | ['permit_mynetworks', 'permit_sasl_authenticated', 'reject_unauth_destination', 'reject_invalid_hostname', 'reject_non_fqdn_hostname', 'reject_non_fqdn_sender', 'reject_non_fqdn_recipient', 'reject_unknown_sender_domain', 'reject_unknown_recipient_domain', 'reject_rbl_client sbl.spamhaus.org', 'reject_rbl_client cbl.abuseat.org', 'reject_rbl_client dul.dnsbl.sorbs.net', 'permit'] |
| postfix_smtpd_sender_restrictions | <p>The list of restrictions that Postfix should apply to outgoing mail senders.</p><p>If not specified, Postfix will not apply any restrictions to outgoing mail senders.</p> | list of 'str' | no |  | ['reject_unknown_sender_domain'] |
| postfix_use_relay_recipient_map_file | <p>Whether Postfix should use a relay recipient map file to determine if a recipient is a relay recipient.</p> | bool | no |  | False |
| postfix_in_flow_delay | <p>The delay that Postfix should use when a message is received from a client.</p> | str | no |  | 1s |
| postfix_use_alias_map_file | <p>Whether Postfix should use an alias map file to determine if a recipient is an alias.</p><p>If set to `true`, Postfix will add `hash:/etc/aliases` to the `alias_maps` configuration.</p> | bool | no |  | True |
| postfix_use_nis_alias_map | <p>Whether Postfix should use NIS aliases.</p><p>If set to `true`, Postfix will add `nis:mail.aliases` to the `alias_maps` configuration.</p> | bool | no |  | True |
| postfix_custom_alias_maps | <p>The list of custom alias maps that Postfix should use.</p> | list of 'str' | no |  | [] |
| postfix_use_header_checks_file | <p>Whether Postfix should use a header checks file to determine if a message header is valid.</p> | bool | no |  | False |
| postfix_custom_header_checks | <p>The list of custom header checks that Postfix should use.</p> | list of 'str' | no |  | [] |
| postfix_smtpd_banner | <p>The banner that Postfix should display when a client connects to the SMTP server.</p> | str | no |  | $myhostname ESMTP $mail_name |
| postfix_debug_peer_level | <p>The level of debugging that Postfix should use for peer connections.</p> | int | no |  | 2 |
| postfix_sendmail_path | <p>The path to the sendmail binary that Postfix should use.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /usr/sbin/sendmail.postfix |
| postfix_newaliases_path | <p>The path to the newaliases binary that Postfix should use.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /usr/bin/newaliases.postfix |
| postfix_mailq_path | <p>The path to the mailq binary that Postfix should use.</p><p>Unless you have a good reason to change this, you should leave it at the default.</p> | path | no |  | /usr/bin/mailq.postfix |
| postfix_setgid_group | <p>The group that Postfix should use to set the group ID.</p> | str | no |  | postdrop |
| postfix_html_directory | <p>The directory where Postfix stores its HTML documentation.</p> | path | no |  |  |
| postfix_manpage_directory | <p>The directory where Postfix stores its manual pages.</p> | path | no |  | /usr/share/man |
| postfix_sample_directory | <p>The directory where Postfix stores its sample configuration files.</p> | path | no |  | /usr/share/doc/postfix/samples |
| postfix_readme_directory | <p>The directory where Postfix stores its README files.</p> | path | no |  |  |
| postfix_smtpd_tls_mandatory_sslv2_enabled | <p>Whether Postfix should require SSLv2 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_mandatory_sslv3_enabled | <p>Whether Postfix should require SSLv3 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_mandatory_tlsv1_enabled | <p>Whether Postfix should require TLSv1 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_mandatory_tlsv1_1_enabled | <p>Whether Postfix should require TLSv1.1 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_mandatory_tlsv1_2_enabled | <p>Whether Postfix should require TLSv1.2 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | True |
| postfix_smtpd_tls_mandatory_tlsv1_3_enabled | <p>Whether Postfix should require TLSv1.3 for incoming SMTP mandatory TLS connections.</p> | bool | no |  | True |
| postfix_smtpd_tls_sslv2_enabled | <p>Whether Postfix should allow SSLv2 for incoming SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_sslv3_enabled | <p>Whether Postfix should allow SSLv3 for incoming SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_tlsv1_enabled | <p>Whether Postfix should allow TLSv1 for incoming SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_tlsv1_1_enabled | <p>Whether Postfix should allow TLSv1.1 for incoming SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_tlsv1_2_enabled | <p>Whether Postfix should allow TLSv1.2 for incoming SMTP TLS connections.</p> | bool | no |  | True |
| postfix_smtpd_tls_tlsv1_3_enabled | <p>Whether Postfix should allow TLSv1.3 for incoming SMTP TLS connections.</p> | bool | no |  | True |
| postfix_smtp_tls_mandatory_sslv2_enabled | <p>Whether Postfix should require SSLv2 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_mandatory_sslv3_enabled | <p>Whether Postfix should require SSLv3 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_mandatory_tlsv1_enabled | <p>Whether Postfix should require TLSv1 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_mandatory_tlsv1_1_enabled | <p>Whether Postfix should require TLSv1.1 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_mandatory_tlsv1_2_enabled | <p>Whether Postfix should require TLSv1.2 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | True |
| postfix_smtp_tls_mandatory_tlsv1_3_enabled | <p>Whether Postfix should require TLSv1.3 for outgoing SMTP mandatory TLS connections.</p> | bool | no |  | True |
| postfix_smtp_tls_sslv2_enabled | <p>Whether Postfix should allow SSLv2 for outgoing SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_sslv3_enabled | <p>Whether Postfix should allow SSLv3 for outgoing SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_tlsv1_enabled | <p>Whether Postfix should allow TLSv1 for outgoing SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_tlsv1_1_enabled | <p>Whether Postfix should allow TLSv1.1 for outgoing SMTP TLS connections.</p> | bool | no |  | False |
| postfix_smtp_tls_tlsv1_2_enabled | <p>Whether Postfix should allow TLSv1.2 for outgoing SMTP TLS connections.</p> | bool | no |  | True |
| postfix_smtp_tls_tlsv1_3_enabled | <p>Whether Postfix should allow TLSv1.3 for outgoing SMTP TLS connections.</p> | bool | no |  | True |
| postfix_smtpd_tls_enabled | <p>Whether Postfix should enable TLS for incoming SMTP connections.</p> | bool | no |  | False |
| postfix_smtpd_tls_cert_file | <p>The path to the certificate file that Postfix should use for incoming SMTP TLS connections.</p><p>If *postfix_smtpd_tls_enabled* is set to `true`, this is required.  Otherwise, it is ignored.</p> | path | no |  |  |
| postfix_smtpd_tls_key_file | <p>The path to the private key file that Postfix should use for incoming SMTP TLS connections.</p><p>If *postfix_smtpd_tls_enabled* is set to `true`, this is required.  Otherwise, it is ignored.</p> | path | no |  |  |
| postfix_smtpd_tls_security_level | <p>The security level that Postfix should use for incoming SMTP TLS connections.</p><p>If *postfix_smtpd_tls_enabled* is set to `false`, this is ignored.</p> | str | no | <ul><li>none</li><li>may</li><li>encrypt</li></ul> | may |
| postfix_smtp_tls_ca_path | <p>The path to the CA certificate directory that Postfix should use for outgoing SMTP TLS connections.</p> | path | no |  |  |
| postfix_smtp_tls_ca_file | <p>The path to the CA certificate file that Postfix should use for outgoing SMTP TLS connections.</p> | path | no |  |  |
| postfix_smtp_tls_security_level | <p>The security level that Postfix should use for outgoing SMTP TLS connections.</p> | str | no | <ul><li>none</li><li>may</li><li>encrypt</li><li>dane</li><li>dane-only</li><li>fingerprint</li><li>verify</li><li>secure</li></ul> | may |
| postfix_use_transport_map_file | <p>Whether Postfix should use a transport map file to determine the transport for a destination.</p> | bool | no |  | False |
| postfix_custom_main_cf | <p>Custom configuration for the `main.cf` file.</p> | str | no |  |  |
| postfix_smtp_listening_port | <p>The port that Postfix should listen on for incoming SMTP connections.</p> | int | no |  | 25 |
| postfix_master_process_config | <p>The list of services and their configurations for the `master.cf` file.</p> | list of dicts of 'postfix_master_process_config' options | no |  | [{'service': "{{ 'smtp' if postfix_smtp_listening_port | int == 25 else postfix_smtp_listening_port }}", 'type': 'inet', 'private': False, 'command': 'smtpd'}, {'service': 'pickup', 'type': 'unix', 'private': False, 'wakeup_time': 60, 'max_processes': 1, 'command': 'pickup'}, {'service': 'cleanup', 'type': 'unix', 'private': False, 'max_processes': 0, 'command': 'cleanup'}, {'service': 'qmgr', 'type': 'unix', 'private': False, 'wakeup_time': 300, 'max_processes': 1, 'command': 'qmgr'}, {'service': 'tlsmgr', 'type': 'unix', 'wakeup_time': '1000?', 'max_processes': 1, 'command': 'tlsmgr'}, {'service': 'rewrite', 'type': 'unix', 'command': 'trivial-rewrite'}, {'service': 'bounce', 'type': 'unix', 'command': 'bounce'}, {'service': 'defer', 'type': 'unix', 'max_processes': 0, 'command': 'bounce'}, {'service': 'trace', 'type': 'unix', 'command': 'bounce'}, {'service': 'verify', 'type': 'unix', 'max_processes': 1, 'command': 'verify'}, {'service': 'flush', 'type': 'unix', 'private': False, 'wakeup_time': '1000?', 'max_processes': 0, 'command': 'flush'}, {'service': 'proxymap', 'type': 'unix', 'command': 'proxymap'}, {'service': 'proxywrite', 'type': 'unix', 'max_processes': 1, 'command': 'proxymap'}, {'service': 'smtp', 'type': 'unix', 'command': 'smtp'}, {'service': 'relay', 'type': 'unix', 'command': 'smtp -o syslog_name=postfix/$service_name'}, {'service': 'showq', 'type': 'unix', 'private': False, 'command': 'showq'}, {'service': 'error', 'type': 'unix', 'command': 'error'}, {'service': 'retry', 'type': 'unix', 'command': 'error'}, {'service': 'discard', 'type': 'unix', 'command': 'discard'}, {'service': 'local', 'type': 'unix', 'unprivileged': False, 'command': 'local'}, {'service': 'virtual', 'type': 'unix', 'unprivileged': False, 'command': 'virtual'}, {'service': 'lmtp', 'type': 'unix', 'command': 'lmtp'}, {'service': 'anvil', 'type': 'unix', 'max_processes': 1, 'command': 'anvil'}, {'service': 'scache', 'type': 'unix', 'max_processes': 1, 'command': 'scache'}, {'service': 'postlog', 'type': 'unix', 'private': False, 'max_processes': 1, 'command': 'postlogd'}] |
| postfix_aliases | <p>The list of aliases that Postfix should use.</p> | list of dicts of 'postfix_aliases' options | no |  | [] |

### Options for postfix_master_process_config
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| service | <p>The name of the service.</p> | str | yes |  |  |
| type | <p>The type of service.</p> | str | yes | <ul><li>inet</li><li>unix</li><li>unix-dgram</li><li>pass</li></ul> |  |
| private | <p>Whether the service is private.</p> | bool | no |  | True |
| unprivileged | <p>Whether the service is unprivileged.</p> | bool | no |  | True |
| chroot | <p>Whether the service is chrooted.</p> | bool | no |  | False |
| wakeup_time | <p>The wakeup time for the service.</p> | str | no |  |  |
| max_processes | <p>The maximum number of processes for the service.</p> | int | no |  |  |
| command | <p>The command for the service.</p><p>This should include any arguments that the command requires.</p> | str | yes |  |  |

### Options for postfix_aliases
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The alias name.</p> | str | yes |  |  |
| destination | <p>The alias destination.</p> | str | yes |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
