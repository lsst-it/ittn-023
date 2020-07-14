:tocdepth: 3

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. note::

   **This technote is a work in progress.**

Introduction
============

Document Overview
-----------------
The purpose of this document is to describe de deployment of the Cisco ISE cluster for the Rubin Observatory on a high-level, as a way for our collaborators to understand how authentication, authorization, and accounting (AAA) will be performed in the Chilean project networks towards operations, without compromising the security of the cluster configuration itself, based on the requirements set by the project, especially -but not limited to- the Tiger Team in different ICDs and in the documents mentioned in section 1.4.

After acceptance this document may or not be under change control, and regardless of that this is considered a living document that will be updated upon requirement addition or changes, experiences in the deployment, and operations process.

Scope
-----
 * Deliver a high-level overview of the Cisco ISE deployment.
 * Deliver enough details to allow readers to understand how authentication, authorization, and accounting work for the Rubin Observatory networks, without compromising internal security.

Assumptions and Caveats
-----------------------
 * It is assumed that low-level configurations for this cluster are considered sensitive as defined in LPM-122 and therefore not shown publicly here. For management-approved project members, details can be found at: (insert confluence link here).
 * It is assumed all the project concepts derived from other documents such as those mentioned in section 1.4 and/or the associated ICDs, are correct and remain unchanged.
 * It is assumed the reader is familiar with concepts such as authentication, authorization, accounting, mac address bypass, x.509 certificates, 802.1x, and LDAP trees.

Related Documents
-----------------
- `LSE-78 LSST Observatory Network Design <https://ls.st/LSE-78>`_
- `LPM-122 Information Classification Policy <https://ls.st/LPM-122>`_
- `LSE-309 Summit to Base ITC Design <https://ls.st/LSE-309>`_

Technical Solution Overview
============

Context
-------
As of early 2017, the network infrastructure in La Serena was very basic and intended for end-users internet and minor intranet access. Authentication and Authorization (AAA) was done manually by the IT North group, authorizing specific machines on specific network ports for wired access, and using 802.1x enabled Wi-Fi SSIDs synced with the Active Directory's LDAP user accounts; no fixed PSKs were used. In contrast to other transition IT services such as VoIP and network infrastructure, CTIO CISS's did not provide support for AAA services.

As part of technical analysis of the project requirements by several vendors and distributors held in the 2015/2016 timeframe by the Tiger Team, out of which Cisco Systems was the chosen vendor for all the LAN network infrastructure, including AAA, the Cisco Identity Service Engine (ISE) solution was the specific technical solution chosen for the project.

As the Rubin Observatory telescope building and the new base facilities get ready, the following summary requirements must be fulfilled by the AAA infrastructure to be implemented:

1. The solution must be redundant, distributed, and highly available.
2. The solution must allow flexibility to perform authentication for systems using MAB, 802.1x, open-conditional access based on the request source device and/or location at least. Using internal or external authentication sources, mainly LDAP-based.
3. The solution must allow flexibility to perform authorization for systems based on attributes such as internal or external identify source, usernames and/or groups, called-station IDs, device origin/manufacturer/type, at least, and be able to apply differentiated services including -but not limited to- dynamic ACLs, static VLAN assignments via Radius attribute overrides, voice domain identification, web-redirection for self-registration and/or remediation.
4. The solution must allow accounting for radius events and TACACS events, in the case of Cisco network devices. Network devices using TACACS for AAA must be able to access the same levels of AAA as stated in points 2 and 3, plus accounting based on authorizations profiles (read-only, read-write, or custom permissions) which shall record logins and every command applied to the devices associated with the solution.

Cisco ISE (Identity Service Engine)
-----------------------------------
The Cisco Secure Network Server is based on the Cisco UCS C220 Rack Server and is configured specifically to support the Cisco Identity Services Engine (ISE) security application.

Granting and denying network access has evolved beyond simple user name and password verifications. Today, additional attributes related to users and their devices are used as decision criteria in determining authorized network access. Additionally, network service provisioning can be based on data such as the type of device accessing the network, including whether it is a corporate or personal device.

The Cisco Secure Network Server is a scalable solution that helps network administrators meet complex network access control demands by managing the many different operations that can place heavy loads on applications and servers, including:

 * Authorization and authentication requests
 * Queries to identity stores such as Active Directory and LDAP databases
 * Device profiling and posture checking
 * Enforcement actions to remove devices from the network
 * Reporting
 
There are different operating modes for the chosen solution in regards to deployment options, both with pros and cons; this document will expand on this matter in section 3.5.

Please note that the first purchase for Cisco ISE cluster (for the Summit site) was done in mid-2017, with the devices arriving at La Serena around September of the same year, when neither the summit nor the base sites were ready to perform the installation of the appliances. During this time until the procurement of the second part of the cluster (for the Base site) early in 2019 these appliances were installed and used at the Summit site but this specific model, the Cisco ISE 3515 appliance, went out of sale and we had to purchase its replacement model. The cluster was reseated and redeployed when the Base site was ready for operations in January 2020.

The specific device models are the following:

 * **Secure Network Server SNS-3615-K9:** NAC controller, intended for administration and monitoring purposes, based on a Cisco UCS 220 M5 server modified with dual 10G NICs and a specific CPU/RAM/HDD setup to support up to 10000 endpoints.

.. figure:: /_static/3615.jpg
    :name: ISE 3615
    :width: 400 px

 * **Secure Network Server SNS-3515-K9:** NAC controller, intended for policy enforcement, based on a Cisco UCS 220 M3 server modified with dual 10G NICs and a specific CPU/RAM/HDD setup to support up to 10000 endpoints.
 
.. figure:: /_static/3515.JPG
    :name: ISE 3515
    :width: 400 px

Defined AAA Architecture
============

Logical Design
--------------
.. figure:: /_static/ISE-Deployment-ISE-Logical.jpg
    :name: ISE Logical Design
    :width: 1000 px

Physical Design
---------------
.. figure:: /_static/ISE-Deployment-ISE-Physical.jpg
    :name: ISE Logical Design
    :width: 1000 px

Appendix
========
Terminology and acronyms
------------------------

.. include:: acronyms.rst

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
