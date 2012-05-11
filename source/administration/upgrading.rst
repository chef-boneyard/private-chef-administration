
.. index::
  single: upgrading

======================
Upgrading Private Chef
======================

.. index::
  pair: upgrading; standalone upgrade

Standalone Upgrade
------------------

The installation process for a standalone upgrade of Private Chef does not
reconfigure Private Chef or restart any of the services. This prevents
inadvertent failovers from occurring on HA installations.

On RedHat RPM based systems run rpm with the appropriate upgrade flags and with the new
RPM to be installed:

.. code-block:: bash

  $ rpm -Uvh private-chef-1.1.10-1.el6.x86_64.rpm

.. warning::

  When upgrading from Private Chef version 1.1.14 or earlier, a package script
  will delete /usr/bin/private-chef-ctl. You can recreate it with:

  ``$ ln -sf /opt/opscode/embedded/cookbooks/bin/private-chef-ctl /usr/bin/``


On Ubuntu or Debian deb-package based systems run dpkg with the install flag:

.. code-block:: bash

  $ dpkg -i private-chef_1.1.10-1.ubuntu.10.04_amd64.deb


After installing the upgraded package, you must instruct private-chef-ctl to
update the configuration:

.. code-block:: bash

  $ private-chef-ctl upgrade

.. index::
  pair: upgrade; high availabilty upgrade

High Availability Upgrade
-------------------------

The high availability process is substantially more complicated than a standalone upgrade.  Systems must be
shut down and upgraded in a specific order, and key material generated during the first install must be
copied around manually to the other nodes in the cluster by the user performing the upgrade.  Also it is
important to validate that services are down and kill any stray processes (this is for upgrading from
old builds prior to 1.1.10).

.. index::
  triple: upgrade; high availabilty upgrade; identifying the backend master

Identifying the Backend Master
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run :command:`private-chef-ctl ha-status` on both backend servers and identify which server returns the line
identifying it as the master backend:

.. code-block:: bash

  [OK] cluster status = master

.. index::
  triple: upgrade; high availabilty upgrade; shutting down frontends

Shutting Down Frontends
~~~~~~~~~~~~~~~~~~~~~~~~

Login to each frontend server in the cluster and make certain to fully stop all Private Chef related
proceses on the servers.  First stop all the services, and then ensure that any stuck processes
have been cleaned up with the graceful-kill command:

.. code-block:: bash

  $ private-chef-ctl stop
  $ private-chef-ctl graceful-kill

.. index::
  triple: upgrade; high availabilty upgrade; shutting down the backend slave

Shutting Down The Backend Slave
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To shut down the backend slave box first stop keepalived and tail the keepalived logs in case there
is a cluster failover in process:

.. code-block:: bash

  $ private-chef-ctl keepalived stop
  $ private-chef-ctl keepalived tail

If there is a state transition from master to backup running, it must be allowed to complete:

.. code-block:: bash

  ==> /var/log/opscode/keepalived/cluster.log <==
  Wed, 28 Mar 2012 22:09:14 +0000: Stopping service opscode-expander-reindexer
  Wed, 28 Mar 2012 22:09:14 +0000: Stopping service opscode-org-creator
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-chef
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-erchef
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-webui
  Wed, 28 Mar 2012 22:09:16 +0000: Stopping service php-fpm
  Wed, 28 Mar 2012 22:09:16 +0000: Stopping service fcgiwrap
  Wed, 28 Mar 2012 22:09:17 +0000: Stopping service nagios
  Wed, 28 Mar 2012 22:09:17 +0000: Stopping service nginx
  Wed, 28 Mar 2012 22:09:18 +0000: Transitioned to backup

Next, stop all the services, and then ensure that any stuck processes
have been cleaned up with the graceful-kill command:

.. code-block:: bash

  $ private-chef-ctl stop
  $ private-chef-ctl graceful-kill

.. index::
  triple: upgrade; high availabilty upgrade; shutting down the backend master

Shutting Down The Backend Master
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The backend master box should be the last box shut down to avoid generating a failvover event.  If
the other nodes are shut down correctly, the backend master may not be shut down.

First, stop all the services, and then ensure that any stuck processes
have been cleaned up with the graceful-kill command:

.. code-block:: bash

  $ private-chef-ctl stop
  $ private-chef-ctl graceful-kill

.. index::
  triple: upgrade; high availabilty upgrade; upgrading the backend master

Upgrading The Backend Master
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning::

  The server previously identified as the backend master must be brought up first so
  that backend processes can initialize their state before accepting connections from
  frontend applications.

On RedHat RPM based systems run rpm with the appropriate upgrade flags and with the new
RPM to be installed:

.. code-block:: bash

  $ rpm -Uvh private-chef-1.1.10-1.el6.x86_64.rpm

