# FIREWALL_VM

This role prepares a host to run an OpenWRT firewall/router VM using libvirt.

It installs and starts the required virtualization stack, downloads an OpenWRT image when configured, and can define/start a VM with two virtual NICs.

Note that services from the host are put on the LAN side.

## Assumptions

- The host is an AlmaLinux/RHEL-style system.
- The host has two NICs or libvirt networks configured for WAN/LAN connectivity.
- `firewall_vm_define: true` must be set to actually create/start the VM.
- OpenWRT configuration inside the VM is handled separately.

## Variables

- `firewall_vm_define`: whether to define and start the VM (default: `false`)
- `firewall_vm_name`: VM name
- `firewall_vm_memory`: RAM in MiB
- `firewall_vm_vcpus`: number of virtual CPUs
- `firewall_vm_disk_size_gb`: disk size for the VM disk
- `firewall_vm_image_url`: URL to download the OpenWRT image
- `firewall_vm_image_checksum`: Checksum of the image (can be omitted)
- `firewall_vm_image_path`: local path where the OpenWRT image is stored
- `firewall_vm_networks`: list of libvirt network definitions for the VM

## Example usage

```yaml
- hosts: firewall-host
  become: true
  roles:
    - role: firewall_vm
      vars:
        firewall_vm_define: true
        firewall_vm_name: openwrt-firewall
        firewall_vm_memory: 2048
        firewall_vm_vcpus: 2
        firewall_vm_image_url: https://downloads.openwrt.org/releases/25.12.4/targets/x86/64/openwrt-25.12.4-x86-64-generic-squashfs-combined-efi.img.gz
        firewall_vm_image_checksum: 013d5f6df2d33c9ca75c5059a0fe759cca0271572006c02ea877299966e1dff6
        firewall_vm_image_path: /var/lib/libvirt/images/openwrt-firewall.img.gz
        firewall_vm_networks:
          - network: lan
          - network: wan
```

## Notes

- If you supply a `.gz` image URL, the role will unpack it before defining the VM.
- This role does not configure OpenWRT itself; it only prepares the host and the VM.
