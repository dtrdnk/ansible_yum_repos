yum_repos
=========

The role manage RHEL based repos files and settings of package manager 

Requirements
------------

Redefine or modify exist setting in default, if you need customization

Role Variables
--------------

Example variables for RHEL 8+ (defaults values of this role)

    yum_repo_common_files:
      - name: CentOS-Stream-AppStream
        repos:
          - name: appstream
            mirrorlist: 'http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=AppStream&infra=$infra'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
      - name: CentOS-Stream-BaseOS
        repos:
          - name: baseos
            mirrorlist: 'http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=BaseOS&infra=$infra'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
      - name: CentOS-Stream-Extras
        repos:
          - name: extras
            mirrorlist: 'http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras&infra=$infra'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
      - name: CentOS-Stream-Extras-common
        repos:
          - name: extras-common
            mirrorlist: 'http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras-extras-common'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-Extras
      - name: epel
        repos:
          - name: epel
            metalink: 'https://mirrors.fedoraproject.org/metalink?repo=epel-$releasever&arch=$basearch&infra=$infra&content=$contentdir'
            enabled: 1
            countme: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
            gpgcheck: 1

    yum_repo_additional_files:
      - name: docker-ce
        repos:
          - name: 'docker-ce-stable'
            baseurl: 'https://download.docker.com/linux/centos/$releasever/$basearch/stable'
            gpgcheck: 1
            enabled: 1
            gpgkey: https://download.docker.com/linux/centos/gpg
    
    rpm_keys:
      - name: epel
        url: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
        saved_path: /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
        state: present

    package_manager_config:
      - name: main
        gpgcheck: 1
        installonly_limit: 3
        clean_requirements_on_remove: True
        best: True
        skip_if_unavailable: False

Example variables for RHEL 7

    yum_repo_common_files:
      - name: CentOS-Base
        repos:
          - name: base
            mirrorlist: 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-{{ ansible_distribution_major_version }}
          - name: updates
            mirrorlist: 'http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra'
            gpgcheck: 1
            enabled: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-{{ ansible_distribution_major_version }}
      - name: epel
        repos:
          - name: epel
            metalink: 'https://mirrors.fedoraproject.org/metalink?repo=epel-$releasever&arch=$basearch&infra=$infra&content=$contentdir'
            enabled: 1
            countme: 1
            gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
            gpgcheck: 1

    yum_repo_additional_files:
      - name: docker-ce
        repos:
          - name: 'docker-ce-stable'
            baseurl: 'https://download.docker.com/linux/centos/$releasever/$basearch/stable'
            gpgcheck: 1
            enabled: 1
            gpgkey: https://download.docker.com/linux/centos/gpg

    rpm_keys:
      - name: epel
        url: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
        saved_path: /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
        state: present
    
    package_manager_config:
      - name: main
        cachedir: /var/cache/yum/$basearch/$releasever
        keepcache: 0
        debuglevel: 2
        logfile: /var/log/yum.log
        exactarch: 1
        obsoletes: 1
        gpgcheck: 1
        plugins: 1
        installonly_limit: 5
        bugtracker_url: http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
        distroverpkg: centos-release

Dependencies
------------

None

Example of test
---------------
Command to run all tests
```{bash}
molecule test --all
```
Command to run specific case
```{bash}
molecule test -s rhel7
```
You can find more information in the [documentation for the molecule](https://molecule.readthedocs.io/en/latest)

Example Playbook
----------------

An example of how to use this role:

    - hosts: servers
      roles:
         - { role: iac.yum_repos, tags: yum_repos }

License
-------

BSD

Author Information
------------------

[Dmitrii Demenko e-mail](d.demenko@iac.spb.ru)
