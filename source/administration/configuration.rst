.. index::
  pair: configuration; private-chef.rb

========================
Configuring Private Chef
========================

Configuration of Private Chef is done through the
:file:`/etc/opscode/private-chef.rb` file.  The file itself is written in Ruby,
allowing you to have as much flexibility as possible with how you configure the
system.

While there are a great deal configuration options, the number required for
common use is quite small. For standalone, single server configurations, no
configuration is required at all - the defaults take care of everything.

A typical High Availability or Tiered configuration consists of only:

* A ``topology``
* A number of ``server`` entries
* An ``api_fqdn`` entry
* A ``backend_vip`` entry
* A ``notification_email``

See the :doc:`tiered </installation/tiered>` and :doc:`high-availability </installation/ha>` 
installation documentation for complete configuration examples.

Applying configuration changes
------------------------------

The :command:`private-chef-ctl reconfigure` command reads your
:file:`/etc/opscode/private-chef.rb` file and configures all of the services
for you. Any time you make a change to your configuration, you need to run
:command:`private-chef-ctl reconfigure` to apply it.

.. index::
  pair: configuration; general options

Common Options
---------------

.. index::
  triple: configuration; common options; topology

topology
~~~~~~~~

Private Chef configurations are governed by a ``topology``, which describes
which of our recommended architectures you plan to install. Your choices are:

*Default Value*: 

.. code-block:: ruby

  "standalone"

*Options*:

- **standalone** (default): All of Private Chef running on a single server.
- **manual**: Identical to standalone.
- **tier**: Many front-end servers to a single, non-high-availability back-end server.
- **ha**: Many front-end servers to a high-availability back-end cluster.

*Example*:

.. code-block:: ruby
  
  topology "standalone"
  topology "manual"
  topology "tier"
  topology "ha"

.. index::
  triple: configuration; common options; notification_email

notification_email
~~~~~~~~~~~~~~~~~~

Private Chef generates notification emails from internal monitoring and
periodic cron jobs. This is the email address they will be sent to.

*Default Value*: ``pc-default@opscode.com``

*Example*:

.. code-block:: ruby
  
  notification_email "sysadmin@example.com"

.. index::
  triple: configuration; common options; server

server
~~~~~~~~~~~~~~~~~~

Server entries represent an individual server in your Private Chef
cluster. Each server has at least an ``ipaddress`` and ``role``, 
and can optionally be marked to run the ``bootstrap`` process.

*Example*:

For a back-end server, marked to run the initial bootstrap:

.. code-block:: ruby

  server "be1.example.com",
   :ipaddress => "192.168.4.1",
   :role => "backend",
   :bootstrap => true

For a back-end server, not marked to run the bootstrap:

.. code-block:: ruby 

  server "be2.example.com",
   :ipaddress => "192.168.4.2",
   :role => "backend"

A front-end server:

.. code-block:: ruby 

  server "fe1.example.com",
   :ipaddress => "192.168.4.3",
   :role => "fronted"

.. index::
  triple: configuration; common options; api_fqdn

api_fqdn
~~~~~~~~~~~~~~~~~~

In a tiered or high availability scenario, you are expected to be
running multiple frontend servers. The ``api_fqdn`` should point 
to the fully qualified domain name that you want to use for
accessing the Web UI and API. 

*Example*:

In the below example, you would access your Private Chef server at
``chef.example.com``.

.. code-block:: ruby

  api_fqdn "chef.example.com"

.. index::
  triple: configuration; common options; backend_vip

backend_vip
~~~~~~~~~~~~~~~~~~

When operating in a tiered or high-availability scenario, you need to
configure the ``backend_vip``.  In a High Availability setup, this 
should be set to the fully qualified domain name and IP address
you will be sharing between your back-end servers. In a Tiered configuration,
it should point directly to your back-end server.

*Example*:

.. code-block:: ruby

  backend_vip "be1.example.com",
   :ipaddress => "192.168.4.1"



.. index::
  pair: configuration; bootstrap

bootstrap
------------------------------------------------

.. index::
  triple: configuration; bootstrap; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Whether or not we should attempt to bootstrap. 

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  bootstrap['enable'] = true

.. index::
  pair: configuration; couchdb

couchdb
------------------------------------------------

.. index::
  triple: configuration; couchdb; batch_save_interval

batch_save_interval
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Number of milliseconds after which to save a batch.

*Default Value*: 

.. code-block:: ruby

  1000

*Example*: 

.. code-block:: ruby

  couchdb['batch_save_interval'] = 1000

.. index::
  triple: configuration; couchdb; batch_save_size

batch_save_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Number of documents after which to save a batch.

*Default Value*: 

.. code-block:: ruby

  1000

*Example*: 

.. code-block:: ruby

  couchdb['batch_save_size'] = 1000

.. index::
  triple: configuration; couchdb; bind_address

bind_address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The IP Address to bind CouchDB to.

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  couchdb['bind_address'] = "127.0.0.1"

.. index::
  triple: configuration; couchdb; data_dir

data_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/couchdb/db"

*Example*: 

.. code-block:: ruby

  couchdb['data_dir'] = "/var/opt/opscode/couchdb/db"

.. index::
  triple: configuration; couchdb; delayed_commits

delayed_commits
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "true"

*Example*: 

.. code-block:: ruby

  couchdb['delayed_commits'] = "true"

.. index::
  triple: configuration; couchdb; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/couchdb"

*Example*: 

.. code-block:: ruby

  couchdb['dir'] = "/var/opt/opscode/couchdb"

.. index::
  triple: configuration; couchdb; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  couchdb['enable'] = true

.. index::
  triple: configuration; couchdb; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  couchdb['ha'] = false

.. index::
  triple: configuration; couchdb; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/couchdb"

*Example*: 

.. code-block:: ruby

  couchdb['log_directory'] = "/var/log/opscode/couchdb"

.. index::
  triple: configuration; couchdb; log_level

log_level
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "error"

*Example*: 

.. code-block:: ruby

  couchdb['log_level'] = "error"

.. index::
  triple: configuration; couchdb; max_attachment_chunk_size

max_attachment_chunk_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "4294967296"

*Example*: 

.. code-block:: ruby

  couchdb['max_attachment_chunk_size'] = "4294967296"

.. index::
  triple: configuration; couchdb; max_dbs_open

max_dbs_open
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  10000

*Example*: 

.. code-block:: ruby

  couchdb['max_dbs_open'] = 10000

.. index::
  triple: configuration; couchdb; max_document_size

max_document_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "4294967296"

*Example*: 

.. code-block:: ruby

  couchdb['max_document_size'] = "4294967296"

.. index::
  triple: configuration; couchdb; os_process_timeout

os_process_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "300000"

*Example*: 

.. code-block:: ruby

  couchdb['os_process_timeout'] = "300000"

.. index::
  triple: configuration; couchdb; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  5984

*Example*: 

.. code-block:: ruby

  couchdb['port'] = 5984

.. index::
  triple: configuration; couchdb; reduce_limit

reduce_limit
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "false"

*Example*: 

.. code-block:: ruby

  couchdb['reduce_limit'] = "false"

.. index::
  triple: configuration; couchdb; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  couchdb['vip'] = "127.0.0.1"

.. index::
  pair: configuration; dark_launch

dark_launch
------------------------------------------------

.. index::
  triple: configuration; dark_launch; new_theme

new_theme
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  dark_launch['new_theme'] = true

.. index::
  triple: configuration; dark_launch; private-chef

private-chef
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  dark_launch['private-chef'] = true

.. index::
  triple: configuration; dark_launch; quick_start

quick_start
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  dark_launch['quick_start'] = false

.. index::
  triple: configuration; dark_launch; sql_users

sql_users
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  dark_launch['sql_users'] = true

.. index::
  pair: configuration; database_type

database_type
------------------------------------------------

.. index::
  triple: configuration; general options; database_type

