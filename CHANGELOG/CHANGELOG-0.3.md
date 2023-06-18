# v0.3

## Changes by Kind

### New Feature

- Add vmware_desktop and parallels vagrant providers
- Manage HA k8s deployment with yakir repository (https://github.com/ricofehr/yakir)

### Bug Fix

- Fix libvirt storage management with recent version of vagrant / libvirt plugin

### Improvement

- Update Openstack Ansible release : deploy Openstack Zed
- Update k8s repository : deploy kubernetes v1.27.2
- Update Ubuntu box on 20.04
- Add Ubuntu 22.04 Image for Cinder images storage
- Large review and rework for Ansible Part : add linter, and improve global quality code

- Use variable for define disk persistence needs on helm deployments
- Use ansible helm collection to deploy helm chart, instead of shell commands

### Other
