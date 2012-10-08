.. index::
  single: nagios

=============================
Nagios
=============================

Your Opscode Private Chef installation includes a Nagios server populated 
with monitors that will watch the various components of Private Chef and 
report on problems.

The default settings for this Nagios server are included in the 
:doc:`Configuration </administration/configuration>` section of this guide.

Logging In
----------

The Nagios server is located on the live backend server of your Private Chef
cluster.  The service binds to all active network interfaces, on port ``9671``.
The URL will be:

.. code-block:: ruby

  http://$IPADDRESS:9671/nagios/

The primary administrative user is ``nagiosadmin``.  The password for this 
user is stored in :file:`/etc/opscode/private-chef-secrets.json`.

Click on `Services` on the left column under `Current Status` to see the monitored
state of your Private Chef servers.

Configuration
-------------

One of the things you may want to change for your Nagios installation is the
password for the ``nagiosadmin`` account.  This password is automatically 
generated during the Private Chef installation process. It is stored in
:file:`/etc/opscode/private-chef-secrets.json`:

.. code-block:: javascript

  "nagios": {
    "admin_password": "b7dbecb622f9494be3980d04c540aa9eb5d0fb217a04d670a53c106f2698498f83108370ddf4991197c43a3c3e7cead46c8a"
  },

As of the current version of Private Chef, changing this password is not automated.
To change this value, delete the file :file:`/var/opt/opscode/nagios/etc/htpasswd` and the 
"nagios" section from your :file:`/etc/opscode/private-chef-secrets.json` file. Then add the new 
setting to your :file:`private-chef.rb` file and run :command:`private-chef-ctl reconfigure`.

.. code-block:: ruby

  nagios['admin_password'] = "privatechef"

This will regenerate the :file:`htpasswd` file with the new password in it.

Current versions of Private Chef do not support changing the admin account for Nagios.
The admin account will always be ``nagiosadmin``.

Checks to Note
--------------

Health of RabbitMQ
~~~~~~~~~~~~~~~~~~
There are two status checks for RabbitMQ.  One that looks at the current queue size, and the other that tracks the number of current connections. 

Both of these checks are NRPE checks, so you can test them with your own centralized Nagios server.

:command:`check_rmq_connections` uses ``rabbitmqctl list_connections`` to determine how many current open connections there are to the rabbitmq service.


.. code-block:: ruby

  command[check_rmq_connections]=/usr/bin/env HOME="/var/opt/opscode/rabbitmq" /opt/opscode/embedded/nagios/libexec/check_rmq_connections -w 300 -c 500


:command:`check_rmq_messages` uses ``rabbitmqctl list_queues`` to check how many messages are sitting in the queues.

.. code-block:: ruby

  command[check_rmq_messages]=/usr/bin/env HOME="/var/opt/opscode/rabbitmq" /opt/opscode/embedded/nagios/libexec/check_rmq_messages -w 100 -c 200

Health of Opscode Expander
~~~~~~~~~~~~~~~~~~~~~~~~~~
  
The :command:`opscode-expander` process takes updates from the Chef server and formats them for Solr. This check alerts if that queue gets too backed up.

.. code-block:: ruby

  command[check_opscode_expander]=/opt/opscode/embedded/service/opscode-expander/bin/check_queue_size -w 1000 -c 2000



Monitoring Private Chef with Your Nagios Server
-----------------------------------------------

The Nagios checks for Private Chef are available over NRPE. You can use
your existing Nagios server to monitor Private Chef by adding the IP address
of your Nagios server to the NRPE configuration on your Private Chef server.

By default, the ``allowed_hosts`` setting in NRPE is set to localhost and the primary IP 
of the Private Chef server.  To keep these two IPs available and add your Nagios 
server, you can add a setting to your :file:`private-chef.rb` file:

.. code-block:: ruby

  nrpe['allowed_hosts'] = [ "127.0.0.1", "192.168.72.1", "192.168.72.189" ]

Notice that the setting is an array, so the values are enclosed in square brackets. 
Leaving out the square brackets will cause an error in current versions of Private Chef.



