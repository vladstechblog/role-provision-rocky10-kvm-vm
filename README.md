role-provision-rocky10-kvm-vm
=========

Provisions Rocky10 VM on a KVM hypervisor using `virt-install`

Requirements
------------

- KVM hypervisor isntalled on the host OS
- user is able to make system conenctions (qemu:///system)
- user is sufficient privileges on the system


Role Variables
--------------

- `ipv4_addr:` IPv4 addres to be assigned to the VM
- `ipv4_mask:` IPv4 network mask to be assigned to the VM
- `ipv4_gate:` IPv4 gateway to be assigned to the VM
- `iso_path:` path to OS installation ISO
- `vm_name:` name of the VM to be provisioned (also used for virtual drives)
- `os_type:` defaults to `Linux`
- `os_variant:` defaults to `rocky_unknown`
- `extra_drive:` whether extra virtual drive will be provisioned
- `graphics:` defaults to `vnc`
- `number_of_vcpus:` self explanatory
- `vdisk_base_directory:` location where VM's virtual drives will be stored
- `ram_size_mb:` how much RAM to allocate to the VM in MB
- `libvirt_network:` name of the `libvirtd network` as shown by `virsh net-list`
- `default_user:` default user account to be created for provisioned VM
- `default_user_can_sudo_without_password:` yes/no (self explanatory)
- `primary_drive_size_gb:` size for the VM's OS virtual drive
- `secondary_drive_size_gb:` size of extra drive (if provisioned)
- `kickstart_output_directory:` kickstart template is rendered here


Dependencies
------------

None

Example Playbook
----------------


```yaml
- name: provision a Rocky VM with virt-install and kickstart
  hosts: localhost # ran on the host OS
  become: false # not needed with libvirtd group membership
  environment:
    LIBVIRT_DEFAULT_URI: 'qemu:///system' # 
  vars:
    ipv4_gate: 192.168.A.B
    ipv4_mask: 255.255.255.0
    ipv4_addr: 192.168.A.C
    iso_path: "{{ playbook_dir }}/common_files/rocky/Rocky-10.0-x86_64-dvd1.iso"
    vdisk_base_directory: /var/lib/libvirt/images/my_custom_directory
    pubkey_file: "~/.ssh/my_ssh_key.pub" 
    vm_name: my_cool_vm_name
    libvirt_network: default # can be changes
    primary_dirve_size_gb: 80
    extra_drive: yes
    secondary_drive_size_gb: 10
    kickstart_output_directory: "{{ playbook_dir }}/common_files/ks" 
    default_user_password_hash: "<PASSWORD_HASH_GOES_HERE>"
    default_user_ssh_pubkey: "{{ lookup('file', pubkey_file) }}"

  roles:
  - role-provision-rocky10-kvm-vm
```


License
-------

BSD

Author Information
------------------

https://vlads.tech
---