database_type
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby 

  postgresql

*Example*: 

.. code-block:: ruby

  database_type "postgresql"

.. index::
  pair: configuration; drbd

drbd
------------------------------------------------

.. index::
  triple: configuration; drbd; data_dir

data_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/drbd/data"

*Example*: 

.. code-block:: ruby

  drbd['data_dir'] = "/var/opt/opscode/drbd/data"

.. index::
  triple: configuration; drbd; device

device
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/dev/drbd0"

*Example*: 

.. code-block:: ruby

  drbd['device'] = "/dev/drbd0"

.. index::
  triple: configuration; drbd; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/drbd"

*Example*: 

.. code-block:: ruby

  drbd['dir'] = "/var/opt/opscode/drbd"

.. index::
  triple: configuration; drbd; disk

disk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/dev/opscode/drbd"

*Example*: 

.. code-block:: ruby

  drbd['disk'] = "/dev/opscode/drbd"

.. index::
  triple: configuration; drbd; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  drbd['enable'] = false

.. index::
  triple: configuration; drbd; flexible_meta_disk

flexible_meta_disk
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "internal"

*Example*: 

.. code-block:: ruby

  drbd['flexible_meta_disk'] = "internal"

.. index::
  triple: configuration; drbd; primary

primary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"fqdn"=>"ubuntu.localdomain", "ip"=>"192.168.4.131", "port"=>7788}


*Example*: 

.. code-block:: ruby

  drbd['primary'] = {"fqdn"=>"ubuntu.localdomain", "ip"=>"192.168.4.131", "port"=>7788}


.. index::
  triple: configuration; drbd; secondary

secondary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"fqdn"=>"ubuntu.localdomain", "ip"=>"192.168.4.131", "port"=>7788}


*Example*: 

.. code-block:: ruby

  drbd['secondary'] = {"fqdn"=>"ubuntu.localdomain", "ip"=>"192.168.4.131", "port"=>7788}


.. index::
  triple: configuration; drbd; shared_secret

shared_secret
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "promisespromises"

*Example*: 

.. code-block:: ruby

  drbd['shared_secret'] = "promisespromises"

.. index::
  triple: configuration; drbd; sync_rate

sync_rate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "40M"

*Example*: 

.. code-block:: ruby

  drbd['sync_rate'] = "40M"

.. index::
  triple: configuration; drbd; version

version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "8.4.1"

*Example*: 

.. code-block:: ruby

  drbd['version'] = "8.4.1"

.. index::
  pair: configuration; estatsd

estatsd
------------------------------------------------

.. index::
  triple: configuration; estatsd; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/estatsd"

*Example*: 

.. code-block:: ruby

  estatsd['dir'] = "/var/opt/opscode/estatsd"

.. index::
  triple: configuration; estatsd; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  estatsd['enable'] = true

.. index::
  triple: configuration; estatsd; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/estatsd"

*Example*: 

.. code-block:: ruby

  estatsd['log_directory'] = "/var/log/opscode/estatsd"

.. index::
  triple: configuration; estatsd; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9466

*Example*: 

.. code-block:: ruby

  estatsd['port'] = 9466

.. index::
  triple: configuration; estatsd; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  estatsd['vip'] = "127.0.0.1"

.. index::
  pair: configuration; keepalived

keepalived
------------------------------------------------

.. index::
  triple: configuration; keepalived; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/keepalived"

*Example*: 

.. code-block:: ruby

  keepalived['dir'] = "/var/opt/opscode/keepalived"

.. index::
  triple: configuration; keepalived; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  keepalived['enable'] = false

.. index::
  triple: configuration; keepalived; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/keepalived"

*Example*: 

.. code-block:: ruby

  keepalived['log_directory'] = "/var/log/opscode/keepalived"

.. index::
  triple: configuration; keepalived; service_order

service_order
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  [{"key"=>"couchdb", "service_name"=>"couchdb"},
 {"key"=>"postgresql", "service_name"=>"postgres"},
 {"key"=>"rabbitmq", "service_name"=>"rabbitmq"},
 {"key"=>"redis", "service_name"=>"redis"},
 {"key"=>"opscode-authz", "service_name"=>"opscode-authz"},
 {"key"=>"opscode-certificate", "service_name"=>"opscode-certificate"},
 {"key"=>"opscode-account", "service_name"=>"opscode-account"},
 {"key"=>"opscode-solr", "service_name"=>"opscode-solr"},
 {"key"=>"opscode-expander", "service_name"=>"opscode-expander"},
 {"key"=>"opscode-expander", "service_name"=>"opscode-expander-reindexer"},
 {"key"=>"opscode-org-creator", "service_name"=>"opscode-org-creator"},
 {"key"=>"opscode-chef", "service_name"=>"opscode-chef"},
 {"key"=>"opscode-erchef", "service_name"=>"opscode-erchef"},
 {"key"=>"opscode-webui", "service_name"=>"opscode-webui"},
 {"key"=>"nagios", "service_name"=>"php-fpm"},
 {"key"=>"nagios", "service_name"=>"fcgiwrap"},
 {"key"=>"nagios", "service_name"=>"nagios"},
 {"key"=>"nginx", "service_name"=>"nginx"}]


*Example*: 

.. code-block:: ruby

  keepalived['service_order'] = [{"key"=>"couchdb", "service_name"=>"couchdb"},
 {"key"=>"postgresql", "service_name"=>"postgres"},
 {"key"=>"rabbitmq", "service_name"=>"rabbitmq"},
 {"key"=>"redis", "service_name"=>"redis"},
 {"key"=>"opscode-authz", "service_name"=>"opscode-authz"},
 {"key"=>"opscode-certificate", "service_name"=>"opscode-certificate"},
 {"key"=>"opscode-account", "service_name"=>"opscode-account"},
 {"key"=>"opscode-solr", "service_name"=>"opscode-solr"},
 {"key"=>"opscode-expander", "service_name"=>"opscode-expander"},
 {"key"=>"opscode-expander", "service_name"=>"opscode-expander-reindexer"},
 {"key"=>"opscode-org-creator", "service_name"=>"opscode-org-creator"},
 {"key"=>"opscode-chef", "service_name"=>"opscode-chef"},
 {"key"=>"opscode-erchef", "service_name"=>"opscode-erchef"},
 {"key"=>"opscode-webui", "service_name"=>"opscode-webui"},
 {"key"=>"nagios", "service_name"=>"php-fpm"},
 {"key"=>"nagios", "service_name"=>"fcgiwrap"},
 {"key"=>"nagios", "service_name"=>"nagios"},
 {"key"=>"nginx", "service_name"=>"nginx"}]


.. index::
  triple: configuration; keepalived; smtp_connect_timeout

smtp_connect_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "30"

*Example*: 

.. code-block:: ruby

  keepalived['smtp_connect_timeout'] = "30"

.. index::
  triple: configuration; keepalived; smtp_server

smtp_server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  keepalived['smtp_server'] = "127.0.0.1"

.. index::
  triple: configuration; keepalived; vrrp_instance_advert_int

vrrp_instance_advert_int
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "1"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_advert_int'] = "1"

.. index::
  triple: configuration; keepalived; vrrp_instance_interface

vrrp_instance_interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "eth0"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_interface'] = "eth0"

.. index::
  triple: configuration; keepalived; vrrp_instance_ipaddress

vrrp_instance_ipaddress
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "192.168.4.131"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_ipaddress'] = "192.168.4.131"

.. index::
  triple: configuration; keepalived; vrrp_instance_ipaddress_dev

vrrp_instance_ipaddress_dev
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "eth0"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_ipaddress_dev'] = "eth0"

.. index::
  triple: configuration; keepalived; vrrp_instance_password

vrrp_instance_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "sneakybeaky"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_password'] = "sneakybeaky"

.. index::
  triple: configuration; keepalived; vrrp_instance_priority

