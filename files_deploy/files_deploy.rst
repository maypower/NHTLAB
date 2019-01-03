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
.. figure:: images/image004.png


click + Add New Disk , choose ‘Clone from Image Service’ and image ‘AutoDC’，click Add.
.. figure:: images/image005.png


Click +Add new NIC and choose ‘Rx-Automation-Network’ vlan.0, click Add.
.. figure:: images/image006.png

Now AD VM is created successfully, power on AD VM, then launch console to see domain name and credentials of AD. These informations will be used later.
.. figure:: images/image007.png
.. figure:: images/image008.png

Deploy and setting File Services
++++++++++++
As a pre-work in this lab, we will create vlan41 as secondary network for File Service’s IPs need. Click gear icon on top right. Go to configuration page and navigate to Network Configuration. Click +Create Network.

.. figure:: images/image009.png

Create vlan 41 named Secondary then click save
.. figure:: images/image010.png
.. figure:: images/image011.png


Now we start to create File Server
In Prism > File Server, click + File Server.
Install File Services Software on POCxx

.. figure:: images/image012.png

The Files 3.1.0.1 package has already been uploaded and the Data Services IP has been configured as 10.21.XXX.38. Click Continue.

Fill out the following fields and click Next:
•	Name - POCxx-Files (e.g. POC04-Files)
•	Domain - POCLAB.local
•	File Server Size - 1 TiB
.. figure:: images/image013.png

Select the Rx-Automation-Network-Unmanaged VLAN for the Client Network. 
Fill out the following fields and click Next:
•	Subnet Mask – 255.255.255.0
•	Gateway – 10.21.xx.1
•	IP – from 10.21.xx.100 to 10.21.xx.102 (click save on the right)
•	DNS – 10.21.xx.51
•	NTP – default
.. figure:: images/image014.png
.. figure:: images/image015.png
Select the Secondary - Managed VLAN for the Storage Network. 
Fill out the following fields and click Next:
•	Subnet Mask – 255.255.255.128
•	Gateway – 10.21.xx.129
•	IP – from 10.21.xx.132 to 10.21.xx.135 (click save on the right)
.. figure:: images/image016.png
.. figure:: images/image017.png


Fill out the following fields and click Next:
•	Select Use SMB Protocol
•	Username - Administrator@POClab.local
•	Password - nutanix/4u
•	Select Make this user a File Server admin
•	Select Use NFS Protocol
•	User Management and Authentication - Unmanaged
.. figure:: images/image018.png

Fill out the following fields and click Create:
•	Select Create a Protection Domain and a default schedule (highly recommended)
•	PROTECTION DOMAIN NAME - NTNX-POCxx-Files
.. figure:: images/image019.png

Monitor deployment progress in Prism > Tasks.
.. figure:: images/image020.png


Upon completion, select the AFS server and click Protect.
Observe the default Self Service Restore schedules, this feature controls the snapshot schedule for Windows’ Previous Versions functionality. Supporting Previous Versions allows end users to roll back changes to files without engaging storage or backup administrators. Note these local snapshots do not protect the file server cluster from local failures and that replication of the entire file server cluster can be performed to remote Nutanix clusters. Click Close.





Takeaways
+++++++++

•	Nutanix provides file services suitable for storing user profiles and application data via SMB or NFSv4.
•	AFS is capable of scaling up and out to meet workload requirements.
•	AFS has data protection built-in by leveraging native snapshots and replication. AFS 3.0 will also feature integration with 3rd party backup solutions.


