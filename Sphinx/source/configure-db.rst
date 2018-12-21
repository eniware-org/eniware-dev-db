.. _db-conf:

2. PostgreSQL Database Configuration
=====================================


.. _dp-pass:

2.1. Set up an Authentication Method
-------------------------------------

After successfully :ref:`installing of PostgreSQL 9.6<db-install>` and configuring network access, the next step is to set up a password for the **postgres** user. Run the following commands at a terminal prompt: 

.. code::
  
  sudo passwd postgres
  sudo su postgres

The first command will add а password to the **postgres** OS user. This is necessary to enable the **postgres** OS user to get permission to create an **eniware** user that is needed for database management.

After executing the first command, you will be prompt to setup a password for the **postgres** user.

The password (mentioned here as ``your_password``) for **eniware** user is configured in the ``postgre-create.sql`` script (see the next section).

The second command allows super user permissions to the **postgres** user.


.. _db-create:

2.2. Creating the Database Components
--------------------------------------

Log in to the terminal with a **postgres** user and navigate to the ``/home/eniware/git/org.eniware.central/eniware-db-setup/postgres/`` directory:

.. code::
  
  cd /home/eniware/git/org.eniware.central/eniware-db-setup/postgres/

Within this directory, run the following commands:

.. code::
  
  psql -U postgres -f postgres-create.sql
  psql -U postgres -d eniware -f postgres-init-plv8.sql

The first command will create the **eniware** user (with the configured password ``your_password`` in the script) and database and add the necessary extensions to the **eniware** database.

The second command will execute the ``postgres-init-plv8.sql`` script that will add the necessary components to the database.

Once the script has been executed, you should navigate to the ``/etc/postgresql/9.6/main/`` directory, where the **postgresql.conf** file is located. The **postgresql.conf** configuration must be updated to add the **plv8** global context. To do that open the **postgresql.conf** file using ``sudo nano /etc/postgresql/9.6/main/postgresql.conf``, and enter the following line after the ``*CUTOMIZED OPTIONS /#Add settings for extensions here/`` field:

.. code::

   plv8.start_proc = 'plv8_startup'


.. warning:: After completing the step above you should restart the **postgresql** service.

Exit from the **postgres** OS user with the ``exit`` command.
From the **eniware** OS user navigate to the ``cd /home/eniware/git/org.eniware.central/eniware-db-setup/postgres/``  directory and run the following command:

.. code::
 
 psql -U eniware -d eniware -f postgres-init.sql

Тhe execution of ``postgres-init.sql`` script will create the entire **eniware** database and all necessary components for the **eniware** user.

 
Finally, using the Eclipse you need to copy the **JDBC properties** files from **edge** and **central** projects: from project ``org.eniware.central.common.dao.jdbc/example/configuration`` and project ``org.eniware.edge.dao.jdbc/example/configuration`` to project ``eniware-osgi-target/configurations/services`` and change the file extensions with ``.cfg``.

  * You need to change configurations of file ``org.eniware.central.dao.jdbc.cfg`` as follows:

   .. code::
    
     jdbc.driver = org.postgresql.Driver
     jdbc.url = jdbc:postgresql://localhost:5432/eniware
     jdbc.user = eniware
     jdbc.pass = your_password
  
   This configurations are need to connect to database. 

  * The configuration settings on file ``org.eniware.edge.dao.jdbc.cfg`` should be as follows:

   .. code::
    
     jdbc.driver = org.apache.derby.jdbc.EmbeddedDriver
     jdbc.url = jdbc:derby:eniwareedge;create=true
     jdbc.user = eniwareedge
     jdbc.pass = eniwareedge



.. hint:: For convenient handling of the database and visualization of its components, we recommend the use of the `pgAdmin 3 <https://www.pgadmin.org/docs/pgadmin4/3.x/>`_ PostrgreSQL tool, which we have :ref:`already installed<db-install>`.

Open **pgAdmin 3** as an Ubuntu application or via a terminal, and add new server connection with the following parameters:

* Host ``127.0.0.1`` 
* maintenance database ``postgres``
* user ``eniware``
* password ``your_password`` (i.e. the password configured in the :ref:`postgres-create.sql<dp-pass>` script)

Through the pgAdmin's GUI interface, you can easily monitor the created database and SQL files:

.. figure:: /images/pgadmin3.png
   :alt: pgAdmin 3 user interface


Now your PostrgreSQL database is ready to be used to store and manage the information for registred users who will use the EniwareNetwork platform.
