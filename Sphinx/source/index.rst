.. Database Setup Guide for EniWARE Platform documentation master file, created by
   sphinx-quickstart on Mon Dec 17 11:30:06 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Database Setup Guide for EniWARE Platform's documentation!
=====================================================================


.. image:: /images/0.png
   :alt: EniWARE
   :scale: 90%
   :align: right
   :target: http://eniware.org


:Version:
        |release|

:Language:
         en

:Description:
        Database Setup Guide for EniWARE Platform

:Author:
         EniWARE

:Rendered:
         |today|
      

Introduction
=============

In the `Eclipse Setup Guide for EniwareNetwork <https://eniware-org.github.io/eniware-dev-docs/>`_ documentation we introduced you the EniwareNetwork open source software platform and how to setup a development environment for contributing to or modifying EniwareNetwork code using the popular Eclipse IDE. 

The EniwareNetwork Platform requires the use of databases. This documentation will describe the process of installation and configuration of database to be used in the various projects related to the EniwareNetwork Platform.

A `PostgreSQL v.9.6 <https://www.postgresql.org/>`_ will be used as a software to manage the database.

**Prerequisites**

 1. Linux based operation system with GUI.
 2. SSH client (`PuTTY <https://www.putty.org/>`_ for example)
 3. Installed and `properly configured Eclipse <https://eniware-org.github.io/eniware-dev-docs/eclipse-set-guide.html#eclipse-setup-guide>`_ and/or Git. 


	  
**Contents:**


.. toctree::
   :maxdepth: 4
    
   install-db
   configure-db


	

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
