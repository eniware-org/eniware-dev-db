.. _db-conf:

2. PostgreSQL Database Configuration
=====================================


.. _dp-pass:

2.1. Set up an Authentication Method
-------------------------------------

After successfully :ref:`installing of PostgreSQL 9.6<db-install>` and configuring network access, the next step is to set up a password for the **postgres** user. 

By default, local connections to PostgreSQL use the ``peer`` authentication system. That means that instead of asking you for a password, they check to see if you are currently logged into a system user that matches the user name in Postgres.

You have to change the way you do authentication and instead tell Postgres to use an encrypted password. 

First you need to set a *password* for the **postgres** user. To do this you need to open up **psql** as the user **postgres**.

.. code:: 

  sudo -u postgres psql


You should see output that looks something like this:

.. code::
 
 could not change directory to "/root": Permission denied
 psql (9.6.11)
 Type "help" for help.
 
 postgres=#

 
This permission denied error isn’t important right now.

Now that you are connected to Postgres, you have to change the *password* for the user **postgres**. Be sure to replace the ``xxxxxxx`` below with an actual password:

.. code::
 
 # Replace xxxxxxx with your own password
 ALTER USER postgres WITH ENCRYPTED PASSWORD 'xxxxxxx';


With the command above you are telling Postgres that you want to update the user **postgres** by setting an *encrypted password* of ``xxxxxxx``. If you did this correct, you should get the following output.

.. code::
 
 ALTER ROLE
 postgres=#


Exit **postgres** user quit by typing ``\q`` and then hitting **Enter**.

Now that you have a *password* set for the **postgres** user you have to update **postgres** to use this *password*. To do this you need to edit the ``pg_hba.conf`` file (you can replace **nano** with an editor of your choice):

.. code::
 
 sudo nano /etc/postgresql/9.6/main/pg_hba.conf

Look for an uncommented line (a line that doesn’t start with ``#``) that has the contents shown below. The spacing will be slightly different, but the words should be the same:

.. code::
 
 local   all   postgres   peer

 
The last part of this line is what you have to change. This is what determines how you authenticate the **postgres** user when making a local connection. Instead of ``peer``, which uses your *system user name* to authenticate you, you have to use ``md5`` which uses an *encrypted password* for authentication. Replace the word ``peer`` with ``md5``:

.. code::
 
 local   all   postgres   md5

Save the changes you've made.

Now you need to restart Postgres so the changes take effect:

.. code:: 
 
 sudo service postgresql restart




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

* The first command will create the **eniware** user (with the configured password ``your_password`` in the script) and database and add the necessary extensions to the **eniware** database.

* The second command will execute the ``postgres-init-plv8.sql`` script that will add the necessary components to the database.

Once the script has been executed, you should log out **postgres** system user with the ``exit`` command and log in with an **eniware** system user. 

Navigate to the ``/etc/postgresql/9.6/main/`` directory, where the **postgresql.conf** file is located. The **postgresql.conf** configuration must be updated to add the **plv8** global context. To do that open the **postgresql.conf** file using ``sudo nano /etc/postgresql/9.6/main/postgresql.conf``, and enter the following line after the ``*CUTOMIZED OPTIONS /#Add settings for extensions here/`` field:

.. code::

   plv8.start_proc = 'plv8_startup'


.. warning:: After completing the step above you should restart the **postgresql** service:

  .. code::
    
      sudo service postgresql restart

From the **eniware** system user navigate to the ``/home/eniware/git/org.eniware.central/eniware-db-setup/postgres/`` directory and run the following command:

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