vrrp_instance_priority
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "100"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_priority'] = "100"

.. index::
  triple: configuration; keepalived; vrrp_instance_state

vrrp_instance_state
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "MASTER"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_state'] = "MASTER"

.. index::
  triple: configuration; keepalived; vrrp_instance_virtual_router_id

vrrp_instance_virtual_router_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "1"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_instance_virtual_router_id'] = "1"

.. index::
  triple: configuration; keepalived; vrrp_sync_group

vrrp_sync_group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "PC_GROUP"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_sync_group'] = "PC_GROUP"

.. index::
  triple: configuration; keepalived; vrrp_sync_instance

vrrp_sync_instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "PC_VI"

*Example*: 

.. code-block:: ruby

  keepalived['vrrp_sync_instance'] = "PC_VI"

.. index::
  pair: configuration; lb

lb
------------------------------------------------

.. index::
  triple: configuration; lb; api_fqdn

api_fqdn
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "ubuntu.localdomain"

*Example*: 

.. code-block:: ruby

  lb['api_fqdn'] = "ubuntu.localdomain"

.. index::
  triple: configuration; lb; cache_cookbook_files

cache_cookbook_files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  lb['cache_cookbook_files'] = false

.. index::
  triple: configuration; lb; debug

debug
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  lb['debug'] = false

.. index::
  triple: configuration; lb; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  lb['enable'] = true

.. index::
  triple: configuration; lb; upstream

upstream
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"opscode-chef"=>["127.0.0.1"],
 "opscode-erchef"=>["127.0.0.1"],
 "opscode-account"=>["127.0.0.1"],
 "opscode-webui"=>["127.0.0.1"],
 "opscode-authz"=>["127.0.0.1"],
 "opscode-solr"=>["127.0.0.1"]}


*Example*: 

.. code-block:: ruby

  lb['upstream'] = {"opscode-chef"=>["127.0.0.1"],
 "opscode-erchef"=>["127.0.0.1"],
 "opscode-account"=>["127.0.0.1"],
 "opscode-webui"=>["127.0.0.1"],
 "opscode-authz"=>["127.0.0.1"],
 "opscode-solr"=>["127.0.0.1"]}


.. index::
  triple: configuration; lb; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  lb['vip'] = "127.0.0.1"

.. index::
  triple: configuration; lb; web_ui_fqdn

web_ui_fqdn
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "ubuntu.localdomain"

*Example*: 

.. code-block:: ruby

  lb['web_ui_fqdn'] = "ubuntu.localdomain"

.. index::
  pair: configuration; lb_internal

lb_internal
------------------------------------------------

.. index::
  triple: configuration; lb_internal; account_port

account_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9685

*Example*: 

.. code-block:: ruby

  lb_internal['account_port'] = 9685

.. index::
  triple: configuration; lb_internal; authz_port

authz_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9683

*Example*: 

.. code-block:: ruby

  lb_internal['authz_port'] = 9683

.. index::
  triple: configuration; lb_internal; chef_port

chef_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9680

*Example*: 

.. code-block:: ruby

  lb_internal['chef_port'] = 9680

.. index::
  triple: configuration; lb_internal; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  lb_internal['enable'] = true

.. index::
  triple: configuration; lb_internal; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  lb_internal['vip'] = "127.0.0.1"

.. index::
  pair: configuration; mysql

mysql
------------------------------------------------

.. index::
  triple: configuration; mysql; destructive_migrate

destructive_migrate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  mysql['destructive_migrate'] = false

.. index::
  triple: configuration; mysql; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  mysql['enable'] = false

.. index::
  triple: configuration; mysql; install_libs

install_libs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  mysql['install_libs'] = true

.. index::
  triple: configuration; mysql; sql_password

sql_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "snakepliskin"

*Example*: 

.. code-block:: ruby

  mysql['sql_password'] = "snakepliskin"

.. index::
  triple: configuration; mysql; sql_user

sql_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "opscode_chef"

*Example*: 

.. code-block:: ruby

  mysql['sql_user'] = "opscode_chef"

.. index::
  triple: configuration; mysql; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  mysql['vip'] = "127.0.0.1"

.. index::
  pair: configuration; nagios

nagios
------------------------------------------------

.. index::
  triple: configuration; nagios; admin_email

admin_email
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "nobody@example.com"

*Example*: 

.. code-block:: ruby

  nagios['admin_email'] = "nobody@example.com"

.. index::
  triple: configuration; nagios; admin_pager

admin_pager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "nobody@example.com"

*Example*: 

.. code-block:: ruby

  nagios['admin_pager'] = "nobody@example.com"

.. index::
  triple: configuration; nagios; admin_password

admin_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "privatechef"

*Example*: 

.. code-block:: ruby

  nagios['admin_password'] = "privatechef"

.. index::
  triple: configuration; nagios; admin_user

admin_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "nagiosadmin"

*Example*: 

.. code-block:: ruby

  nagios['admin_user'] = "nagiosadmin"

.. index::
  triple: configuration; nagios; alert_email

alert_email
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "nobody@example.com"

*Example*: 

.. code-block:: ruby

  nagios['alert_email'] = "nobody@example.com"

.. index::
  triple: configuration; nagios; debug_level

debug_level
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  0

*Example*: 

.. code-block:: ruby

  nagios['debug_level'] = 0

.. index::
  triple: configuration; nagios; debug_verbosity

debug_verbosity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1

*Example*: 

.. code-block:: ruby

  nagios['debug_verbosity'] = 1

.. index::
  triple: configuration; nagios; default_host

default_host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"check_interval"=>15,
 "retry_interval"=>15,
 "max_check_attempts"=>1,
 "notification_interval"=>300}


*Example*: 

.. code-block:: ruby

  nagios['default_host'] = {"check_interval"=>15,
 "retry_interval"=>15,
 "max_check_attempts"=>1,
 "notification_interval"=>300}


.. index::
  triple: configuration; nagios; default_service

default_service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"check_interval"=>60,
 "retry_interval"=>15,
 "max_check_attempts"=>3,
 "notification_interval"=>1200}


*Example*: 

.. code-block:: ruby

  nagios['default_service'] = {"check_interval"=>60,
 "retry_interval"=>15,
 "max_check_attempts"=>3,
 "notification_interval"=>1200}


.. index::
  triple: configuration; nagios; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/nagios"

*Example*: 

.. code-block:: ruby

  nagios['dir'] = "/var/opt/opscode/nagios"

.. index::
  triple: configuration; nagios; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  nagios['enable'] = true

.. index::
  triple: configuration; nagios; fcgiwrap_log_directory

fcgiwrap_log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/fcgiwrap"

*Example*: 

.. code-block:: ruby

  nagios['fcgiwrap_log_directory'] = "/var/log/opscode/fcgiwrap"

.. index::
  triple: configuration; nagios; fcgiwrap_port

fcgiwrap_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9670

*Example*: 

.. code-block:: ruby

  nagios['fcgiwrap_port'] = 9670

.. index::
  triple: configuration; nagios; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  nagios['ha'] = false

.. index::
  triple: configuration; nagios; hosts

hosts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"ubuntu"=>{"ipaddress"=>"192.168.4.131", "hostgroups"=>[]}}


*Example*: 

.. code-block:: ruby

  nagios['hosts'] = {"ubuntu"=>{"ipaddress"=>"192.168.4.131", "hostgroups"=>[]}}


.. index::
  triple: configuration; nagios; interval_length

interval_length
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1

*Example*: 

.. code-block:: ruby

  nagios['interval_length'] = 1

.. index::
  triple: configuration; nagios; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/nagios"

*Example*: 

.. code-block:: ruby

  nagios['log_directory'] = "/var/log/opscode/nagios"

.. index::
  triple: configuration; nagios; php_fpm_log_directory

php_fpm_log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/php-fpm"

*Example*: 

