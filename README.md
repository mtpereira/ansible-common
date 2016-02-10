Common
========

Ansible role for host basic and common configuration: this role contains essencial general configuration for any Debian-based host.

This role is designed to be used on Debian-based systems. **Support for EL-based distros as been dropped on 3.0 !**

There were made no assumptions regarding the user that is running these tasks.

At the moment, it provides means to:

* Install ansible dependencies;
* Add custom repositories;
* Upgrades the system's packages;
* Install a list of packages;
* Set hostname;
* Configure timezone;
* Configure NTP;
* Add a swap file if there's none mounted;
* Adds custom mount points and configures their filesystems.

Requirements
------------

None.

Role Variables
--------------

Repositories management:

* `common_repo_deb`: (Optional) List of Debian repositories to be added. Format is:
```
- { url: 'http://ftp.rnl.ist.utl.pt/pub/debian', codename: 'wheezy', components: 'main contrib' }
```
* `common_repo_ppa`: (Optional) List of Ubuntu PPA repositories to be added. Format is:
```
- repo: 'ppa:ppa-user/ppa-name'
```
* `common_repo_deb_keys`: (Optional) List of Debian repository keys to be added. It only takes effect when set. Format is:
```
- { url: 'https://ftp-master.debian.org/keys/archive-key-6.0.asc', id: '473041FA' }
- { url: 'https://dl.google.com/linux/linux_signing_key.pub' }
```

Packages:

* `common_pkg_state`: Specifies if this role will garantee that the packages are installed or installed and updated. Possible values: `installed` and `latest`. Defaults to `installed`. **Applies to all action that install packages in this role!**
* `common_pkg_list_deb`: (Optional) List of packages to be installed in a Debian server. It only takes effect when set.
* `common_pkg_remove_list_deb`: (Optional) List of packages to be removed in a Debian server. It only takes effect when set.
* `common_pkg_purge`: (Optional) Whether to purge or not packages of `common_pkg_remove_list_deb` in a Debian server. Defaults to `no`.

Upgrade:

* `common_upgrade`: Whether to upgrade or not the system (`dist-upgrade`) in a Debian host. Defaults to `no`.
* `common_upgrade_reboot`: Whether to reboot or not the system after a upgrade. Defaults to `yes`.

Hostname:

* `common_hostname`: Set hostname. Defaults to "ansible".
* `common_hostname_domain`: Set host's domain to be used in /etc/hosts. Defaults to "domain".

Timezone:

* `common_timezone`: Timezone to be configured. Defaults to `Etc/UTC`. Note: Checkout https://github.com/ansible/ansible/issues/5715 for an explanation on the weird regex_replace on this variable on task `timezone - configure /etc/timezone`.

NTP:

* `common_ntp_servers`: Servers to be used by NTP to sync time. Defaults to the following servers, which are Portuguese NTP servers (taken from http://www.pool.ntp.org/zone/pt): '3.pt.pool.ntp.org', '0.europe.pool.ntp.org', '3.europe.pool.ntp.org'.
* `common_ntp_options`: (Optional) Some additional options for NTP. It only takes effect when set.

Filesystems:

* `common_filesystems`: (Optional) Create listed filesystems and mounts them according to provided options. This variable is a list of dictionaries, with the following structure:
* `type`: Filesystem type (`ext4`, `xfs`, etc).
* `device`: Device where the filesystem will be created.
* `opts`: Options for filesystem creation (`mkfs` options).
* `mount`: Mount point path.
* `mount_opts`: Mount options for `/etc/fstab`.
* `passno`: Mount filesystem check options.

For more information, run `ansible-doc filesystem` and `ansible-doc mount`.

Swap:

Configures a swap file. If there already exists a mounted swap (file or partition) and its size is approximately the same as `common_swap_size` (in MB), it won't perform any action.
The check is not for the exact same size because the actual mounted size partition is slight smaller than the swap file/partion size. As such, we check for a difference of at least 10%.

* `common_swap`: Enables swap file creation. Defaults to `yes`.
* `common_swap_size`: Defines swap size in MB. Defaults to `ansible_memtotal_mb`.
* `common_swap_fstab`: Defines whether or not to add swap to fstab. Defaults to `yes`.
* `common_swap_path`: Defines the path for the swap file. Defaults to `/swap`.

Testing
-------

For testing purposes, a Vagrantfile was added. Simply run ```vagrant up``` in your working copy dir to get a Debian host up and provisioned with ```test.yml``` playbook.

License
-------

MIT

Author Information
------------------

[GitHub project page](https://github.com/mtpereira/ansible-common)

[Manuel Tiago Pereira](https://mtpereira.com)
