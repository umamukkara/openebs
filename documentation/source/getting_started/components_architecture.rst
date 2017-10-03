.. _Components and Architecture:

Components and Architecture
===========================
This section includes OpenEBS components and architecture.

Components
----------

OpenEBS platform contains the following main components

  * Maya - The helper storage orchestration engine that aids the kubernetes orchestration of storage volumes
  * Jiva - A docker image that is used to spin the storage volume containers on kubernetes minions

Maya
----
OpenEBS orchestration does not pre-empt or overwrite the kubernetes orchestration system, rather it fills the storage orchestration gaps left behind by kubernetes. For example, in OpenEBS, storage volume provisioning workflow is handled by kubernetes. Just like other kubernetes storage incubators, OpenEBS provides a new storage incubator called "OpenEBS". This incubator will have a storage class called "OpenEBS-storageclass". Internally, the openebs-storageclass will interact with maya to take decisions on which node a given volume has to be provisioned, when it has to be augmented automatically in capacity etc. Maya will also help in the data protection operations such as taking snapshots, restoring from snapshots etc. 

Jiva
----
Jiva is the docker container image for storage volume containers. In OpenEBS, the storage volumes are containerized. Each volume will have atleast one storage controller and a storage replica, each of which will be a Jiva container. The functionality embedded into the Jiva image includes the following:
 - iSCSI target
 - Block replication controller (if the container is a controller)
 - Block storage handler (if the container is a replica)


Architecture
-------------

Maya is the orchestration engine that schedules the VSMs among OpenEBS hosts as needed. Maya driver plays an important role in achieving the smooth flow of provisioning of VSMs and attaining the application consistent snapshots. The data is kept in more than one copy among the OpenEBS hosts through a backend network replication, thus achieving the necessary redundancy. VSMs expose the iSCSI interface currently.

The backend data store for Jiva containers come either through locally managed disks or through remotely managed network disks. The intelligent caching along with the lazy read indexing capability makes it possible to treat remote S3 storage also as the backing data store.

.. image:: ../_static/architecture.png
