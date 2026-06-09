# CIS L1 Debian 13 (Trixie) Hardening
## Warning
This is a work in progress playbook, which means:
1. Your system is quite likely to break
2. Things will definitely be changing
3. You use this at your own risk

## Foreword
This playbook has only been tested on a fresh installation of Debian 13, and **may potentially break installations** that already have been customised or **may potentially break applications** that have already been installed. This playbook has also **not** been tested on desktop environments but *should* work for desktop environments.

If this playbook is to be used on existing installations of Debian 13, **make a backup before applying**. 

## Excluded Sections
This playbook includes all the Automated compliance settings as much as possible. Most if not all of the Manual compliance settings have to be done manually. This playbook also **does not** apply compliance settings for Server Level 2.

### Section 1.7
The entire 1.7 section is skipped as this playbook is meant for use in a headless environment. If compliance settings for the desktop environment are desired, please apply them manually.

### Section 5.2.5
Both the default `/etc/sudoers` and the `/etc/sudoers.d/99-cis` file provided by this playbook doesn't include !authenticate by default. This section is skipped due to the difficulty in conforming to the correct sudoers file syntax using Ansible.

### Section 5.4.1.6
Although this section is marked Automated, it is better to leave this as a manual check. The remediation entails locking, expiring, or resetting the passwords of any user accounts that have their last password change dates set to the future, which would require manual intervention anyway.

### Section 5.4.2
Sections 5.4.2.1 to 5.4.2.5 are not implemented. Only sections 5.4.2.6 to 5.4.2.8 are implemented.

The compliance settings from 5.4.2.1 to 5.4.2.3 states that any other accounts other than `root` are not supposed to have a UID or GID of 0. Remediating such accounts if they exist would require manual configuration.

The compliance setting for 5.4.2.4 states that the `root` account must have a password or is locked. Locking or setting a password for the root account should be done manually. 

The compliance setting for 5.4.2.5 states that the `root` account's `PATH` must not include unsafe directories such as non-`root` owned directories or invalid directories etc. By default, the `PATH` for root only contains `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`. Since overriding `PATH` requires editing `.bashrc` or `.profile`, or manually using `export PATH`, this setting should be manually checked.

## Before Using This Playbook
- **Carefully review the options in ./options/\*.yaml** before running this playbook.
- Explanations and appropriate values for each option is in each of the .yaml files for easy reference.

## Using This Playbook
- Once you have the options configured, the playbook by default runs against the localhost without an inventory file supplied.
- Tags are provided for each major section (e.g. entirety of 1.1 or 2.2 etc) if only specific sections are to be applied.
