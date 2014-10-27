=======
Support
=======

ATTN: Chef 12 is the  new Chef server! Please see the documentation at http://docs.getchef.com/server/. 

ATTN: The documentation for Private Chef has been moved to https://github.com/opscode/chef-docs and is published to http://docs.opscode.com/release/private_chef/index.html. This content is no longer actively maintained.

.. index::
  pair: support; contact information

The Opscode Support Engineering team is an interface between Customer’s
Designated Contact Personnel and Opscode for the support of services provided by
Opscode.  This arrangement provides Customer with access to a direct point of
contact for reporting incidents, receiving updates and escalation.

Support Service Hours
---------------------

:Contact Availability Hours: 7x24x365
:Email Address:	support@opscode.com
:Web Access:	http://www.opscode.com/support
:Service Hours of Operation: - *Severity 1 or 2 Issues*: 7x24x365
                             - *Severity 3 or 4 Issues*: 6am - 6pm PST, Monday - Friday, Excluding holidays


Communicating Incidents
-----------------------

Customer will communicate incidents to Opscode in one of the following manners:

*	Use the web interface http://www.opscode.com/support to directly open a trouble ticket at that site, or

*	Direct an email to Private Chef Support (support@opscode.com), with the sending email address being one of those associated with Customer's organization on Private Chef.

*	Please note that the Opscode Support Phone Number (206-508-4799) is for contact only.

  *	Depending upon the specifics of the issue, it is highly unlikely that real-time analysis and resolution can occur over the phone.

  *	The phone call can initiate support response, but the following will be required from the customer prior to engaging support. This will ensure meaningful action can be effectively and efficiently undertaken for problem resolution.

  *	Customer documentation of the issue, as experienced from their perspective and particular use case.

  *	Documentation of any actions taken immediately prior to the issues occurrence (if any), and

  *	Documentation of any actions taken to troubleshoot or attempted direct resolution that the customer undertook prior to engaging Hosted Chef Support.

* Following using the Support Phone to initiate contact, the required information can be provided by the customer through either of the previously detailed means.

Opscode will validate that the individual communicating the incident is
authorized by Customer to engage support.

*	This avoids an unauthorized engagement of support, and the potential incident overage charges that could occur for Customer.

*	Those who seek support, who aren’t authorized, will be redirected to those who Customer has authorized as determined in Contact Personnel.

For all authorized contacts, Opscode will generate a single response for each
trouble ticket that is received from Customer, within the support hours of
operation as shown above, to confirm receipt of the incident report and
the initiation of appropriate response actions.

Opscode sets the initial classification of the trouble ticket and responds
based upon that classification, unless otherwise agreed between Opscode and
Customer. See Incident Handling and Updates for details on Opscode response.

Information for Incident Reporting
----------------------------------

For each incident, Customer is required to provide Opscode with information
that will facilitate timely problem determination and resolution. Customer
should provide Opscode, at a minimum, the following information for all
reported incidents:

*	OS Platform and version running both Private Chef and Chef-Client.
*	Private Chef version and Chef-Client version.
*	Debug logs from the client. Run :command:`chef-client -l debug`.
*	Debug logs from the server. A tarball or zip file of :file:`/var/log/opscode`.
*	Description of the incident or problem.
*	Customer criticality of the incident or problem.
*	List of those actions taken by Customer to verify the problem and that Customer has attempted to resolve the incident.
*	Validation of Private Chef and Chef-Client interaction.
*	Review and analysis of any system logs.
*	Other comments to provide additional pertinent information as appropriate dependent upon the incident being reported.

Coverage Criteria
-----------------
We will address issues with the Software not performing to specifications, and
provide support and guidance in addressing specific tasks.

*	Problem resolution for the licensed software, diagnosing and addressing discrete problems with specific systems, when it is reasonable to believe that the problem is caused by the Software not performing to specifications.
*	Advisory support and guidance to answer specific questions on use of the Software, such as understanding how to utilize Software functionality, in understanding the purpose and behavior of a configuration option or parameter, or troubleshooting issues in Software behavior variance from specifications.