On Ubuntu or Debian deb-package based systems run dpkg with the install flag:

.. code-block:: bash

  $ dpkg -i private-chef_1.1.10-1.ubuntu.10.04_amd64.deb

After installing the upgraded package, you must instruct private-chef-ctl to
update the configuration and start the system:

.. code-block:: bash

  $ private-chef-ctl upgrade
  $ private-chef-ctl start

.. index::
  triple: upgrade; high availabilty upgrade; validating the backend master

Validating The Backend Master
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, wait until the load average of the server has dropped by watching :command:`top` until
the load average on the server is below 1.00 and the server has finished initailizing.  Then
run the test suite against the backend by running the command on the upgraded backend master:

.. code-block:: bash

  $ private-chef-ctl test

If this test succeeds without any red failing tests, then you are ready to proceed.

.. index::
  triple: upgrade; high availabilty upgrade; copying configuration to other nodes

Copying Configuration To Other Nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning::

  The entire contents of /etc/opscode must be copied from the backend
  master to all of the other members of the cluster.  There may be new
  configuration state generated as a result of bootstrapping the first
  member of the cluster which must agree on all cluster members.

The entire contents of /etc/opscode on the backend master must now be copied to the other
cluster members.  The easiest way to accomplish this is to have ssh root trust and logins setup
between all the cluster members and to copy the contents around from the backend master.  In
a cluster with backed master named be1, backend slave named be2, and frontend servers fe1 and
fe2 this might look like:

.. code-block:: bash

  be1# scp /etc/opscode/* fe1:/etc/opscode
  be1# scp /etc/opscode/* fe2:/etc/opscode
  be1# scp /etc/opscode/* be2:/etc/opscode

The details of how to accomplish shipping this data between servers will vary from site to site, please
use whatever scp and rsync tools you have available.

For example, a particularly simple method is to configure SSH agent forwarding on your
workstation. A successful authentication and login of the user from
the workstation to be1 can be passed through from be1 to the other members of
the cluster, just by initiating a connection to them from be1. 

.. index::
  triple: upgrade; high availabilty upgrade; upgrading the backend slave

Upgrading The Backend Slave
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the /etc/opscode files have been copied over to the backend slave from the master, the backend
slave may be updated.

On RedHat RPM based systems run rpm with the appropriate upgrade flags and with the new
RPM to be installed:

.. code-block:: bash

  $ rpm -Uvh private-chef-1.1.10-1.el6.x86_64.rpm

On Ubuntu or Debian deb-package based systems run dpkg with the install flag:

.. code-block:: bash

  $ dpkg -i private-chef_1.1.10-1.ubuntu.10.04_amd64.deb

After installing the upgraded package, you must instruct private-chef-ctl to
update the configuration and start the system:

.. code-block:: bash

  $ private-chef-ctl upgrade
  $ private-chef-ctl start

This may trigger a cluster failover, which will requiring watching the keepalived logs until
the cluster failover completes and the server has transitioned fully into either the
master or backup states:

.. code-block:: bash

  $ private-chef-ctl keepalived tail

  ==> /var/log/opscode/keepalived/cluster.log <==
  Wed, 28 Mar 2012 22:09:14 +0000: Stopping service opscode-expander-reindexer
  Wed, 28 Mar 2012 22:09:14 +0000: Stopping service opscode-org-creator
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-chef
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-erchef
  Wed, 28 Mar 2012 22:09:15 +0000: Stopping service opscode-webui
  Wed, 28 Mar 2012 22:09:16 +0000: Stopping service php-fpm
  Wed, 28 Mar 2012 22:09:16 +0000: Stopping service fcgiwrap
  Wed, 28 Mar 2012 22:09:17 +0000: Stopping service nagios
  Wed, 28 Mar 2012 22:09:17 +0000: Stopping service nginx
  Wed, 28 Mar 2012 22:09:18 +0000: Transitioned to backup

If instead bringing the backup node online triggers a transition to master, please use the
:command:`top` command to watch for the load average to fall below 1.00 before 
proceeding.

.. index::
  triple: upgrade; high availabilty upgrade; upgrading the frontends

Upgrading The Frontends
~~~~~~~~~~~~~~~~~~~~~~~

On RedHat RPM based systems run rpm with the appropriate upgrade flags and with the new
RPM to be installed:

.. code-block:: bash

  $ rpm -Uvh private-chef-1.1.10-1.el6.x86_64.rpm

On Ubuntu or Debian deb-package based systems run dpkg with the install flag:

.. code-block:: bash

  $ dpkg -i private-chef_1.1.10-1.ubuntu.10.04_amd64.deb

After installing the upgraded package, you must instruct private-chef-ctl to
update the configuration and start the system:

.. code-block:: bash

  $ private-chef-ctl upgrade
  $ private-chef-ctl start

