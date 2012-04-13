############
Architecture
############

We're happy you're interested in Private Chef!

This section will provide you with an overview of the currently supported architectures for Private Chef, as well as some guidance if you have questions or concerns about the efficacy or performance of these architectures in your environment.  Future changes or options to Private Chef are listed in the :doc:`Scaling and Customization </architecture/snowflakes>` section, but this is not meant to serve as a definitive roadmap for Private Chef; please contact us if you have additional concerns.

The three currently supported architectures of Private Chef are:

-  :doc:`Standalone </architecture/standalone>`: A full stack Private Chef on a single host.
-  :doc:`Tiered </architecture/tiered>`: Private Chef with a horizontally scaled front-end and single back-end.
-  :doc:`High Availability </architecture/high_availability>`: Private Chef with N front-ends and a clustered back-end pair.

Private Chef is flexible, and we want you to be confident in your environment! However, our ability to officially support customizations is extremely constrained.

-  :doc:`Modifications to Private Chef</architecture/modifications>`: Deviating from the supported architectures.

.. toctree::

   architecture/standalone
   architecture/tiered
   architecture/high_availability
   architecture/modifications
