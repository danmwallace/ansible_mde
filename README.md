# Microsoft Defender Role (ansible_mde)

This role automates the installation and configuration of Microsoft Defender for Endpoint on Linux systems. It handles package repository setup, installation, and onboarding to Microsoft Defender for Endpoint.

## Requirements

- Debian/Ubuntu-based system
- Network connectivity to Microsoft repositories
- Valid Microsoft Defender for Endpoint license
- Onboarding package from Microsoft Defender Security Center

## Role Structure

```
microsoft-defender/
├── files/
│   └── mdatp_onboard.json                       # Onboarding package from Security Center
│   └── mde_installer.sh                         # Onboarding script, available via a MS GitHub Repo. See the documentation under *Notes & References*
├── tasks/
│   └── main.yml                                 # Installation and configuration tasks
└── README.md
```

## Features

- Automated Microsoft Defender for Endpoint installation
- Microsoft repository and GPG key configuration
- Secure onboarding package deployment
- Support for modern APT repository management
- Proper file permissions and security configurations

## Usage

1. Download the onboarding package from Microsoft Defender Security Center
2. Extract the `mdatp_onboard.json` file from the `WindowsDefenderATPOnboardingPackage.zip` file you download. See [here](https://learn.microsoft.com/en-us/defender-endpoint/linux-install-with-ansible#download-the-onboarding-package-applicable-to-both-the-methods) for more information.
3. Include this role in your playbook:

```yaml
- hosts: servers
  roles:
    - role: microsoft-defender
```

**... or run the `playbooks/security.yml` playbook provided.**

## Security Notes

- APT repository keys are managed securely using modern methods
- All files are owned by root with appropriate permissions
- Repository configuration follows latest security best practices

## Installation Process

1. Checks for existing installation
2. Deploys onboarding package if not already present
3. Configures Microsoft APT repository and GPG key
4. Installs Microsoft Defender package
5. Sets up proper file permissions and ownership
6. Notifies the operator that it was successfully deployed - you will see `MDE successfully deployed` as a message in console (locally, or on Semaphore)

## Dependencies

None. This role is self-contained and does not depend on other Ansible roles.

## Compatibility

This role is tested and compatible with:
- Ubuntu 20.04+
- Debian 10+
- Other Debian-based distributions with APT package management

## Notes & References

- The new version of this role (See: `tasks/main.yml`) uses a Microsoft-provided onboarding script: https://learn.microsoft.com/en-us/defender-endpoint/linux-install-with-ansible#deploy-defender-for-endpoint-using-mde_installersh-with-ansible
- The OLD role was created following the documentation at that point in time, see: https://learn.microsoft.com/en-us/defender-endpoint/linux-install-with-ansible#deploy-defender-for-endpoint-using-ansible-by-configuring-repositories-manually
- We kept the old `main.yml` task file, now appended with `.bak`, for historical reference.