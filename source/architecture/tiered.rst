.. index::
  single: tiered

==========
Tiered
==========

In a Tiered topology, your Private Chef environment will run on multiple servers, but the backend datastore layer will live on a single host. No highly available data storage is provide on the backend host, though the frontend layer can be scaled horizontally.  This topology may be of interest for environments that wish to provide their own methods for backup and data redundancy. 

Opscode recommends that most environments considering a Tiered topology should consider using the :doc:`High Availability </architecture/high_availability>` topology for data redundancy, which is supported by Opscode Support.

.. index::
  pair: tiered; hosts

Hosts
-----

The Tiered topology is assumed to be used in a production environment.  Small hosts can be used for the frontend layer if necessary, but the backend host in particular should be a production grade system with at least 8 cores and 16GB of RAM. The full specification recommendations are listed in the :doc:`installation guide </installation/tiered>`.

.. index::
  pair: tiered; backups

Backups
-------

Tiered Private Chef is expected to be a production configuration; so we'd expect you to back up the data in the backend datastore on a regular basis.  This can be accomplished using LVM snapshots or with a third-party backup solution supported by your team.  The frontend web tier does not retain data, and for most environments we expect that reinstalling a web host would be easier than restoring from a backup.

The keys that the web tier is required to use to access the data tier are stored in ``/etc/opscode``.  These files must be copied from the backend host to any frontend that will be accessing it.  Your backup plans should include keeping a copy of these files available in case the backend has to be restored.

The rest of the data written and used by the Private Chef server is stored in ``/var/opt/opscode`` on your backend host.

Without a backup scheme, losing the backend server will require you to recreate your organizations, users, and user pem files, in addition to re-uploading your cookbooks and other files from your source code management system.  

.. index:: 
  pair: tiered; scaling

Scaling
-------

The tiered topology for Private Chef is designed to give you some flexibility around the way you scale your Private Chef server.  The frontend can be scaled horizontally to N hosts for better API availability.  The single backend server can be vertically scaled for IO performance if needed.
