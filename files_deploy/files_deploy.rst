.. _files_deploy:

-------------
Files: Deploy
-------------

Overview
++++++++

.. note::

  Estimated time to complete: 1 HOUR

In this exercise you will use Prism to deploy Acropolis File Services (AFS), a
native, distributed file server solution for Nutanix clusters. You will configure
both SMB and NFS shares, and familiarize yourself with new features of the
AFS offering.

In following steps, you may replace ‘xx’ with your assigned cluster ID


Deploy Acropolis File Services
++++++++++++

Create AutoDC VM for AD/LDAP connectivity on POC##-AHV
Open a terminal and SSH to CVM, type ‘ssh nutanix@10.21.xx.31’ , type ‘ yes’ and enter CVM credentials then access to acli command line
acli image.create AutoDC container=Images image_type=kDiskImage source_url=http://10.21.250.221/images/auto_dc.qcow2


.. figure:: images/image001.png

Now we have an image out there call AutoDC to create a vm for AD, it is a pre-requirement for deploying File Service.
Go to Prism Portal through cluster IP, loggin by providing Prism credentials. 
In Prism > VM, click + Create VM


.. figure:: images/image002.png

Now we are going to create AD VM from AutoDC


.. figure:: images/image003.png

click + Add New Disk , choose ‘Clone from Image Service’ and image ‘AutoDC’，click Add.
.. figure:: images/image004.png


Click +Add new NIC and choose ‘Rx-Automation-Network’ vlan.0, click Add.
.. figure:: images/image005.png

Now AD VM is created successfully, power on AD VM, then launch console to see domain name and credentials of AD. These informations will be used later.
.. figure:: images/image006.png
.. figure:: images/image007.png

Deploy and setting File Services
++++++++++++
As a pre-work in this lab, we will create vlan41 as secondary network for File Service’s IPs need. Click gear icon on top right. Go to configuration page and navigate to Network Configuration. Click +Create Network.

.. figure:: images/image008.png

Create vlan 41 named Secondary then click save
.. figure:: images/image009.png

Now we start to create File Server
In Prism > File Server, click + File Server.
Install File Services Software on POCxx

.. figure:: images/image010.png

Takeaways
+++++++++

  - Nutanix provides file services suitable for storing user profiles and application data via SMB or NFSv4.
  - AFS is capable of scaling up and out to meet workload requirements.
  - AFS has data protection built-in by leveraging native snapshots and replication. AFS 3.0 will also feature integration with 3rd party backup solutions.
