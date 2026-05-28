# CIS L1 Debian 13 (Trixie) Hardening
## Foreword
This playbook has only been tested on a fresh installation of Debian 13, and **may potentially break installations** that already have been customised or **may potentially break applications** that have already been installed. This playbook has also **not** been tested on desktop environments but *should* work for desktop environments.

If this playbook is to be used on existing installations of Debian 13, **make a backup before applying**. 

## Excluded Sections
This playbook includes all the Automated compliance settings as much as possible. All Manual compliance settings have to be done, well, manually. This playbook also **does not** apply Level 2 compliance settings.

The entire 1.7 section is skipped as this playbook has only been tested in a headless environment. If desktop compliance settings are desired, **apply them by hand**.

## Before Using This Playbook
- **Carefully review the options in ./options/\*.yaml** before running this playbook.
- Explanations and appropriate values for each option is in each of the .yaml files for easy reference.