This excludes the following:

*	Performing tasks for you, rather than advising you in your efforts to perform them.  For example: we can advise you on the means of writing a recipe or a cookbook, or on addressing a specific goal, but we will not write recipes or cookbooks for you.
*	Open-ended requests, such as reviewing a server to find out what is wrong with it or why there are Software issues on it.
*	Architecture and design advice, such as modeling your infrastructure for you and determining a strategy for Private Chef implementation.

Incident Response
-----------------
Opscode will endeavor to respond to and address each reported incident and
request for Private Chef support.  Opscode response will begin upon receipt of
notification, within the Service Hours of Operation listed above.

Opscode, however, makes no commitment on the amount of time that it may take to
resolve an individual incident or issue, as causation factors can vary in
complexity.

Customer will be provided status on incident resolution as described below in
Incident Management.  The timing of customer response status reporting will
occur consistent with the timeframes indicated in Incident Severity. When additional
information has been requested from Customer in order to address the incident,
any failure by Customer to provide requested information will be included in
customer response status reporting consistent with the times indicated in
Incident Severity.

Incident Management
-------------------
Private Chef Support will coordinate incident isolation, testing and repair
work within Opscode and all third party systems that are within Opscode’s Span
of Control based upon the service times specified in the Incident Severity section.  Incident
severity is considered in responding to any Opscode detected or customer
reported issues.

.. index::
  pair: support; incident severity

Incident Severity
-----------------
Opscode initially determines the level of incident severity based on a number
of criteria.  This includes the extent of impact to Customer in use of Private
Chef functionality, the level of repeatability/constancy in issue occurrence,
and the availability of a functional work around.

Customer may seek to increase the initially determined Opscode Severity Level
by following the detail found in Escalation to Opscode.

.. index::
  pair: incident severity; severity 1

Severity 1
~~~~~~~~~~

Private Chef API calls that can cause a Chef-Client run to finish are not available.  Data loss.

:Support Response Targets: - Opscode will begin addressing immediately.
                           - If code change is needed, we will work with you to supply the necessary fixes.
:Customer Response Targets: - Customer first contact must be by phone.
                            - Customer will respond within 30 service minutes
                              following service hour based receipt of report.
                            - Subsequent updates hourly.

.. index::
  pair: incident severity; severity 2

Severity 2
~~~~~~~~~~

Private Chef functionality is available but severely limited.  There is no
available work around.

:Support Response Targets: - Begin addressing as soon as possible.
                           - Prioritized bug and product release cycle as required.
:Customer Response Targets: - Customer first contact must be by phone.
                            - Customer response within 2 service hours
                              following service hour based receipt of report.
                            - Subsequent updates every 2 hours.

.. index::
  pair: incident severity; severity 3

Severity 3
~~~~~~~~~~
Private Chef functionality is usable with minor degradation in service. There
is an available work around for issue.

:Support Response Targets: - Address as soon as possible.
                           - Standard bug and product release cycle as required.
:Customer Response Targets: - Customer response within 1 service day following
                              service hour based receipt of report.
                            - Subsequent updates as warranted, or as agreed.

.. index::
  pair: incident severity; severity 4

Severity 4
~~~~~~~~~~
Issue causes little impact to functionality or Private Chef use. A reasonable circumvention to the problem has been found.

:Support Response Targets: - Address as time permits, best effort.
:Customer Response Targets: - Customer response within 2 service days following
                              service hour based receipt of report.
                            - Subsequent updates as warranted, or as agreed.

.. note::
  The following items will rarely be classified above Severity 3:

  * Questions regarding the use of cookbooks, recipes, attributes, data bags, or any other individual portions of the Chef architecture.
  * Debugging a customer written or modified cookbook or recipe.
  * Deployment questions that are of a “how to” nature

  *Severity 1 and 2 classifications are based in the loss or limitation of
  Private Chef functionality, not in responding to questions on how to utilize
  the product.*


