=========
orgmapper
=========

:command:`orgmapper` is a tool based on :command:`irb` (the ruby REPL) which provides
administrative access to the many of the back-end objects in Private Chef.

Starting orgmapper
------------------

Login to the server that is your Private Chef backend. (In an HA configuration, this should be the current HA primary.)

.. code-block:: bash

  $ /opt/opscode/bin/orgmapper

You can then query organizations, users, etc through methods like:

.. code-block:: ruby

  orgmapper:0 > pp ORGS.all
  orgmapper:0 > pp ORGS['ORGNAME']
  orgmapper:0 > pp USERS.all
  orgmapper:0 > pp USERS['USER']

Where ``ORGNAME`` is an organization short name, and ``USER`` is a username.


Determine the Users in an Organization
--------------------------------------

.. code-block:: ruby

  orgmapper:0 > OrganizationUser.users_for_organization(ORGS['ORGNAME']).collect do |orguser| 
    Mixlib::Authorization::Models::User.get(orguser).username
  end

Replace ``ORGNAME`` with the organization's short name.

Determine the Organizations for a User
--------------------------------------
.. code-block:: ruby

  orgmapper:0 > OrganizationUser.organizations_for_user(USERS['USERNAME']).collect do |orguser| 
    Mixlib::Authorization::Models::Organization.get(orguser).name
  end

Replace ``USERNAME`` with the username.

Determine a Username based on an Email Address
----------------------------------------------
.. code-block:: ruby

  orgmapper:0 > USERS.select{|u| u.email == 'user@company.com'}

Replace ``user@company.com`` with the email address.

Associate a User to an Organization
-----------------------------------

Make sure the user and the organization both exist, then:

.. code-block:: ruby

  orgmapper:0 > OrgMapper::Associator.associate_user(ORGS['ORGNAME'], USERS['USERNAME'])

Replace ``ORGNAME`` with the organization, and ``USERNAME`` with the username you want to associate.

Add a User to an Organization's Admins Group
--------------------------------------------

.. code-block:: ruby

  orgmapper:0> ORGS['ORGNAME'].add_user_to_group('USERNAME', 'admins')

Replace ``ORGNAME`` with the organization, and ``USERNAME`` with the username.

Remove a User to an Organization's Admins Group
-----------------------------------------------

.. code-block:: ruby

  orgmapper:0> ORGS['ORGNAME'].remove_user_from_group('USERNAME', 'admins')

Delete a User
-------------

.. code-block:: ruby

  orgmapper:0 > USERS['USERNAME']
  orgmapper:0 > USERS['USERNAME'].destroy

Replace ``USERNAME`` with the username you want to delete.