.. code-block:: ruby

  nagios['php_fpm_log_directory'] = "/var/log/opscode/php-fpm"

.. index::
  triple: configuration; nagios; php_fpm_port

php_fpm_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9000

*Example*: 

.. code-block:: ruby

  nagios['php_fpm_port'] = 9000

.. index::
  triple: configuration; nagios; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9671

*Example*: 

.. code-block:: ruby

  nagios['port'] = 9671

.. index::
  pair: configuration; nginx

nginx
------------------------------------------------

.. index::
  triple: configuration; nginx; cache_max_size

cache_max_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "5000m"

*Example*: 

.. code-block:: ruby

  nginx['cache_max_size'] = "5000m"

.. index::
  triple: configuration; nginx; client_max_body_size

client_max_body_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "250m"

*Example*: 

.. code-block:: ruby

  nginx['client_max_body_size'] = "250m"

.. index::
  triple: configuration; nginx; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/nginx"

*Example*: 

.. code-block:: ruby

  nginx['dir'] = "/var/opt/opscode/nginx"

.. index::
  triple: configuration; nginx; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  nginx['enable'] = true

.. index::
  triple: configuration; nginx; gzip

gzip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "on"

*Example*: 

.. code-block:: ruby

  nginx['gzip'] = "on"

.. index::
  triple: configuration; nginx; gzip_comp_level

gzip_comp_level
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "2"

*Example*: 

.. code-block:: ruby

  nginx['gzip_comp_level'] = "2"

.. index::
  triple: configuration; nginx; gzip_http_version

gzip_http_version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "1.0"

*Example*: 

.. code-block:: ruby

  nginx['gzip_http_version'] = "1.0"

.. index::
  triple: configuration; nginx; gzip_proxied

gzip_proxied
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "any"

*Example*: 

.. code-block:: ruby

  nginx['gzip_proxied'] = "any"

.. index::
  triple: configuration; nginx; gzip_types

gzip_types
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  ["text/plain",
 "text/css",
 "application/x-javascript",
 "text/xml",
 "application/xml",
 "application/xml+rss",
 "text/javascript"]


*Example*: 

.. code-block:: ruby

  nginx['gzip_types'] = ["text/plain",
 "text/css",
 "application/x-javascript",
 "text/xml",
 "application/xml",
 "application/xml+rss",
 "text/javascript"]


.. index::
  triple: configuration; nginx; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  nginx['ha'] = false

.. index::
  triple: configuration; nginx; keepalive_timeout

keepalive_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  65

*Example*: 

.. code-block:: ruby

  nginx['keepalive_timeout'] = 65

.. index::
  triple: configuration; nginx; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/nginx"

*Example*: 

.. code-block:: ruby

  nginx['log_directory'] = "/var/log/opscode/nginx"

.. index::
  triple: configuration; nginx; sendfile

sendfile
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "on"

*Example*: 

.. code-block:: ruby

  nginx['sendfile'] = "on"

.. index::
  triple: configuration; nginx; server_name

server_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "ubuntu.localdomain"

*Example*: 

.. code-block:: ruby

  nginx['server_name'] = "ubuntu.localdomain"

.. index::
  triple: configuration; nginx; ssl_certificate

ssl_certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  nil

*Example*: 

.. code-block:: ruby

  nginx['ssl_certificate'] = nil

.. index::
  triple: configuration; nginx; ssl_certificate_key

ssl_certificate_key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  nil

*Example*: 

.. code-block:: ruby

  nginx['ssl_certificate_key'] = nil

.. index::
  triple: configuration; nginx; ssl_ciphers

ssl_ciphers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "RC4-SHA:RC4-MD5:RC4:RSA:HIGH:MEDIUM:!LOW:!kEDH:!aNULL:!ADH:!eNULL:!EXP:!SSLv2:!SEED:!CAMELLIA:!PSK"

*Example*: 

.. code-block:: ruby

  nginx['ssl_ciphers'] = "RC4-SHA:RC4-MD5:RC4:RSA:HIGH:MEDIUM:!LOW:!kEDH:!aNULL:!ADH:!eNULL:!EXP:!SSLv2:!SEED:!CAMELLIA:!PSK"

.. index::
  triple: configuration; nginx; ssl_company_name

ssl_company_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "YouCorp"

*Example*: 

.. code-block:: ruby

  nginx['ssl_company_name'] = "YouCorp"

.. index::
  triple: configuration; nginx; ssl_country_name

ssl_country_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "US"

*Example*: 

.. code-block:: ruby

  nginx['ssl_country_name'] = "US"

.. index::
  triple: configuration; nginx; ssl_email_address

ssl_email_address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "you@example.com"

*Example*: 

.. code-block:: ruby

  nginx['ssl_email_address'] = "you@example.com"

.. index::
  triple: configuration; nginx; ssl_locality_name

ssl_locality_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "Seattle"

*Example*: 

.. code-block:: ruby

  nginx['ssl_locality_name'] = "Seattle"

.. index::
  triple: configuration; nginx; ssl_organizational_unit_name

ssl_organizational_unit_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "Operations"

*Example*: 

.. code-block:: ruby

  nginx['ssl_organizational_unit_name'] = "Operations"

.. index::
  triple: configuration; nginx; ssl_port

ssl_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  443

*Example*: 

.. code-block:: ruby

  nginx['ssl_port'] = 443

.. index::
  triple: configuration; nginx; ssl_protocols

ssl_protocols
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "SSLv3 TLSv1"

*Example*: 

.. code-block:: ruby

  nginx['ssl_protocols'] = "SSLv3 TLSv1"

.. index::
  triple: configuration; nginx; ssl_state_name

ssl_state_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "WA"

*Example*: 

.. code-block:: ruby

  nginx['ssl_state_name'] = "WA"

.. index::
  triple: configuration; nginx; tcp_nodelay

tcp_nodelay
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "on"

*Example*: 

.. code-block:: ruby

  nginx['tcp_nodelay'] = "on"

.. index::
  triple: configuration; nginx; tcp_nopush

tcp_nopush
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "on"

*Example*: 

.. code-block:: ruby

  nginx['tcp_nopush'] = "on"

.. index::
  triple: configuration; nginx; url

url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "https://ubuntu.localdomain"

*Example*: 

.. code-block:: ruby

  nginx['url'] = "https://ubuntu.localdomain"

.. index::
  triple: configuration; nginx; worker_connections

worker_connections
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  10240

*Example*: 

.. code-block:: ruby

  nginx['worker_connections'] = 10240

.. index::
  triple: configuration; nginx; worker_processes

worker_processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4

*Example*: 

.. code-block:: ruby

  nginx['worker_processes'] = 4

.. index::
  pair: configuration; notification_email

notification_email
------------------------------------------------

.. index::
  triple: configuration; general options; notification_email

notification_email
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby 

  pc-default@opscode.com

*Example*: 

.. code-block:: ruby

  notification_email "pc-default@opscode.com"

.. index::
  pair: configuration; nrpe

nrpe
------------------------------------------------

.. index::
  triple: configuration; nrpe; allowed_hosts

allowed_hosts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  ["127.0.0.1", "192.168.4.131"]


*Example*: 

.. code-block:: ruby

  nrpe['allowed_hosts'] = ["127.0.0.1", "192.168.4.131"]


.. index::
  triple: configuration; nrpe; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/nrpe"

*Example*: 

.. code-block:: ruby

  nrpe['dir'] = "/var/opt/opscode/nrpe"

.. index::
  triple: configuration; nrpe; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  nrpe['enable'] = true

.. index::
  triple: configuration; nrpe; listen

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "192.168.4.131"

*Example*: 

.. code-block:: ruby

  nrpe['listen'] = "192.168.4.131"

.. index::
  triple: configuration; nrpe; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/nrpe"

*Example*: 

.. code-block:: ruby

  nrpe['log_directory'] = "/var/log/opscode/nrpe"

