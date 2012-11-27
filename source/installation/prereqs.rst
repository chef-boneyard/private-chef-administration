Universal Prerequisites
========================

The following prerequisites apply to every installation of Private Chef.

Downloading Private Chef
------------------------

If you do not have a copy of Private Chef, please contact your sales
representative (`sales@opscode.com <mailto:sales@opscode.com>`_) or
installation engineer via the customer portal to receive one.

Private Chef is distributed on Red Hat and CentOS as an :command:`rpm` package,
and on Ubuntu as a :command:`deb`.

Choosing a Supported Operating Systems
--------------------------------------

Private Chef is supported on the following operating systems:

-  Red Hat Enterprise Linux 5 and 6 and clones (CentOS, etc)
-  Ubuntu 10.04 and 11.04

Private Chef requires a 64-bit x86_64 compatible systems architecture.

Configuring the Operating System
--------------------------------

Before installing Private Chef, ensure that each system has the
following installed and configured:

Hostnames and Fully Qualified Domain Names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ensure that the systems you are going to install Private Chef on have
properly configured hostnames, and resolvable fully qualified domain
names. Refer to your operating systems manual or local systems
administrator for guidance.

NTP
~~~

Private Chef requires that the systems on which it is running be connected to
NTP, as Chef is particularly sensitive to clock drift.

*Install NTP on Red Hat and CentOS 6*

.. code-block:: bash

  $ yum install ntp
  $ chkconfig -add ntp
  $ service ntpd start

*Install NTP on Ubuntu*

.. code-block:: bash

  $ apt-get install ntp

Mail Relay
~~~~~~~~~~

The Private Chef system utilizes email to send notifications for
various events (such as cluster fail-over, or failed periodic jobs.) We
recommend you follow your operating systems guidelines and individual
corporate policy for installation and configuration of a local Mail
Transfer Agent.

Cron
~~~~

Periodic maintenance tasks are performed on Private Chef servers via
Cron and the :file:`/etc/cron.d` directory. On CentOS 6 minimal
installations, you may not have Cron installed and configured.

*Install Crontabs on CentOS 6*

.. code-block:: bash

  $ yum install crontabs

Git
~~~

Private Chef requires that Git be installed, so that various internal
services can confirm their own revision.

*Install Git on Red Hat and CentOS 6*

.. code-block:: bash

  $ yum install git

*Install Git on Ubuntu*

.. code-block:: bash

  $ apt-get install git-core

Red Hat/CentOS dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Private Chef requires libfreetype and libpng, which may not be present in a minimal installation.

*Install freetype and libpng on Red Hat and CentOS 6*

.. code-block:: bash

  $ yum install freetype libpng

Apache Qpid
~~~~~~~~~~~

On CentOS and Red Hat systems, the Apache Qpid daemon is installed by default. In order to run Private Chef, this daemon must be disabled, as Private Chef uses RabbitMQ for messaging (and they share the same protocol).

To determine if it is installed:

.. code-block:: bash

  $ rpm -qa | grep qpid
  qpid-cpp-server-0.12-6.el6.x86_64

If you see a response like the above, you have the qpid server installed. To disable it:

.. code-block:: bash

  $ service qpidd stop
  $ chkconfig --del qpidd

Required Users
~~~~~~~~~~~~~~

If your environment has restrictions on the creation of local user and group
accounts (via the ``adduser`` command), you will need to ensure that the
following users exist:

========            =====     ====
Username            Shell     Home
========            =====     ====
opscode             /bin/bash /opt/opscode/embedded
opscode-pgsql       /bin/bash /var/opt/opscode/postgresql
opscode-nagios      /bin/bash /var/opt/opscode/nagios
opscode-nagios-cmd  /bin/bash /var/opt/opscode/nagios
========            =====     ====

