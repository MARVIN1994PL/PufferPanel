Installing Nodes
================
This documentation was written as we performed an installation of this program on Ubuntu 14.04 LTS. Please be aware that the command structure may vary depending on your *nix distro.

Dependencies
------------
* CURL
* OpenSSL
* Git
* ``g++`` and ``make``
* Java (``apt-get install default-jre``)
* NodeJS

Installing NodeJS
^^^^^^^^^^^^^^^^^
Please follow the instructions below for adding NodeJS (thanks to `@gametainers/gsd <https://github.com/gametainers/gsd/>`_ for the documentation).

.. code-block:: sh

	sudo add-apt-repository ppa:chris-lea/node.js
	sudo apt-get update
	sudo apt-get install nodejs

Create a Node
-------------
The first step in this process requires that you first add a node in the panel. Once you have done this you can continue with this process.

Adding a node in the panel (versions ``0.7.5+``) gives you an example configuration that you can then use in ``config.json`` below.

Initial Setup
-------------
Before we can begin installing a node, you must first create some directories that are needed by GSD.

.. code-block:: sh

	[$]~ mkdir -p /mnt/MC/CraftBukkit

You will need to upload a file called ``server.jar`` into the ``/mnt/MC/CraftBukkit`` directory. You can also upload any other miscelaneous files that you want to be added to each server upon creation.

Install Game Server Daemon
--------------------------

.. code-block:: sh

	[$]~ cd /srv && git clone https://github.com/PufferPanel/gsd.git
	[$]~ git checkout tags/<version>
	[$]~ cd /tmp && git clone https://github.com/PufferPanel/cpulimit.git
	[$]~ sudo cp cpulimit/cpulimit /usr/bin

This will download all of the files necessary and place them into the correct directory. To checkout the lastest version of ``GSD`` please `check the releases <https://github.com/PufferPanel/gsd/releases>`_ and input the version that the release is titled. For example, ``0.1.4``.

You will need to make ``config.json`` in ``/srv/gsd``. The contents of this file are shown after you create a node in the panel. Simply copy and paste this into the file and save it.

.. code-block:: sh

	[$]~ cd /srv/gsd && vi config.json
	
.. note::

	When using *vi* you can save and close the file by pressing: *ESC* and then typing *:wq*.

FTPS Configuration
^^^^^^^^^^^^^^^^^^
To ensure a secure connection to your servers GSD uses FTPS by default. In order to allow this to run smoothly you must create some SSL certificates.

.. code-block:: sh

	[$]~ cd /srv/gsd
	[$]~ openssl req -x509 -days 365 -newkey rsa:4096 -keyout ftps.key -out ftps.pem -nodes

Running the command above will ask you a series of questions, you should fill them out as accurately as you can.

Firewall
^^^^^^^^
If you are running a firewall on your server you will need to open up the following ports by default.

.. code-block:: sh

	21 (FTP)
	8003 (GSD Listening Port)
	8031 (GSD Console Port)
	4000 - 5000 (FTP Passive Ports)

Once all of that is complete run the commands below to complete the install of GSD.

.. code-block:: sh

	[$]~ cd /srv/gsd
	[$]~ npm install

In order to start GSD, execute the command below in a new screen

.. code-block:: sh

	[$]~ npm start

Congratulations! Your first node is configured.

Connecting to FTP
-----------------
If you try to login to the FTP server like you would a normal FTP server, you will probably see an error message similar to: ``This server does not permit login over a non-secure connection; connect using FTP-SSL with explicit AUTH TLS``.

In order to connect to the FTP server you will need to connect using ``FTP with TLS/SSL`` (sometimes called ``FTP Explicit SSL/TLS`` or similar) and ``Passive Mode`` enabled.
Do not select ``FTP with Implicit SSL`` as that will not work. On your first connect you will be asked if you trust the server certificate, click Accept.

Please consider writing your own documentation for users to help them out as this can be confusing if they've never done it before.
