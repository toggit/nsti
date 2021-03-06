Installation
============

NSTI performs well with and is designed to run on CentOS, RHEL and most Linux distributions that utilize the yum package manager.  It is important to note that NSTI was designed to be installed on a clean server with no other software installed.

.. danger::

   If you install NSTI on a server that has Nagios XI or Nagios Core installed on it there will be further configuration involved with snmp and snmptt based on what was used to configure the Nagios trap setup.

.. note::

   NSTI is designed to work under any environment, but the installer script
   will only work with the yum package manager until cross platform support is added in
   another version.

Installing NSTI Using Tarball
-----------------------------
Here is the Official Nagios NSTI Tarball page to track releases: 

   http://assets.nagios.com/downloads/nsti/tarballs/

If you want to install NSTI using a tarball use the URL below

   http://assets.nagios.com/downloads/nsti/tarballs/latest.php

From the command line use wget to install

.. code-block:: bash

    cd /tmp
    wget http://assets.nagios.com/downloads/nsti/tarballs/latest.php
    tar zxf nsti-3.0.2.tar.gz
    cd nsti-3.0.2/
    sh install.sh

Now NSTI should be installed, to verify navigate to this address::

    http://<NSTI Server IP>/nsti

.. note ::

   If the previous install options happen to fail--the best way to
   troubleshoot is to consult the install log that is generated in the install
   directory.  This will show what the install did and where it failed.


NSTI Dependency Notes
---------------------
If you wish to install NSTI manually the following is a list of components
that you will need to run NSTI:

* mysql 
* mysql-devel
* mysql-server
* httpd
* gcc
* wget
* make
* tar
* mod_wsgi
* snmptt
* net-snmp
* python

In addition to the above, there are python modules that are required.  These
modules are also included in nsti/install/requirements.txt.  These are easily
managed if you install using pip:

.. code-block:: bash

    curl https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py | python
  

Possible NSTI Install Gotcha's
-------------------------------

If any of the packages failed to install you will see error outputs such as the following possible errors:

.. warning::

    | 'Cannot continue install until all of these prereqs are met.'
    |
    | 'mod_wsgi is not installed in Apache. Please use your package manager to install it.'
    |
    | 'No user `nagios`. Add a user for nagios and make sure it is in the nagcmd group.'
    | 'Apache must also be in nagcmd.'


Flask and Storm
****************

.. note::

    Both of these are inside the requirements.txt that the install script refers to so they should both be installed at this point, but just in case we will go over them and how to install them if, for some reason, they are missing.


NSTI uses Flask as a lightweight web application framework and uses Werkzeug and Jinja2 for templating engines.  Flask is a great microframework that allows extentions to be added granting it access to form validation and database abstraction.  You will need Flask so install it using pip or easy_install.


Storm is a Python programming library for Object-Relational Mapping (ORM) between one or more SQL databases and Python objects.  This allows NSTI to make queries and demands of the database to be able to populate, collect and filter the traps in the database, but also how the traps are displayed in the UI.



MySQL Configuration
*******************

.. danger::

   If the installer ran successfully the first time the following section does
   not apply.  It is only relevent if any errors occurred or if you are
   installing NSTI manually.

To run NSTI correctly it is important that you have your mysql set up correctly so NSTI can read and write data from the database.  The most common problem with NSTI not communicating properly with your mysql database is the root user password settings.  It is recommended that you set the password if the install script failed to do so.

The nsti.py file is located here::

    nsti/etc/nsti.py


This is where you will set your database type, host, name, user and password.  To test if you successfully configured your database use the runserver.py in the nsti directory.  It will output any connection errors and you will be able to adjust settings accordingly.

Another point to be made is that you will need to verify that the database NSTI is writing to was created.  If it wasn't you will receive similar errors to the following:

.. warning::

    ERROR 1049 (42000): Unknown database 'snmptt'

    Unable to add database schema. Does the snmptt exist?


This means that the installer did not create the database so you will need to run the installer again or manually add the database that will be written to, create the root user and use the password 'password' to make sure it is working.  The default database is Snmptt.

.. note::

    After you verify everything is working with the database it is recommended that you change your password and update it in the configuration files.



Installing NSTI Using Git Clone (Developers)
--------------------------------------------
This section is for developers who would like to clone the NSTI repository to get a look at the front and back end firsthand.  First you will clone the Git repo to retrieve NSTI.

To clone a repository with git run the following command in the directory you want nsti to be copied to:

.. code-block:: bash

    cd /tmp
    git clone https://github.com/NagiosEnterprises/nsti.git
    cd nsti/
    sh install.sh

Now NSTI should be installed, to verify navigate to this address::

    http://<NSTI Server IP>/nsti


While developing use the runserver.py to launch the server and get apache request output from the commandline and access NSTI as shown below (Note: runserver.py is used to launch the nsti program for debugging and development- if you want to access the nsti page as normal just use the <IP>/nsti URL):

  cd /tmp/nsti/
  
  python runserver.py

  http://<NSTI Server IP>:8080/nsti
