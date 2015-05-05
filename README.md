Workstation
===========

This project provides a set of Ansible playbooks and roles that configure a
Java developer's workstation.  As developers, we often like the cleanliness of
a fresh Linux install, but dread the time it takes to reinstall our tools.
The scripts included with this project automate the process of getting back to
work.  Generally, your Internet bandwidth will determine how long it takes
this process to complete.

This project currently supports 32 and 64 bit Debian-based systems.  It should
be possible to add support for Fedora-based systems without too much work if
there's interest.

Operation
---------

The software development (and systems) tools installed by this project fall
into two categories.  How these installations are managed is dramatically
different depending on the class of the application as follows:

-   Operating System (OS) packaged applications - Applications that provide
    packages in the target operating system are installed using the OS' package
    manager.  Currently these applications are Debian style packages that can
    be installed using "apt".

-   Unpackaged applications - Applications that do not provide OS packages are
    installed by downloading the binary (or source if needed), then compiled
    and/or unarchived into a "safe" location.  Whether you're using a Linux
    distribution that continuously updates or one that expects you to reinstall
    the system when updating to a new major version, the applications installed
    from archives or source code can be protected from the updater.  Applications
    are installed into the following directory:

        /opt/<application-name>/<application-version>

    For systems that upgrade their distribution by reinstalling the OS from a new
    image (such as Linux Mint), it is recommended that the /opt directory be
    installed in a separate partition.  When the OS is upgraded, simply remap the
    /opt directory into the file system without reformating.
    
    Since the path also includes the version number, the applications managed
    by this project are safe during updates.  When a new version of an application
    is included, it's simply installed "next to" the older version of the same
    application.  A soft-link to the most recent version is installed in:

        /opt/<application-name>/latest

    The link to the latest version of the application is used by the desktop
    launchers installed into the OS' menu.  Therefore, launching an application
    from the menu will result in the latest version being launched.  If you n
        provide eed
    to run an older version, you can do so by navigating to the appropriate
    version and run it via the command line.  This project never deletes any of
    the code it installs.  Therefore, if you need to recoup space on your hard
    drive, you'll have to manually delete older versions of the application from
    the /opt directory.

Use
---

There are three scripts included with this project.  The first two simply
prepare a newly installed Linux system to allow the use of Ansible.  The third
script does all the work and is idempotent.  Run it periodically and it will
make sure your system stays up-to-date.  These three scripts are as follows:

*   ansible.sh - This script requires sudo privileges and simply installs
    Ansible and its dependencies on the target system, then creates an SSH key
    that Ansible will later use to connect back into the system.  Run this
    script once or run the steps manually.  It is possible to alter the Ansible
    inventory file to maintain a pool of development workstations.
                 
*   bootstrap.sh - This script creates an Ansible inventory file containing
    only localhost, then uses Ansible to execute the playbook provided by
    bootstrap.yml.  This playbook creates the ansible system user, adds the
    user's public key and then configures passwordless sudo.  Run this script
    once, or perform the equivalent steps manually.
                   
*   workstation.sh - This script actually runs the Ansible playbook that builds
    and maintains the development environment.  It can be run frequently and at
    a minimum will keep the programs installed via the system's package manager
    (currently apt) up-to-date.  It's possible to exclude some or all of the
    programs that are defined in this playbook via the use of tags.  See the
    "tags" section below for more details.

Tags
----

tags:

- coverage
- database
- eclipse
- editors
- firefox-de
- git
- issues
- java
- ldap
- maven
- modeling
- networking
- quality
- subversion
- uml
- virtualization

Example
-------
```
./workstation.sh --tags=networking
```