.. index::
  triple: configuration; nrpe; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9672

*Example*: 

.. code-block:: ruby

  nrpe['port'] = 9672

.. index::
  pair: configuration; opscode_account

opscode_account
------------------------------------------------

.. index::
  triple: configuration; opscode_account; backlog

backlog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1024

*Example*: 

.. code-block:: ruby

  opscode_account['backlog'] = 1024

.. index::
  triple: configuration; opscode_account; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-account"

*Example*: 

.. code-block:: ruby

  opscode_account['dir'] = "/var/opt/opscode/opscode-account"

.. index::
  triple: configuration; opscode_account; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_account['enable'] = true

.. index::
  triple: configuration; opscode_account; environment

environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "privatechef"

*Example*: 

.. code-block:: ruby

  opscode_account['environment'] = "privatechef"

.. index::
  triple: configuration; opscode_account; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_account['ha'] = false

.. index::
  triple: configuration; opscode_account; listen

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1:9465"

*Example*: 

.. code-block:: ruby

  opscode_account['listen'] = "127.0.0.1:9465"

.. index::
  triple: configuration; opscode_account; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-account"

*Example*: 

.. code-block:: ruby

  opscode_account['log_directory'] = "/var/log/opscode/opscode-account"

.. index::
  triple: configuration; opscode_account; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9465

*Example*: 

.. code-block:: ruby

  opscode_account['port'] = 9465

.. index::
  triple: configuration; opscode_account; proxy_user

proxy_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "pivotal"

*Example*: 

.. code-block:: ruby

  opscode_account['proxy_user'] = "pivotal"

.. index::
  triple: configuration; opscode_account; session_secret_key

session_secret_key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "change-by-default"

*Example*: 

.. code-block:: ruby

  opscode_account['session_secret_key'] = "change-by-default"

.. index::
  triple: configuration; opscode_account; tcp_nodelay

tcp_nodelay
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_account['tcp_nodelay'] = true

.. index::
  triple: configuration; opscode_account; umask

umask
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "0022"

*Example*: 

.. code-block:: ruby

  opscode_account['umask'] = "0022"

.. index::
  triple: configuration; opscode_account; url

url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http://127.0.0.1:9465"

*Example*: 

.. code-block:: ruby

  opscode_account['url'] = "http://127.0.0.1:9465"

.. index::
  triple: configuration; opscode_account; validation_client_name

validation_client_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chef"

*Example*: 

.. code-block:: ruby

  opscode_account['validation_client_name'] = "chef"

