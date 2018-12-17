.. _db-install:

1. PostgreSQL Database Installation
=====================================

**The first step** is to add the necessary repositorie from which PostgreSQL can be installed:

.. code::
   
   sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -sc)-pgdg main"
   wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

   
**The second step** is to install the PostgreSQL database using the following commands:

.. code::
      
   sudo apt-get update
   sudo apt-get install postgresql-9.6 postgresql-9.6-plv8 postgresql-contrib-9.6 pgadmin3

   
**The third step** is to make the necessary network settings to allow connections via the network. 
To enable other computers to connect to your PostgreSQL server, edit the file ``/etc/postgresql/9.6/main/postgresql.conf`` as it is described below:

  Locate the line ``#listen_addresses = 'localhost'`` and change it to ``listen_addresses = '*'``.

  To allow both IPv4 and IPv6 connections replace ``'localhost'`` with ``'::'``.

You may also edit all other parameters, if you know what you are doing! For details, refer to the configuration file or to the `PostgreSQL documentation <https://www.postgresql.org/docs/9.6/index.html>`_.


Now we can connect to our PostgreSQL server and make the :ref:`necessary configurations<db-conf>`.