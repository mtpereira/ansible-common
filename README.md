Common
========

Ansible role for host basic and common configuration: this role should contain essencial general configuration that any host should have, regardless of its function.

This role is designed to be used in either Debian- or EL-based systems **but, at the moment, it has only been tested in Debian!**

There were made no assumptions regarding the user that is running these tasks.

At the moment, it provides means to:

* Install ansible dependencies;
* Add custom Debian or EL repositories;
* Install a list of packages;
* Set hostname;
* Configure timezone;
* Configure NTP;
* Add swap file if there's none mounted.

TODO:

* Test on Centos.

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
- { url: 'https://ftp-master.debian.org/keys/archive-key-6.0.asc', key: '473041FA' }
```

* `common_repo_rpm`: (Optional) List of EL repositories to be added. Repositories are added by copying the repo description file from local to remote. Format is:
```- etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6```

* `common_repo_rpm_keys`: (Optional) List of EL repository keys to be added. It only takes effect when set. Format is:
```
# add by URL
- http://apt.sw.be/RPM-GPG-KEY.dag.txt
# add by local file
- /path/to/key.gpg
```

Packages:

* `common_pkg_state`: Specifies if this role will garantee that the packages are installed or installed and updated. Possible values: `installed` and `latest`. Defaults to `installed`. **Applies to all action that install packages in this role!**
* `common_pkg_list_deb`: (Optional) List of packages to be installed in a Debian server. It only takes effect when set.
* `common_pkg_remove_list_deb`: (Optional) List of packages to be removed in a Debian server. It only takes effect when set.
* `common_pkg_list_rpm`: (Optional) List of packages to be installed in a RedHat server. It only takes effect when set.
* `common_pkg_remove_list_rpm`: (Optional) List of packages to be installed in a RedHat server. It only takes effect when set.
* `common_pkg_purge`: (Optional) Whether to purge or not packages of `common_pkg_remove_list_deb` in a Debian server. Defaults to `no`.

Hostname:

* `common_hostname`: Set hostname. Defaults to "ansible".
* `common_hostname_domain`: Set host's domain to be used in /etc/hosts. Defaults to "domain".

Timezone:

* `common_timezone`: Timezone to be configured. Defaults to `Etc/UTC`.

NTP:

* `common_ntp_servers`: Servers to be used by NTP to sync time. Defaults to the following servers, which are Portuguese NTP servers (taken from http://www.pool.ntp.org/zone/pt): '3.pt.pool.ntp.org', '0.europe.pool.ntp.org', '3.europe.pool.ntp.org'.
* `common_ntp_options`: (Optional) Some additional options for NTP. It only takes effect when set.

Swap:

* `common_swap`: Enables swap file creation. Defaults to `yes`.
* `common_swap_size`: Defines swap size in MB. Defaults to `ansible_memtotal_mb`.
* `common_swap_fstab`: Defines whether or not to add swap to fstab. Defaults to `yes`.
* `common_swap_path`: Defines the path for the swap file. Defaults to `/swap`.

Testing
-------

For testing purposes, a Vagrantfile was added. Simply run ```vagrant up``` in your working copy dir to get a Debian host up and provisioned with ```ansible-common.yml``` playbook. **At the moment there are no tests defined for EL-based systems!**

License
-------

MIT

Author Information
------------------

[GitHub project page](https://github.com/mtpereira/ansible-common)

[Manuel Tiago Pereira](http://mtpereira.github.io)
