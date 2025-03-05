# Changelog

All notable changes to this project will be documented in this file.

## [1.1.4] - 2025-01-10

### Role - install

- Added support for PostgreSQL 17.
- Removed default value for the `pgsql_major_version` variable. This was done to prevent unintentional upgrades.

## [1.1.3] - 2025-01-08

### Collection

- Added Changelog.
- Updated collection README documentation.

## [1.1.2] - 2024-09-23

### Role - install

- Clarified documentation for the `pgsql_configure_firewall` and `pgsql_configure_monitoring` variables.

## [1.1.1] - 2024-09-06

### Role - install

- Added `no_log` option to the `pgsql_monitoring_password` variable.

## [1.1.0] - 2024-09-05

### Role - install

- Removed database and user management tasks from the role.

## [1.0.7] - 2024-08-30

### Role - install

- Fixed a typo in task name.

## [1.0.6] - 2024-08-09

### Collection

- Minimum Ansible version changed from `2.14` to `2.15` due to EOL status.

## [1.0.5] - 2024-08-06

### Role - install

- Updated documentation, role metadata, and templates for readability.
- Replaced `pgsql_configure_firewalld` and `pgsql_configure_ufw` variables with the `pgsql_configure_firewall` and `pgsql_firewall_type` variables. This allows all firewall configuration to be enabled/disabled in one variable and the default firewall type overridden in one variable.
- Refactored the validation of variables.

## [1.0.4] - 2024-07-02

### Role - install

- Added step to install `gnupg` package on Debian-based systems when installing TimescaleDB.

## [1.0.3] - 2024-07-02

### Role - install

- Updated documentation, role metadata, and templates for readability.

## [1.0.2] - 2024-06-29

### Role - install

- Changed references to Red Hat Enterprise Linux (RHEL) to more accurately reference Enterprise Linux (EL) to convey the intention to support derivatives (Rocky/AlmaLinux/etc.)

## [1.0.1] - 2024-06-28

### Role - install

- Removed support for version 12.

## [1.0.0] - 2024-06-28

### Collection

- Initial release.
- *install* role added.
