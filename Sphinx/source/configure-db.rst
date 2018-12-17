.. _db-conf:

2. PostgreSQL Database Configuration
=====================================


.. _dp-pass:

2.1. Set up an Authentication Method
-------------------------------------

After successfully :ref:`installing of PostgreSQL 9.6<db-install>` and configuring network access, the next step is to set up a password for the **postgres** user. Run the following command at a terminal prompt to connect to the default PostgreSQL template database:

.. code::

   sudo -u postgres psql template1   - лог в пострге с темп1 за да може да сменим паролата

The above command connects to PostgreSQL database **template1** as user **postgres**. Once you connect to the PostgreSQL server, you will be at a SQL prompt. You can run the following SQL command at the **psql** prompt to configure the *password* for the user **postgres**:

.. code::
  
  ALTER USER postgres with encrypted password 'your_password'   
  

After configuring the *password*, exit from the SQL promp with the command ``\q``. The next step is to edit the file ``/etc/postgresql/9.6/main/pg_hba.conf`` to use MD5 authentication with the **postgres** user:

.. code::
  
   local all     postgres     md5

   
To initialize the new configuration, you should restart the **postgresql** service using the command ``sudo service postgresql restart``.


.. _db-create:

2.2. Creating the Database Components
--------------------------------------

For convenient handling of the database and visualization of its components, we recommend the use of the `pgAdmin 3 <https://www.pgadmin.org/docs/pgadmin4/3.x/>`_ PostrgreSQL tool.

**pgAdmin 3** is a comprehensive PostgreSQL database design and management system for Unix and Windows systems. It is freely available under the terms of the The PostgreSQL Licence.

.. note:: TODO: How to install **pgAdmin 3**


Open **pgAdmin 3** as an Ubuntu application or via a terminal, and add new server connection with the following parameters:
* Host ``127.0.0.1`` 
* maintenance database ``postgres``
* user ``postgres``
* password ``your_password`` (same as the password in the :ref:`previous section<dp-pass>` is configured)

Through the pgAdmin's GUI interface, you can easily monitor the created database and SQL files.



.. warning:: Which is right?
   
  * **Var. 1:** Now you have to install all the tools you need to open the SQL files from project ``eniware-db-setup/postgres`` and this will generate all the necessary tables, functionts, types, views, triggers etc, used to create the **EniwareDatabase**.
   
  * **Var 2:** After installing all tools (using Eclipse or the cloned repository files **??**) you need to open the SQL files from project (eniware-db-setup/postgres) to create the **EniwareDatabase**.


To add the required components to the database, follow the procedure below:

1. Run the following command at a terminal prompt to connect to the **postgres** database:

  .. code::
     
     sudo -u postgres psql postgres``

2. From the **postgres** user copy and paste the scripts in terminal from the following files:

 * **postgres-create.sql**: When logged as user **postgres** copy and paste the command scripts ``ROLE`` and ``DATABASE`` in terminal from file ``postgres-create.sql``:
 
  .. code::
     
      CREATE ROLE eniware WITH SUPERUSER LOGIN ENCRYPTED PASSWORD 'Eniware8' VALID UNTIL 'infinity';
      CREATE DATABASE eniware WITH ENCODING='UTF8' OWNER=eniware TEMPLATE=template0;
 
  After executing the ``ROLE`` and ``DATABASE`` scripts logout from user **postgres**. Connect to user **eniware** with command ``psql -U eniware`` and create **extensions** from same file:
 
  .. code::
     
     CREATE EXTENSION IF NOT EXISTS citext WITH SCHEMA public;
     CREATE EXTENSION IF NOT EXISTS pgcrypto WITH SCHEMA public;
 
 * **postgres-init-plv8.sql**: To create **functions** copy and paste all scripts form this file and press enter. After that go to the ``/etc/postgresql/9.6/main/postgresql.conf`` file and check in ``CUTOMIZED OPTIONS /#Add settings for extensions here/`` if the ``plv8.start_proc = 'plv8_startup'`` exist. If not add it and save the file.
  
  Restart the **postgresql** ervice again.
 
 * **postgres-init.sql**: In terminal navigate to directory ``/home/eniware/git/org.eniware.central/eniware-db-setup/postgres``. When you are in this directory log in from terminal in PostgreSQL with user **eniware** and type ``\i postgres-init.sql`` and press enter.  **TODO: What is the result of this action?**
 
 * After step 3 you need to copy the **JDBC properties** files from **edge** and **central** projects: from project ``org.eniware.central.common.dao.jdbc/example/configuration`` and project ``org.eniware.edge.dao.jdbc/example/configuration`` to project ``eniware-osgi-target/configurations/services`` and change the file extensions with ``.cfg``.

3. You need to change configurations of file ``org.eniware.central.dao.jdbc.cfg`` as follows:

  .. code::
    
    jdbc.driver = org.postgresql.Driver
    jdbc.url = jdbc:postgresql://localhost:5432/eniware
    jdbc.user = eniware
    jdbc.pass = Eniware8
  
  This configurations is need to connect to database. 

4.The configuration settings on file ``org.eniware.edge.dao.jdbc.cfg`` should be as follows:

  .. code::
    
    jdbc.driver = org.apache.derby.jdbc.EmbeddedDriver
    jdbc.url = jdbc:derby:eniwareedge;create=true
    jdbc.user = eniwareedge
    jdbc.pass = eniwareedge



