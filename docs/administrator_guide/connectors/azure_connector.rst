Microsoft Azure
===============

Installation
------------

The Azure connector requires some python dependencies to be installed.

::

    $ pip install -y apache-libcloud \
                     scpclient \
                     isodate \
                     xmlwitch

You can install the Azure connector with:

::

    $ yum install slipstream-connector-azure

You will need to restart the SlipStream server to make this connector
visible.

Configuration
-------------

To allow users to take advantage of this connector, you must add one or
more instances of this connector by either:

1. Using the `UI <#with-the-ui>`__.
2. Drop a `configuration file <#with-a-configuration-file>`__ and
   restart the service.

With the UI
-----------

Instanciate one or more instances of the connector
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once logged-in with a privileged user (e.g. *super*), open the
configuration page by clicking on *Configuration -> System* at the top
of the page. Then open the *SlipStream Basics* section and define a new
instance of the connector with the following format:

::

    <connector-instance-name>:<connector-name>

Here is an example:

::

    azure-west-europe:azure

You can also instantiate the connector several times (in compliance with
your license) by comma separating the connector string. Here is an
example:

::

    azure-1:azure, azure-2:azure, ...

Here is a screenshot of the parameter to define:

.. figure:: images/screenshot-cloud-config-param.png
   :alt: SlipStream Configuation - Basics section

   SlipStream Configuation - Basics section

Don't forget to save the configuration!
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that the connector is loaded, you need to configure it.

Configure the connector instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With the connector loaded in SlipStream, a new section in the
configuration page will appear, allowing you to configure how the
connector is to communicate with the IaaS cloud endpoint.

.. figure:: images/screenshot-Azure_ss_system_parameters.png
   :alt: SlipStream Configuation - Azure section

   SlipStream Configuation - Azure section

You can find a detailed description of each parameter as well as an
explaination of how to find the right value of them in the
```Parameters`` <#parameters>`__ paragraph below.

With a Configuration File
-------------------------

Please see :ref:`dg-cfg-files` for details about this method of
configuration.

Here is an example, which will configure the Azure connector to interact
with Azure in the 'West Europe' region:

::

    $ cat /etc/slipstream/connectors/azure-west-europe.conf
    cloud.connector.class = azure-west-europe:azure
    azure-west-europe.quota.vm = 
    azure-west-europe.max.iaas.workers = 20
    azure-west-europe.native-contextualization = linux-only
    azure-west-europe.service.region = West Europe
    azure-west-europe.orchestrator.instance.type = Small
    azure-west-europe.orchestrator.imageid = TODO
    azure-west-europe.orchestrator.ssh.username =
    azure-west-europe.orchestrator.ssh.password = 

You can find a detailed description of each parameter as well as an
explaination of how to find the right value of them in the
```Parameters`` <#parameters>`__ paragraph below.

Parameters
----------

Quota
~~~~~

The quota is SlipStream feature which enable the SlipStream
administrator to set a default quota for all users of a specified
connector. You can also override this value per user in the user
profile. If this feature is disabled in the *SlipStream Advanced*
section of this page, you can leave this field blank.

Image Id of the Orchestrator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The image id of the Orchestrator needs to match a Linux image with
``wget`` and ``python`` installed. An Ubuntu 12.04 or 14.04 will do the
job perfectly.

To find an image id go on the Azure web interface and click on the link
named *Images & Snapshots* and then click on the image you want. The
*ID* value is what you need to paste in.

.. figure:: images/screenshot-Azure_imageId.png
   :alt: Azure web interface - Image details

   Azure web interface - Image details

Region
~~~~~~

From the Azure management console, you can see all of the available
Azure regions. The configuration will use the 'West Europe' region by
default.

Size of the Orchestrator
~~~~~~~~~~~~~~~~~~~~~~~~

The size (instance type) is a name which is linked to a hardware
specification defined by the Cloud. To find the list of all possible
values, please go on the Azure web interface and find a link called
*Size* or *Instance type*. The Orchestrator doesn't need a big amount of
resources so you can choose a small size (like 1 CPU and 512 MB of RAM).

Configure Native Images for This Connector Instance
---------------------------------------------------

Now you need to update SlipStream native images to add the image id and
some parameters for Azure.

This can be done via the UI or via configuration file. Documentation
about how to do it via configuration file can be found here
:ref:`dg-cfg-files-unique-cloud-identifier`.

Please go on a SlipStream base image (e.g. Ubuntu 14.04) and click on
the *Edit* button. Add the image id for Azure in the section named
*Cloud Image Identifiers and Image Hierarchy*.

And then configure the default *instance type* and the default *security
groups* on the tab *Azure* (or the name you gave your Azure connector
earlier) of the section *Cloud Configuration*.

.. figure:: images/screenshot-Azure_image_parameters.png
   :alt: SlipStream Image - edit mode Azure

   SlipStream Image - edit mode Azure

User Credentials
----------------

Now that the connector is configured and the native images updated,
inform your users that they need to configure their credentials for
Azure in their user profile to take advantage of your new connector.

The parameters that will have to be provided are:

-  Subscriber ID
-  Management Certificate (in PEM format)
-  Cloud Service Name
-  Storage Account

These need to be configured with Azure through the web management
interface before the user can use the Azure cloud infrastructure through
SlipStream.