.. index::
  triple: configuration; opscode_account; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_account['vip'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_account; worker_processes

worker_processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4

*Example*: 

.. code-block:: ruby

  opscode_account['worker_processes'] = 4

.. index::
  triple: configuration; opscode_account; worker_timeout

worker_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  3600

*Example*: 

.. code-block:: ruby

  opscode_account['worker_timeout'] = 3600

.. index::
  pair: configuration; opscode_authz

opscode_authz
------------------------------------------------

.. index::
  triple: configuration; opscode_authz; caching

caching
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "enabled"

*Example*: 

.. code-block:: ruby

  opscode_authz['caching'] = "enabled"

.. index::
  triple: configuration; opscode_authz; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-authz"

*Example*: 

.. code-block:: ruby

  opscode_authz['dir'] = "/var/opt/opscode/opscode-authz"

.. index::
  triple: configuration; opscode_authz; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_authz['enable'] = true

.. index::
  triple: configuration; opscode_authz; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_authz['ha'] = false

.. index::
  triple: configuration; opscode_authz; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-authz"

*Example*: 

.. code-block:: ruby

  opscode_authz['log_directory'] = "/var/log/opscode/opscode-authz"

.. index::
  triple: configuration; opscode_authz; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9463

*Example*: 

.. code-block:: ruby

  opscode_authz['port'] = 9463

.. index::
  triple: configuration; opscode_authz; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_authz['vip'] = "127.0.0.1"

.. index::
  pair: configuration; opscode_certificate

opscode_certificate
------------------------------------------------

.. index::
  triple: configuration; opscode_certificate; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-certificate"

*Example*: 

.. code-block:: ruby

  opscode_certificate['dir'] = "/var/opt/opscode/opscode-certificate"

.. index::
  triple: configuration; opscode_certificate; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_certificate['enable'] = true

.. index::
  triple: configuration; opscode_certificate; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_certificate['ha'] = false

.. index::
  triple: configuration; opscode_certificate; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-certificate"

*Example*: 

.. code-block:: ruby

  opscode_certificate['log_directory'] = "/var/log/opscode/opscode-certificate"

.. index::
  triple: configuration; opscode_certificate; num_certificates_per_worker

num_certificates_per_worker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "50"

*Example*: 

.. code-block:: ruby

  opscode_certificate['num_certificates_per_worker'] = "50"

.. index::
  triple: configuration; opscode_certificate; num_workers

num_workers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "2"

*Example*: 

.. code-block:: ruby

  opscode_certificate['num_workers'] = "2"

.. index::
  triple: configuration; opscode_certificate; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  5140

*Example*: 

.. code-block:: ruby

  opscode_certificate['port'] = 5140

.. index::
  triple: configuration; opscode_certificate; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_certificate['vip'] = "127.0.0.1"

.. index::
  pair: configuration; opscode_chef

opscode_chef
------------------------------------------------

.. index::
  triple: configuration; opscode_chef; backlog

backlog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1024

*Example*: 

.. code-block:: ruby

  opscode_chef['backlog'] = 1024

.. index::
  triple: configuration; opscode_chef; checksum_path

checksum_path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-chef/checksum"

*Example*: 

.. code-block:: ruby

  opscode_chef['checksum_path'] = "/var/opt/opscode/opscode-chef/checksum"

.. index::
  triple: configuration; opscode_chef; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-chef"

*Example*: 

.. code-block:: ruby

  opscode_chef['dir'] = "/var/opt/opscode/opscode-chef"

.. index::
  triple: configuration; opscode_chef; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_chef['enable'] = true

.. index::
  triple: configuration; opscode_chef; environment

environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "privatechef"

*Example*: 

.. code-block:: ruby

  opscode_chef['environment'] = "privatechef"

.. index::
  triple: configuration; opscode_chef; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_chef['ha'] = false

.. index::
  triple: configuration; opscode_chef; listen

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1:9460"

*Example*: 

.. code-block:: ruby

  opscode_chef['listen'] = "127.0.0.1:9460"

.. index::
  triple: configuration; opscode_chef; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-chef"

*Example*: 

.. code-block:: ruby

  opscode_chef['log_directory'] = "/var/log/opscode/opscode-chef"

.. index::
  triple: configuration; opscode_chef; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9460

*Example*: 

.. code-block:: ruby

  opscode_chef['port'] = 9460

.. index::
  triple: configuration; opscode_chef; proxy_user

proxy_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "pivotal"

*Example*: 

.. code-block:: ruby

  opscode_chef['proxy_user'] = "pivotal"

.. index::
  triple: configuration; opscode_chef; sandbox_path

sandbox_path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-chef/sandbox"

*Example*: 

.. code-block:: ruby

  opscode_chef['sandbox_path'] = "/var/opt/opscode/opscode-chef/sandbox"

.. index::
  triple: configuration; opscode_chef; tcp_nodelay

tcp_nodelay
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_chef['tcp_nodelay'] = true

.. index::
  triple: configuration; opscode_chef; umask

umask
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "0022"

*Example*: 

.. code-block:: ruby

  opscode_chef['umask'] = "0022"

.. index::
  triple: configuration; opscode_chef; upload_internal_port

upload_internal_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9460

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_internal_port'] = 9460

.. index::
  triple: configuration; opscode_chef; upload_internal_proto

upload_internal_proto
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http"

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_internal_proto'] = "http"

.. index::
  triple: configuration; opscode_chef; upload_internal_vip

upload_internal_vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_internal_vip'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_chef; upload_port

upload_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9460

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_port'] = 9460

.. index::
  triple: configuration; opscode_chef; upload_proto

upload_proto
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http"

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_proto'] = "http"

.. index::
  triple: configuration; opscode_chef; upload_vip

upload_vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_chef['upload_vip'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_chef; url

url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http://127.0.0.1:9460"

*Example*: 

.. code-block:: ruby

  opscode_chef['url'] = "http://127.0.0.1:9460"

.. index::
  triple: configuration; opscode_chef; validation_client_name

validation_client_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chef"

*Example*: 

.. code-block:: ruby

  opscode_chef['validation_client_name'] = "chef"

.. index::
  triple: configuration; opscode_chef; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_chef['vip'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_chef; web_ui_admin_default_password

web_ui_admin_default_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "p@ssw0rd1"

*Example*: 

.. code-block:: ruby

  opscode_chef['web_ui_admin_default_password'] = "p@ssw0rd1"

.. index::
  triple: configuration; opscode_chef; web_ui_admin_user_name

web_ui_admin_user_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "admin"

*Example*: 

.. code-block:: ruby

  opscode_chef['web_ui_admin_user_name'] = "admin"

.. index::
  triple: configuration; opscode_chef; web_ui_client_name

web_ui_client_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chef-webui"

*Example*: 

.. code-block:: ruby

  opscode_chef['web_ui_client_name'] = "chef-webui"

.. index::
  triple: configuration; opscode_chef; worker_processes

worker_processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4

*Example*: 

.. code-block:: ruby

  opscode_chef['worker_processes'] = 4

.. index::
  triple: configuration; opscode_chef; worker_timeout

worker_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  3600

*Example*: 

.. code-block:: ruby

  opscode_chef['worker_timeout'] = 3600

.. index::
  pair: configuration; opscode_erchef

opscode_erchef
------------------------------------------------

.. index::
  triple: configuration; opscode_erchef; auth_skew

auth_skew
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "900"

*Example*: 

.. code-block:: ruby

  opscode_erchef['auth_skew'] = "900"

.. index::
  triple: configuration; opscode_erchef; bulk_fetch_batch_size

bulk_fetch_batch_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "5"

*Example*: 

.. code-block:: ruby

  opscode_erchef['bulk_fetch_batch_size'] = "5"

.. index::
  triple: configuration; opscode_erchef; cache_ttl

cache_ttl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "3600"

*Example*: 

.. code-block:: ruby

  opscode_erchef['cache_ttl'] = "3600"

.. index::
  triple: configuration; opscode_erchef; couchdb_max_conn

couchdb_max_conn
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "100"

*Example*: 

.. code-block:: ruby

  opscode_erchef['couchdb_max_conn'] = "100"

.. index::
  triple: configuration; opscode_erchef; db_pool_size

db_pool_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "20"

*Example*: 

.. code-block:: ruby

  opscode_erchef['db_pool_size'] = "20"

.. index::
  triple: configuration; opscode_erchef; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-erchef"

*Example*: 

.. code-block:: ruby

  opscode_erchef['dir'] = "/var/opt/opscode/opscode-erchef"

.. index::
  triple: configuration; opscode_erchef; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_erchef['enable'] = true

.. index::
  triple: configuration; opscode_erchef; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_erchef['ha'] = false

.. index::
  triple: configuration; opscode_erchef; listen

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_erchef['listen'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_erchef; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-erchef"

*Example*: 

.. code-block:: ruby

  opscode_erchef['log_directory'] = "/var/log/opscode/opscode-erchef"

.. index::
  triple: configuration; opscode_erchef; max_cache_size

max_cache_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "10000"

*Example*: 

.. code-block:: ruby

  opscode_erchef['max_cache_size'] = "10000"

.. index::
  triple: configuration; opscode_erchef; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  8000

*Example*: 

.. code-block:: ruby

  opscode_erchef['port'] = 8000

.. index::
  triple: configuration; opscode_erchef; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_erchef['vip'] = "127.0.0.1"

.. index::
  pair: configuration; opscode_expander

opscode_expander
------------------------------------------------

.. index::
  triple: configuration; opscode_expander; consumer_id

consumer_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "default"

*Example*: 

.. code-block:: ruby

  opscode_expander['consumer_id'] = "default"

.. index::
  triple: configuration; opscode_expander; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-expander"

*Example*: 

.. code-block:: ruby

  opscode_expander['dir'] = "/var/opt/opscode/opscode-expander"

.. index::
  triple: configuration; opscode_expander; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_expander['enable'] = true

.. index::
  triple: configuration; opscode_expander; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_expander['ha'] = false

.. index::
  triple: configuration; opscode_expander; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-expander"

*Example*: 

.. code-block:: ruby

  opscode_expander['log_directory'] = "/var/log/opscode/opscode-expander"

.. index::
  triple: configuration; opscode_expander; nodes

nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  2

*Example*: 

.. code-block:: ruby

  opscode_expander['nodes'] = 2

.. index::
  triple: configuration; opscode_expander; reindexer_log_directory

reindexer_log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-expander-reindexer"

*Example*: 

.. code-block:: ruby

  opscode_expander['reindexer_log_directory'] = "/var/log/opscode/opscode-expander-reindexer"

.. index::
  pair: configuration; opscode_org_creator

opscode_org_creator
------------------------------------------------

.. index::
  triple: configuration; opscode_org_creator; create_splay_ms

create_splay_ms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  25000

*Example*: 

.. code-block:: ruby

  opscode_org_creator['create_splay_ms'] = 25000

.. index::
  triple: configuration; opscode_org_creator; create_wait_ms

create_wait_ms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  30000

*Example*: 

.. code-block:: ruby

  opscode_org_creator['create_wait_ms'] = 30000

.. index::
  triple: configuration; opscode_org_creator; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-org-creator"

*Example*: 

.. code-block:: ruby

  opscode_org_creator['dir'] = "/var/opt/opscode/opscode-org-creator"

.. index::
  triple: configuration; opscode_org_creator; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_org_creator['enable'] = true

.. index::
  triple: configuration; opscode_org_creator; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_org_creator['ha'] = false

.. index::
  triple: configuration; opscode_org_creator; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-org-creator"

*Example*: 

.. code-block:: ruby

  opscode_org_creator['log_directory'] = "/var/log/opscode/opscode-org-creator"

.. index::
  triple: configuration; opscode_org_creator; max_workers

max_workers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1

*Example*: 

.. code-block:: ruby

  opscode_org_creator['max_workers'] = 1

.. index::
  triple: configuration; opscode_org_creator; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4369

*Example*: 

.. code-block:: ruby

  opscode_org_creator['port'] = 4369

.. index::
  triple: configuration; opscode_org_creator; ready_org_depth

ready_org_depth
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  10

*Example*: 

.. code-block:: ruby

  opscode_org_creator['ready_org_depth'] = 10

.. index::
  pair: configuration; opscode_solr

opscode_solr
------------------------------------------------

.. index::
  triple: configuration; opscode_solr; commit_interval

commit_interval
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  60000

*Example*: 

.. code-block:: ruby

  opscode_solr['commit_interval'] = 60000

.. index::
  triple: configuration; opscode_solr; data_dir

data_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-solr/data"

*Example*: 

.. code-block:: ruby

  opscode_solr['data_dir'] = "/var/opt/opscode/opscode-solr/data"

.. index::
  triple: configuration; opscode_solr; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-solr"

*Example*: 

.. code-block:: ruby

  opscode_solr['dir'] = "/var/opt/opscode/opscode-solr"

.. index::
  triple: configuration; opscode_solr; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_solr['enable'] = true

.. index::
  triple: configuration; opscode_solr; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_solr['ha'] = false

.. index::
  triple: configuration; opscode_solr; heap_size

heap_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "256M"

*Example*: 

.. code-block:: ruby

  opscode_solr['heap_size'] = "256M"

.. index::
  triple: configuration; opscode_solr; ip_address

ip_address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_solr['ip_address'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_solr; java_opts

java_opts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  ""

*Example*: 

.. code-block:: ruby

  opscode_solr['java_opts'] = ""

.. index::
  triple: configuration; opscode_solr; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-solr"

*Example*: 

.. code-block:: ruby

  opscode_solr['log_directory'] = "/var/log/opscode/opscode-solr"

.. index::
  triple: configuration; opscode_solr; max_commit_docs

max_commit_docs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1000

*Example*: 

.. code-block:: ruby

  opscode_solr['max_commit_docs'] = 1000

.. index::
  triple: configuration; opscode_solr; max_field_length

max_field_length
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  100000

*Example*: 

.. code-block:: ruby

  opscode_solr['max_field_length'] = 100000

.. index::
  triple: configuration; opscode_solr; max_merge_docs

max_merge_docs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  2147483647

*Example*: 

.. code-block:: ruby

  opscode_solr['max_merge_docs'] = 2147483647

.. index::
  triple: configuration; opscode_solr; merge_factor

merge_factor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  100

*Example*: 

.. code-block:: ruby

  opscode_solr['merge_factor'] = 100

.. index::
  triple: configuration; opscode_solr; poll_seconds

poll_seconds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  20

*Example*: 

.. code-block:: ruby

  opscode_solr['poll_seconds'] = 20

.. index::
  triple: configuration; opscode_solr; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  8983

*Example*: 

.. code-block:: ruby

  opscode_solr['port'] = 8983

.. index::
  triple: configuration; opscode_solr; ram_buffer_size

ram_buffer_size
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  200

*Example*: 

.. code-block:: ruby

  opscode_solr['ram_buffer_size'] = 200

.. index::
  triple: configuration; opscode_solr; url

url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http://localhost:8983"

*Example*: 

.. code-block:: ruby

  opscode_solr['url'] = "http://localhost:8983"

.. index::
  triple: configuration; opscode_solr; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_solr['vip'] = "127.0.0.1"

.. index::
  pair: configuration; opscode_webui

opscode_webui
------------------------------------------------

.. index::
  triple: configuration; opscode_webui; backlog

backlog
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  1024

*Example*: 

.. code-block:: ruby

  opscode_webui['backlog'] = 1024

.. index::
  triple: configuration; opscode_webui; cookie_domain

cookie_domain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "all"

*Example*: 

.. code-block:: ruby

  opscode_webui['cookie_domain'] = "all"

.. index::
  triple: configuration; opscode_webui; cookie_secret

cookie_secret
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "47b3b8d95dea455baf32155e95d1e64e"

*Example*: 

.. code-block:: ruby

  opscode_webui['cookie_secret'] = "47b3b8d95dea455baf32155e95d1e64e"

.. index::
  triple: configuration; opscode_webui; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/opscode-webui"

*Example*: 

.. code-block:: ruby

  opscode_webui['dir'] = "/var/opt/opscode/opscode-webui"

.. index::
  triple: configuration; opscode_webui; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_webui['enable'] = true

.. index::
  triple: configuration; opscode_webui; environment

environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "privatechef"

*Example*: 

.. code-block:: ruby

  opscode_webui['environment'] = "privatechef"

.. index::
  triple: configuration; opscode_webui; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  opscode_webui['ha'] = false

.. index::
  triple: configuration; opscode_webui; listen

listen
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1:9462"

*Example*: 

.. code-block:: ruby

  opscode_webui['listen'] = "127.0.0.1:9462"

.. index::
  triple: configuration; opscode_webui; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/opscode-webui"

*Example*: 

.. code-block:: ruby

  opscode_webui['log_directory'] = "/var/log/opscode/opscode-webui"

.. index::
  triple: configuration; opscode_webui; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  9462

*Example*: 

.. code-block:: ruby

  opscode_webui['port'] = 9462

.. index::
  triple: configuration; opscode_webui; session_key

session_key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "_sandbox_session"

*Example*: 

.. code-block:: ruby

  opscode_webui['session_key'] = "_sandbox_session"

.. index::
  triple: configuration; opscode_webui; tcp_nodelay

tcp_nodelay
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  opscode_webui['tcp_nodelay'] = true

.. index::
  triple: configuration; opscode_webui; umask

umask
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "0022"

*Example*: 

.. code-block:: ruby

  opscode_webui['umask'] = "0022"

.. index::
  triple: configuration; opscode_webui; url

url
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "http://127.0.0.1:9462"

*Example*: 

.. code-block:: ruby

  opscode_webui['url'] = "http://127.0.0.1:9462"

.. index::
  triple: configuration; opscode_webui; validation_client_name

validation_client_name
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chef"

*Example*: 

.. code-block:: ruby

  opscode_webui['validation_client_name'] = "chef"

.. index::
  triple: configuration; opscode_webui; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  opscode_webui['vip'] = "127.0.0.1"

.. index::
  triple: configuration; opscode_webui; worker_processes

worker_processes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4

*Example*: 

.. code-block:: ruby

  opscode_webui['worker_processes'] = 4

.. index::
  triple: configuration; opscode_webui; worker_timeout

worker_timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  3600

*Example*: 

.. code-block:: ruby

  opscode_webui['worker_timeout'] = 3600

.. index::
  pair: configuration; postgresql

postgresql
------------------------------------------------

.. index::
  triple: configuration; postgresql; data_dir

data_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/postgresql/data"

*Example*: 

.. code-block:: ruby

  postgresql['data_dir'] = "/var/opt/opscode/postgresql/data"

.. index::
  triple: configuration; postgresql; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/postgresql"

*Example*: 

.. code-block:: ruby

  postgresql['dir'] = "/var/opt/opscode/postgresql"

.. index::
  triple: configuration; postgresql; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  postgresql['enable'] = true

.. index::
  triple: configuration; postgresql; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  postgresql['ha'] = false

.. index::
  triple: configuration; postgresql; home

home
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/opt/opscode/embedded"

*Example*: 

.. code-block:: ruby

  postgresql['home'] = "/opt/opscode/embedded"

.. index::
  triple: configuration; postgresql; listen_address

listen_address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "localhost"

*Example*: 

.. code-block:: ruby

  postgresql['listen_address'] = "localhost"

.. index::
  triple: configuration; postgresql; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/postgresql"

*Example*: 

.. code-block:: ruby

  postgresql['log_directory'] = "/var/log/opscode/postgresql"

.. index::
  triple: configuration; postgresql; max_connections

max_connections
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  200

*Example*: 

.. code-block:: ruby

  postgresql['max_connections'] = 200

.. index::
  triple: configuration; postgresql; md5_auth_cidr_addresses

md5_auth_cidr_addresses
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  []


*Example*: 

.. code-block:: ruby

  postgresql['md5_auth_cidr_addresses'] = []


.. index::
  triple: configuration; postgresql; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  5432

*Example*: 

.. code-block:: ruby

  postgresql['port'] = 5432

.. index::
  triple: configuration; postgresql; shell

shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/bin/sh"

*Example*: 

.. code-block:: ruby

  postgresql['shell'] = "/bin/sh"

.. index::
  triple: configuration; postgresql; shmall

shmall
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  4194304

*Example*: 

.. code-block:: ruby

  postgresql['shmall'] = 4194304

.. index::
  triple: configuration; postgresql; shmmax

shmmax
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  17179869184

*Example*: 

.. code-block:: ruby

  postgresql['shmmax'] = 17179869184

.. index::
  triple: configuration; postgresql; sql_password

sql_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "snakepliskin"

*Example*: 

.. code-block:: ruby

  postgresql['sql_password'] = "snakepliskin"

.. index::
  triple: configuration; postgresql; sql_ro_password

sql_ro_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "shmunzeltazzen"

*Example*: 

.. code-block:: ruby

  postgresql['sql_ro_password'] = "shmunzeltazzen"

.. index::
  triple: configuration; postgresql; sql_ro_user

sql_ro_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "opscode_chef_ro"

*Example*: 

.. code-block:: ruby

  postgresql['sql_ro_user'] = "opscode_chef_ro"

.. index::
  triple: configuration; postgresql; sql_user

sql_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "opscode_chef"

*Example*: 

.. code-block:: ruby

  postgresql['sql_user'] = "opscode_chef"

.. index::
  triple: configuration; postgresql; trust_auth_cidr_addresses

trust_auth_cidr_addresses
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  ["127.0.0.1/32", "::1/128"]


*Example*: 

.. code-block:: ruby

  postgresql['trust_auth_cidr_addresses'] = ["127.0.0.1/32", "::1/128"]


.. index::
  triple: configuration; postgresql; username

username
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "opscode-pgsql"

*Example*: 

.. code-block:: ruby

  postgresql['username'] = "opscode-pgsql"

.. index::
  triple: configuration; postgresql; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  postgresql['vip'] = "127.0.0.1"

.. index::
  pair: configuration; rabbitmq

rabbitmq
------------------------------------------------

.. index::
  triple: configuration; rabbitmq; consumer_id

consumer_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "hotsauce"

*Example*: 

.. code-block:: ruby

  rabbitmq['consumer_id'] = "hotsauce"

.. index::
  triple: configuration; rabbitmq; data_dir

data_dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/rabbitmq/db"

*Example*: 

.. code-block:: ruby

  rabbitmq['data_dir'] = "/var/opt/opscode/rabbitmq/db"

.. index::
  triple: configuration; rabbitmq; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/rabbitmq"

*Example*: 

.. code-block:: ruby

  rabbitmq['dir'] = "/var/opt/opscode/rabbitmq"

.. index::
  triple: configuration; rabbitmq; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  rabbitmq['enable'] = true

.. index::
  triple: configuration; rabbitmq; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  rabbitmq['ha'] = false

.. index::
  triple: configuration; rabbitmq; jobs_password

jobs_password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "workcomplete"

*Example*: 

.. code-block:: ruby

  rabbitmq['jobs_password'] = "workcomplete"

.. index::
  triple: configuration; rabbitmq; jobs_user

jobs_user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "jobs"

*Example*: 

.. code-block:: ruby

  rabbitmq['jobs_user'] = "jobs"

.. index::
  triple: configuration; rabbitmq; jobs_vhost

jobs_vhost
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/jobs"

*Example*: 

.. code-block:: ruby

  rabbitmq['jobs_vhost'] = "/jobs"

.. index::
  triple: configuration; rabbitmq; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/rabbitmq"

*Example*: 

.. code-block:: ruby

  rabbitmq['log_directory'] = "/var/log/opscode/rabbitmq"

.. index::
  triple: configuration; rabbitmq; node_ip_address

node_ip_address
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  rabbitmq['node_ip_address'] = "127.0.0.1"

.. index::
  triple: configuration; rabbitmq; node_port

node_port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "5672"

*Example*: 

.. code-block:: ruby

  rabbitmq['node_port'] = "5672"

.. index::
  triple: configuration; rabbitmq; nodename

nodename
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "rabbit@localhost"

*Example*: 

.. code-block:: ruby

  rabbitmq['nodename'] = "rabbit@localhost"

.. index::
  triple: configuration; rabbitmq; password

password
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chefrocks"

*Example*: 

.. code-block:: ruby

  rabbitmq['password'] = "chefrocks"

.. index::
  triple: configuration; rabbitmq; reindexer_vhost

reindexer_vhost
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/reindexer"

*Example*: 

.. code-block:: ruby

  rabbitmq['reindexer_vhost'] = "/reindexer"

.. index::
  triple: configuration; rabbitmq; user

user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "chef"

*Example*: 

.. code-block:: ruby

  rabbitmq['user'] = "chef"

.. index::
  triple: configuration; rabbitmq; vhost

vhost
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/chef"

*Example*: 

.. code-block:: ruby

  rabbitmq['vhost'] = "/chef"

.. index::
  triple: configuration; rabbitmq; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  rabbitmq['vip'] = "127.0.0.1"

.. index::
  pair: configuration; redis

redis
------------------------------------------------

.. index::
  triple: configuration; redis; appendfsync

appendfsync
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "everysec"

*Example*: 

.. code-block:: ruby

  redis['appendfsync'] = "everysec"

.. index::
  triple: configuration; redis; appendonly

appendonly
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "no"

*Example*: 

.. code-block:: ruby

  redis['appendonly'] = "no"

.. index::
  triple: configuration; redis; bind

bind
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  redis['bind'] = "127.0.0.1"

.. index::
  triple: configuration; redis; databases

databases
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "16"

*Example*: 

.. code-block:: ruby

  redis['databases'] = "16"

.. index::
  triple: configuration; redis; dir

dir
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/redis"

*Example*: 

.. code-block:: ruby

  redis['dir'] = "/var/opt/opscode/redis"

.. index::
  triple: configuration; redis; enable

enable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  true

*Example*: 

.. code-block:: ruby

  redis['enable'] = true

.. index::
  triple: configuration; redis; ha

ha
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  false

*Example*: 

.. code-block:: ruby

  redis['ha'] = false

.. index::
  triple: configuration; redis; log_directory

log_directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/log/opscode/redis"

*Example*: 

.. code-block:: ruby

  redis['log_directory'] = "/var/log/opscode/redis"

.. index::
  triple: configuration; redis; loglevel

loglevel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "notice"

*Example*: 

.. code-block:: ruby

  redis['loglevel'] = "notice"

.. index::
  triple: configuration; redis; maxmemory

maxmemory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "1g"

*Example*: 

.. code-block:: ruby

  redis['maxmemory'] = "1g"

.. index::
  triple: configuration; redis; maxmemory_policy

maxmemory_policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "volatile-lru"

*Example*: 

.. code-block:: ruby

  redis['maxmemory_policy'] = "volatile-lru"

.. index::
  triple: configuration; redis; port

port
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "6379"

*Example*: 

.. code-block:: ruby

  redis['port'] = "6379"

.. index::
  triple: configuration; redis; root

root
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/var/opt/opscode/redis"

*Example*: 

.. code-block:: ruby

  redis['root'] = "/var/opt/opscode/redis"

.. index::
  triple: configuration; redis; timeout

timeout
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "300"

*Example*: 

.. code-block:: ruby

  redis['timeout'] = "300"

.. index::
  triple: configuration; redis; vip

vip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "127.0.0.1"

*Example*: 

.. code-block:: ruby

  redis['vip'] = "127.0.0.1"

.. index::
  triple: configuration; redis; vm

vm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  {"enabled"=>"no",
 "max_memory"=>"0",
 "page_size"=>"32",
 "pages"=>"134217728",
 "max_threads"=>"4"}


*Example*: 

.. code-block:: ruby

  redis['vm'] = {"enabled"=>"no",
 "max_memory"=>"0",
 "page_size"=>"32",
 "pages"=>"134217728",
 "max_threads"=>"4"}


.. index::
  pair: configuration; user

user
------------------------------------------------

.. index::
  triple: configuration; user; home

home
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/opt/opscode/embedded"

*Example*: 

.. code-block:: ruby

  user['home'] = "/opt/opscode/embedded"

.. index::
  triple: configuration; user; shell

shell
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "/bin/sh"

*Example*: 

.. code-block:: ruby

  user['shell'] = "/bin/sh"

.. index::
  triple: configuration; user; username

username
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Default Value*: 

.. code-block:: ruby

  "opscode"

*Example*: 

.. code-block:: ruby

  user['username'] = "opscode"

