############
Installation
############

ATTN: Chef 12 is the  new Chef server! Please see the documentation at http://docs.getchef.com/server/. 

ATTN: The documentation for Private Chef has been moved to https://github.com/opscode/chef-docs and is published to http://docs.opscode.com/release/private_chef/index.html. This content is no longer actively maintained.

We’re excited to help you install Private Chef!

Your first step is to make sure you meet the :doc:`universal prerequisites </installation/prereqs>` - regardless of how you install Private Chef, you'll need to make sure these steps are complete.

Next, this guide provides details on 3 different installation options:

-  :doc:`Standalone </installation/standalone>` Private Chef on a single server
-  :doc:`Tiered </installation/tiered>`: Private Chef on multiple servers
-  :doc:`High Availability </installation/ha>`: Private Chef on multiple servers with a highly available data storage layer

Your Opscode representative can provide guidance on which option is
right for your environment. If you are in doubt, follow the
**standalone** installation.

Once you have Private Chef installed, you'll want to move on to :doc:`creating a user </installation/user_creation>` and :doc:`creating an organization </installation/org_creation>`.

.. toctree::
   :maxdepth: 2
  
   installation/prereqs
   installation/standalone
   installation/tiered
   installation/ha
   installation/user_creation
   installation/org_creation
   installation/ad_ldap

