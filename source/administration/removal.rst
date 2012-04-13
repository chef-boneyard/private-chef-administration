
.. index::
  pair: removal; uninstall

======================
Removing Private Chef
======================

Removing the Private Chef packages will not stop any of Private Chef's
processes or prevent Private Chef's process supervisor from starting up on
reboot. In order to cleanly remove Private Chef from a system, you need to use
private-chef-ctl to stop processes and remove the process supervisor.

If you wish to preserve Private Chef's data, but otherwise delete the software:

.. code-block:: bash

  $ private-chef-ctl uninstall

To completely remove Private Chef, including all data, use the ``cleanse`` subcommand:

.. code-block:: bash

  $ private-chef-ctl cleanse


After Private Chef is stopped, it is safe to remove the package. On RedHad RPM based systems:


.. code-block:: bash

  $ rpm -eVh private-chef

On Ubuntu or Debian deb-package based systems:

.. code-block:: bash

  $ dpkg -P private-chef