Opscode Troubleshooting and Resolution Access
---------------------------------------------

Customer shall provide Opscode with access to Customer's Network and Systems if
jointly deemed necessary to support resolution of Customer reported issue.

This access must include the following where appropriate:
  1. The ability to connect to the system(s) that are experiencing the issue.
  2. The ability to review full server and client logs.
  3. The ability to review network logs.

This could be particularly critical in responding to Severity 1 or Severity 2
incidents in a timely manner.  Customer should ensure the means of granting
such accesses are in place and ready to be used in advance of their need, as
much as possible.

Time which passes during the granting of necessary accesses, and/or to
establish the means of granting said accesses, will be excluded from any
consideration in the meeting of Customer Response Targets defined in Incident
Severity.

Escalation Procedures
---------------------

Opscode Internal Escalation
~~~~~~~~~~~~~~~~~~~~~~~~~~~
Escalation procedures are in place at Opscode to manage the resolution of
incidents when they occur. All referenced communications and escalations are
available based upon those hours, as listed in Support Service Hours.

The Incident Severity determines the escalation timeline. If the incident
remains open after the time indicated, Opscode escalates stewardship of issue
resolution to the next level, to ensure appropriate resources are aligned and
focused on addressing its resolution.  The following table provides escalation
timelines for Severity 1 and 2 incidents, based on time after the incident was
received during service hours.

================ ============================= ========== ==========
Escalation Level Escalation Contact            Severity 1 Severity 2
================ ============================= ========== ==========
Level 1	         Private Chef Support Engineer 1 hour     2 hours
Level 2	         Management                    2 hours    4 hours
Level 3	         Senior Management             3 hours    8 hours
Level 4          Executive                     4 hours    24 hours
================ ============================= ========== ==========

Severity 3 incidents seldom require escalation but in the event that Customer
believes that Opscode is not addressing the incident in a timely manner, the
parties can mutually agree to elevate the priority of the incident, and treat
it as a Severity 2 incident. Customer can pursue that increase in severity
classification by following the process detailed in Customer Escalation to
Opscode.

Customer Escalation to Opscode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In the event that Customer believes there is cause to increase the Opscode
defined severity level for a specific incident, or if Opscode does not maintain
communication status to Customer consistent with the commitments made in
Incident Severity, Customer can request that the incident be escalated to the
next level.

All escalation requests should be initiated through the Private Chef Support
representative, and not to any other Opscode personnel directly.  Customer
requested escalations will be undertaken by Private Chef Support, and will
occur through the Incident Severity Escalation Table.

Only in the event that Customer has not received a response that the desired
escalation has occurred within thirty (30) minutes of Customer’s request,
should the next Opscode escalation level be contacted directly. Customer should
verify that escalation has not occurred, or that it remains in consideration,
with the Private Chef Support representative prior to utilizing the Private Chef
Support Escalation Contacts in Appendix B for their direct escalation.

Private Chef Support Contact Information
----------------------------------------
Both parties are responsible for ensuring that their contact information is
updated and maintained as current.  Opscode Contact Information is listed
below.

Support Contacts
~~~~~~~~~~~~~~~~

:Contact Availability Hours:	7x24x365
:Email Address: support@opscode.com
:Web Access: http://www.opscode.com/support
:Service Hours of Operation: - Severity 1 or 2 Issues: 7x24x365
                             - Severity 3 or 4 Issues: 6am - 6pm PST, Monday - Friday, Excluding holidays


Additional Opscode Contacts
~~~~~~~~~~~~~~~~~~~~~~~~~~~

:Computing Security: security@opscode.com
:Training and Implementation Services: ts@opscode.com
:Professional Services: ps@opscode.com
:Sales Account Manager: sales@opscode.com
