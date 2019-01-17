.. _db-install:

1. PostgreSQL Database Installation
=====================================

**The first step** is to add the necessary repository  from which PostgreSQL can be installed:

.. code::
   
   sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -sc)-pgdg main"
   wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

.. important:: The first ``sudo`` command above will require to enter a user password. If for some reason you omit this command, the second ``sudo`` command (i.e. ``sudo apt-key add -``) will also require a user password. In this case, if you run it and then when it does not continue, type your password. **It will not show on screen!**

 Under the above circumstances, the second command may not be executed correctly. To verify that the key is correctly added, run the following: ``apt-key list``. This will list the keys added and the key from PostgreSQL should be listed on the bottom like this:
 
 .. figure:: /images/apt-key-list.png
   :alt: apt-key list
   :align: center


**The second step** is to install the PostgreSQL database using the following commands:

.. code::
      
   sudo apt-get update
   sudo apt-get install postgresql-9.6 postgresql-9.6-plv8 postgresql-contrib-9.6 pgadmin3

The above command will install the following:

  * postgresql-9.6 - PostgreSQL 9.6 open source database;
  * postgresql-9.6-plv8 - `the plv8 package <https://packages.debian.org/stretch/postgresql-9.6-plv8>`_ provides a procedural language interface to JavaScript from PostgreSQL.
  * postgresql-contrib-9.6 - `the contrib package <https://www.postgresql.org/docs/9.6/contrib.html>`_ provides several additional supplied modules for the PostgreSQL database.
  * pgadmin3 - `the pgadmin3 package <https://www.pgadmin.org/docs/pgadmin4/3.x/>`_ provides a graphical administration utility.
   
**The third step** is to make the necessary network settings to allow connections via the network. 
To enable other computers to connect to your PostgreSQL server, edit the file  ``/etc/postgresql/9.6/main/postgresql.conf`` as it is described below:

  * Locate the line ``#listen_addresses = 'localhost'`` and change it to ``listen_addresses = '*'``.
  * To allow both IPv4 and IPv6 connections replace ``'localhost'`` with ``'::'``.

You may also edit all other parameters, if you know what you are doing! For details, refer to the configuration file or to the `PostgreSQL official documentation <https://www.postgresql.org/docs/9.6/index.html>`_.


Now we can connect to our PostgreSQL server and make the :ref:`necessary configurations<db-conf>`.




 