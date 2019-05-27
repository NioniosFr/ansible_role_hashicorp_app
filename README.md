Ansible Role: HashiCorp Application
=========

An ansible role to install hashicorp applications in a Linux x86_64 based system.

Requirements
------------

The role targets Debian and RHEL based systems build on the `x86_64` architecture.

The role is intended to run on the remote machine, which means that internet connectivity on remote is required.

Supported HashiCorp products are:

- [vagrant](https://www.vagrantup.com/)
- [packer](https://www.packer.io/)
- [terraform](https://www.terraform.io/)
- [consul](https://www.consul.io/)
- [nomad](https://nomadproject.io/)
- [vault](https://www.vaultproject.io/)
- [atlas-upload-cli](https://github.com/hashicorp/atlas-upload-cli)

Role Variables
--------------

Required:

```yaml
hashicorp_app_name: # The name of a valid HashiCorp product. See: https://checkpoint.hashicorp.com/ and https://releases.hashicorp.com/
```

Default:

```yaml
hashicorp_app_version: "latest" # Latest or a released version from: https://releases.hashicorp.com/{{hashicorp_app_name}}/ to keep the package frozen.

hashicorp_app_binary_dest: "/opt/{{ hashicorp_app_name }}" # The destination directory where the `packer` binary will be placed

hashicorp_app_cleanup_after: false # If set to true, it will cleanup all downloaded files

hashicorp_app_configure_system_path: true # Whether the `hashicorp_app_binary_dest` directory should be added to the system `PATH`
hashicorp_app_system_path_prepend: false # Whether to append or prepend the `hashicorp_app_binary_dest` directory into the `PATH`, IF (hashicorp_app_configure_system_path is True).

hashicorp_app_tmp_dir: # Temporary folder to store the donloaded archive
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
    - hosts: localhost
      roles:
        - role: nioniosfr.hashicorp_app
          vars:
            hashicorp_app_name: "terraform" # Installs the latest version of terraform by overriding the current (if any)

        - role: nioniosfr.hashicorp_app
          vars:
            hashicorp_app_name: "packer"
            hashicorp_app_version: "1.4.1" # Use a specific version
            hashicorp_app_tmp_dir: "/mnt/nfs_share/downloads" # Store the downloaded archive in a more persistent path than '/tmp'

        - role: nioniosfr.hashicorp_app
          vars:
            hashicorp_app_name: "consul"
            hashicorp_app_binary_dest: "/usr/local/bin" # Installs in a common user path
            hashicorp_app_configure_system_path: false # Do not manipulate the system path for users
            hashicorp_app_tmp_dir: "/mnt/nfs_share/downloads" # Change the folder used for the downloads
            hashicorp_app_cleanup_after: true # Remove both the downloaded file, as well as the system profile.d for consul if it was already created from a previous run
```

License
-------

MIT

Author Information
------------------

[NioniosFr](https://github.com/NioniosFr)
