Tequila-postgresql
==================

Changes

v 1.0.0? on ?
-------------

*BACKWARD INCOMPATIBLE CHANGE*

* Focus this role on creating and managing Postgresql *servers* only.

  * Remove database creation function.
  * Rename db_user, db_password to pg_user, pg_password
  * Add pg_user_attrs to control permissions of the user created

Migration note: when upgrading to this version of tequila-postgresql,
if you want your project deployed the same way as before the upgrade,
also upgrade tequila-django and configure ``db_admin_user`` and
``db_admin_password`` so it can do the user and database creation
itself.

v0.8.1 on April 8, 2019
-----------------------

* Add Postgres repo and its key before trying to install Postgres

v 0.8.0 on May 3, 2017
----------------------

* Initial release
