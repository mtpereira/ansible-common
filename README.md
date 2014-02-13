Common
========

Ansible role for host basic and common configuration: this role should contain essencial general configuration that any host should have, regardless of its function.

This role is designed to be used in either Debian- or EL-based systems **but, at the moment, it has only been tested in Debian!**

There were made no assumptions regarding the user that is running these tasks.

At the moment, it provides means to:

* Add custom Debian or EL repositories;
* Install a list of packages;
* Set hostname;
* Configure NTP.

TODO:

* Configure swap if necessary;
* Test on Centos.

Requirements
------------

None.

Role Variables
--------------

Hostname related variables:
* `common_hostname`: Set hostname. Defaults to "ansible".
* `common_hostname_domain`: Set host's domain to be used in /etc/hosts. Defaults to "domain".

NTP configuration variables:
* `common_ntp_servers`: Servers to be used by NTP to sync time. Defaults to the following servers, which are Portuguese NTP servers (taken from http://www.pool.ntp.org/zone/pt): '3.pt.pool.ntp.org', '0.europe.pool.ntp.org', '3.europe.pool.ntp.org'.
* `common_ntp_options`: (Optional) Some additional options for NTP. It only takes effect when set.

Packages installation variables:
* `common_pkg_state`: Specifies if this role will garantee that the packages are installed or installed and updated. Possible values: `installed` and `latest`. Defaults to `installed`. **Applies to all action that install packages in this role!**
* `common_pkg_list_deb`: (Optional) List of packages to be installed in a Debian server. It only takes effect when set.
* `common_pkg_list_rpm`: (Optional) List of packages to be installed in a Debian server. It only takes effect when set.

Repositories management variables:
* `common_repo_deb`: (Optional) List of Debian repositories to be added. Format is:
```
- { url: 'http://ftp.rnl.ist.utl.pt/pub/debian', components: 'main contrib' }
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
