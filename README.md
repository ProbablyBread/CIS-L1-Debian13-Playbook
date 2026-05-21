# CIS L1 Debian 13 (Trixie) Hardening
## Prerequisites
- Carefully review the options in ./options/kernel_module_options.yaml and ensure that you are not using the filesystems listed within.
- Carefully review the options in ./options/fs_options.yaml and ensure that your partitions are set correctly
    - This playbook assumes that you **do not** have partitioning done by default.
- Replace the bootloader username and password in ./options/bootloader_options.yaml
    - The password can be generated with grub-mkpasswd-pbkdf2.
