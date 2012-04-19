.. index::
  pair: configuration; private-chef.rb

=======================
Active Directory / LDAP
=======================

Private Chef supports Active Directory and LDAP authentication, allowing
users to log in using their corporate credentials instead of having a separate
Chef password.  To set this up, follow the instructions in the
:doc:`Active Directory / LDAP Installation </installation/ad_ldap>` section of this guide.

Logging in with Active Directory / LDAP
---------------------------------------

When Active Directory / LDAP is enabled, the login page will authenticate
users using their Active Directory credentials.

.. image:: ../images/ad_ldap_login.png
  :alt: Logging in with Active Directory / LDAP

Users Logging In For The First Time
-----------------------------------

Users who have never used Chef before simply log in with their Active Directory
credentials, and Chef account is automatically created for them, populated with name and
email information from Active Directory / LDAP.

*Image: Created User*

Existing Users Logging In After Turning On Active Directory / LDAP
------------------------------------------------------------------

If a user owned a Chef account with a username and password before Active Directory
upgrade, Chef will notice this and ask them to enter the password for their old account.
After which the account will be "linked" and the user will still be a member of all their
organizations and retain all permissions.

*Image: Link Account*



