# CIS L1 Debian 13 (Trixie) Hardening
## Foreword
This playbook has only been tested on a fresh installation of Debian 13, and **may potentially break installations** that already have been customised or **may potentially break applications** that have already been installed. This playbook has also **not** been tested on desktop environments but *should* work for desktop environments.

If this playbook is to be used on existing installations of Debian 13, **make a backup before applying**. 

## What is Included/Excluded
This playbook includes all the Automated compliance settings as much as possible. Most of the Manual compliance settings have to be done manually. This playbook also **does not** include compliance settings for Server Level 2 with a few exceptions, with options provided to either enable or disable them. All inclusions/exclusions to this are documented as follows.

Companion scripts are available [in this GitHub repository](https://github.com/ProbablyBread/CIS-L1-Debian13-Audit) to purely audit those sections that have been excluded due to difficulty conforming to the compliance requirements with an Ansible playbook. Sections that are checked by the companion script are marked with (AUDIT SCRIPT). Note the script only performs auditing and does not perform remediation.

### Excluded Automated Settings

#### Section 1.7
The entire 1.7 section is skipped as this playbook is meant for use in a headless environment. If compliance settings for the desktop environment are desired, please apply them manually.

#### Section 2.3
All `chronyd` settings are excluded. This playbook specifically uses `systemd-timesyncd` for clock synchronisation since it's the default clock synchronisation service on Debian. If necessary/desired, please configure another timesync service manually.

#### Section 5.2.5 (AUDIT SCRIPT)
Both the default `/etc/sudoers` and the `/etc/sudoers.d/99-cis` file provided by this playbook doesn't include `!authenticate` by default. This section is skipped due to the difficulty in conforming to the correct sudoers file syntax using Ansible.

#### Section 5.4.1.6 (AUDIT SCRIPT)
Although this section is marked Automated, it is better to leave this as a manual check. The remediation entails locking, expiring, or resetting the passwords of any user accounts that have their last password change dates set to the future, which would require manual intervention anyway.

#### Section 5.4.2 (AUDIT SCRIPT)
Sections 5.4.2.1 to 5.4.2.5 are not implemented. Only sections 5.4.2.6 to 5.4.2.8 are implemented.

The compliance settings from 5.4.2.1 to 5.4.2.3 states that any other accounts other than `root` are not supposed to have a UID or GID of 0. Remediating such accounts if they exist would require manual configuration.

The compliance setting for 5.4.2.4 states that the `root` account must have a password or is locked. Locking or setting a password for the root account should be done manually. 

The compliance setting for 5.4.2.5 states that the `root` account's `PATH` must not include unsafe directories such as non-`root` owned directories or invalid directories etc. By default, the `PATH` for root only contains `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`. Since overriding `PATH` requires editing `.bashrc` or `.profile`, or manually using `export PATH`, this setting should be manually checked.

#### Section 6.1.1.2/6.1.2 (AUDIT SCRIPT)
All remote syslog configurations are excluded. With configurations varying widely across different systems with different syslogging needs, remote syslogging options would be better controlled using a separate playbook that defines what is to be forwarded. However, this playbook also provides a convenience option to enable or disable the forwarding of `journald` logs.

#### Section 7.1.11/7.1.12 (AUDIT SCRIPT)
World writable files and directories needs to be secured manually since there is no way to know if a file/directory requires world write permissions. Orphaned files and directories should also be removed manually to prevent potential data loss from removing directories/files that may still be required even though they are orphaned.

#### Section 7.2.3 (AUDIT SCRIPT)
Groups that exist in `/etc/passwd` that do not exist in `/etc/group` should be manually remediated by either removing the user or adding the respective group.

#### Section 7.2.4 (partial) (AUDIT SCRIPT)
This playbook only ensures that the `shadow` group does not have any users explicitly added to the group. Remediation has to be done manually if there are any users that have the `shadow` group as their primary group.

#### Section 7.2.5/7.2.6/7.2.7/7.2.8 (AUDIT SCRIPT)
The compliance setting for these sections ensure that all UIDs and GIDs are unique on the system. Remediation of files that are assigned to users that had the same UID/GID has to be done manually after changing the UIDs or GIDs. Similarly for duplicate usernames and group names, remediation should be performed manually to rename the affected user(s)/group(s). 

#### Section 7.2.9 (partial) (AUDIT SCRIPT)
This playbook only ensures that the home directory of all interactive users on the system are owned by the respective users, and have their permissions set to 0750. Remediation has to be done manually if there are any users who have missing or nonexistent home directories.

#### Section 7.2.10 (AUDIT SCRIPT)
Permissions on dot files for interactive users should be configured manually since different permissions may be required for different dot files (e.g. `.bash_history` vs `.vimrc`. There is a need to manually review each dot file and ensure that changing their permissions do not result in overly open permissions.

### Included L2/Manual Settings
- Section 3.1.1 - Enables or disables IPv6 (see `./options/3.1_network_options.yaml`)
- Section 5.3.3.2.3 - Configures password complexity requirements (see `./options/5.3_pam_options.yaml`)
- Section 5.4.1.2 - Configures how long in days before a user can reset their password (see `./options/5.4_shadow_options.yaml`)
- Section 6.1.1.1.3 - Configures `journald` log rotation (see `./options/6.1_logging_options.yaml`)

## Before Using This Playbook
- **Carefully review the options in `./options/\*.yaml`** before running this playbook.
- Explanations and appropriate values for each option is in each of the .yaml files for easy reference.

## Using This Playbook
- Once you have the options configured, the playbook by default runs against the localhost without an inventory file supplied.
- Tags are provided for each major section (e.g. entirety of 1.1 or 2.2 etc) if only specific sections are to be applied.
- Tags for individual sub-sections (e.g. 1.1.1 etc) are not supplied.
