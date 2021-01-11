===================
OSISM CLI Reference
===================

The following section lists osism commands and describe these.

osism-ansible
=============

container manager_osism-ansible_1

.. code-block:: console

   osism-ansible generic all
       [--module-name ANSIBLE_MODULE | -m ANSIBLE_MODULE]
       [--args 'COMMAND' | -a 'COMMAND']
       [--limit ANSIBLE_INVENTORY_NAME | -l ANSIBLE_INVENTORY_NAME]

   --module-name setup
       run ANSIBLE_MODULE (ansible-doc -l) for host ANSIBLE_INVENTORY_NAME to STDOUT,
       can be forwarded e.g. via > to FILE
   --args 'COMMAND'
       arguments for command 'COMMAND', e.g. 'chronyc tracking'|'uname -a'
   --limit ANSIBLE_INVENTORY_NAME
       limits the action to ANSIBLE_INVENTORY_NAME

   osism-ansible generic all -m shell -a 'chronyc tracking'
   osism-ansible generic all -m setup -l server1

osism-ceph
==========

container manager_ceph-ansible_1

.. code-block:: console

   osism-ceph
       [mons]
       [mgrs]
       [osds]
       [--limit ANSIBLE_INVENTORY_NAME | -l ANSIBLE_INVENTORY_NAME]

   mgrs
      deploys ceph manager
   mons
      deploys ceph monitoring
   osds
      deployes ceph osd
   --limit ANSIBLE_INVENTORY_NAME
      limits the actions to ANSIBLE_INVENTORY_NAME

   osism-ceph mons
   osism-ceph mgrs
   osism-ceph osds -l server1,server2

osism-generic
=============

container manager_osism-ansible_1

.. code-block:: console

   osism-generic
       [backup-mariadb]
       [bootstrap]
       [check-reboot]
       [cleanup-backup-mariadb]
       [cleanup-docker]
       [cleanup-elasticsearch]
       [cleanup-sosreport]
       [cockpit]
       [configuration]
       [docker]
       [facts]
       [hardening]
       [hosts]
       [network]
       [operator]
       [ping]
       [python]
       [reboot]
       [repository]
       [resolvconf]
       [sosreport]
       [timezone]
       [upgrade-packages]
       [--user USER | -u USER]
       [--key-file /path/to/id_rsa]
       [--ask-pass]
       [--ask-become-pass]
       [--become]
       [--limit ANSIBLE_INVENTORY_NAME | -l ANSIBLE_INVENTORY_NAME]

   backup-mariadb
       mariadb backup
   bootstrap
       bootstrap
   check-reboot
       check if reboot is necessary
   cleanup-backup-mariadb
       cleanup backups
   cleanup-docker
       cleanup docker
   cleanup-elasticsearch
       cleanup elasticsearch
   cleanup-sosreport
       cleanup sos reports
   cockpit
       cockpit role
   configuration
       get the latest git data for osism
   docker
       install/update/configure docker daemon
   facts
       update the facts
   hardening
       hardening role
   hosts
       update /etc/hosts
   network
       configure network
   operator
       login via key and configure dragon user
       in combination with --user, --key-file and --limit or
       --ask-pass, --ask-become-pass and --become
       --user USER
           argument for remote user
       --key-file /path/to/id_rsa
           argument for keyfile to login via remote user
       --ask-pass
           argument for asking the login password
       --ask-become-pass
           argument for asking the become pass
       --become
           argument for using the become method, e.g. sudo
   ping
       connection test via ansible
   python
       install python on server
   reboot
       reboot, the playbook asks are you sure
   repository
       add repositories
   resolvconf
       update DNS
   sosreport
       create sosreports
   timezone
       configure timezone
   upgrade-packages
       upgrade the repository packages, the playbook asks are you sure

   osism-generic configuration
   osism-generic hosts -l server1,server2
   osism-generic operator -l server1 -u root --key-file /path/to/keyfile
   osism-generic operator -l server1 -u ubuntu --ask-pass --ask-become-pass
   osism-generic python --limit server1 -u ubuntu --ask-pass --ask-become-pass

osism-infrastucture
===================

container manager_osism-ansible_1

.. code-block:: console

   osism-infrastructure
       [helper]
       [cobbler]
       [mirror]
       [mirror-images]
       [mirror-packages]
       [--tags HELPER_TAG]
       [--limit ANSIBLE_INVENTORY_NAME | -l ANSIBLE_INVENTORY_NAME]

   cobbler
       deploy/configure/update cobbler
   helper
       deploy helper like cephclient, openstackclient, phpmyadmin, rally, sshconfig, adminer
   mirror
       deploy aptly, nexus, registry
   mirror-images
       mirror images
   mirror-packages
       create aptly mirror

   osism-infrastructure helper --tags cephclient

osism-kolla
===========

container manager_kolla-ansible_1

.. code-block:: console

   osism-kolla
       [deploy SERVICE]
       [pull SERVICE]
       [reconfigure SERVICE]
       [upgrade SERVICE]
       [--limit ANSIBLE_INVENTORY_NAME | -l ANSIBLE_INVENTORY_NAME]

   deploy
       deploy SERVICE like common, keystone, nova, neutron
   pull
       pull container image for SERVICE
   reconfigure
       reconfigure SERVICE, e.g. configuration change
   upgrade
       upgrade SERVICE, e.g. Rocky -> Stein

   osism-kolla deploy common
   osism-kolla pull nova
   osism-kolla reconfigure neutron -l server1
   osism-kolla upgrade nova

osism-manager
=============

script using environment /opt/configuration/environments/manager/

.. code-block:: console

   osism-manager
       [manager]

   manager
       deploy/update manager, twice vault pw
   prefix
       please use environment variables for Ansible configuration like ANSIBLE_ASK_VAULT_PASS=True

   ANSIBLE_ASK_VAULT_PASS=True osism-manager manager

osism-mirror
============

script using environment /opt/configuration/environments/infrastructure
.. code-block:: console

   osism-mirror images, packages
       # synchronize images and packages

osism-monitoring
================

.. code-block:: console

   osism-monitoring prometheus-exporter, prometheus, monitoring
       # deploy prometheus, grafana and configuration

osism-openstack
===============

.. code-block:: console

   osism-openstack nova-aggregates
   osism-openstack nova-flavors
   osism-openstack glance-images

osism-run
=========

osism-run is for all additional plays/playbooks

.. code-block:: console

   osism-run proxmox create
       # create proxmox VM
   osism-run custom force-timesync
       # force NTP sync via chrony http://docs.osism.io/operations/generic.html#run-commands
   osism-run-without-secrets ...
       # runs the following command without asking for password and without all secrets,
         e.g. for cronjobs
   osism-run custom personalized-accounts
       # runs playbook for configuring personalized accounts

osism-run-without-secrets
=========================

run playbooks without vault access

.. code-block:: console

   dragon@controller:~$ cat /etc/cron.d/osism
   INTERACTIVE="false"
   #Ansible: gather facts
   15 */6 * * * dragon /usr/local/bin/osism-run-without-secrets generic facts