[[Linux/index|Linux/index]]

### AppArmor and SELinux:

**AppArmor**: Ubuntu, Debian, PopOS etc..
**SELinux**: Fedora, NixOS, Andriod, WSL, AlpineLinux, Arch Linux

**SELinux and AppArmor**
- both are Linux Security Modules (LSM)
- both uses Mandatory Access Control (MAC)
- both operates on top of Discretionary Access Control (DAC) which is the classic Linux permissions model: owner/group/others
- MAC can deny DAC rules
- RuleSet / Policies are not modifiable by unprivileged users

**SELinux**: label based MAC: Every process and file has a security context (label)
**AppArmor**: path based MAC: Profiles apply to programs by file paths, and rules reference filesystem paths directly

