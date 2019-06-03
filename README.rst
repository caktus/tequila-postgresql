tequila-postgresql
==================

This repository holds an `Ansible <http://www.ansible.com/home>`_ role
that is installable using ``ansible-galaxy``.  This role contains
tasks used to install and set up a Postgres server for a Django
deployment.

This Ansible role is only needed if you are hosting a Postgres
server yourself. In other words, if you need to install and
run the Postgres server on a system or systems, and configure
a user with sufficient privileges to create databases and manage
other users.

If you are using RDS, for example, you do not need this role.
`tequila-django <https://github.com/caktus/tequila-django>`_
can handle creating a project user and database
on RDS, or other existing Postgres server, so long as you can give
it access to a user with the necessary privileges on that server.

tequila-django can also handle the case where you set up a
project user and database yourself, and just give tequila-django
access to that (so tequila-django doesn't need access to a user
with more privileges, and you can omit those credentials entirely
from your deploy).

This role exists primarily to support the `Caktus Django project
template <https://github.com/caktus/django-project-template>`_.

More complete documentation can be found in `caktus/tequila
<https://github.com/caktus/tequila>`_.


License
-------

This Ansible role is released under the BSD License.  See the `LICENSE
<https://github.com/caktus/tequila-postgresql/blob/master/LICENSE>`_
file for more details.


Contributing
------------

If you think you've found a bug or are interested in contributing to
this project check out `tequila-postgresql on Github
<https://github.com/caktus/tequila-postgresql>`_.

Development sponsored by `Caktus Consulting Group, LLC
<http://www.caktusgroup.com/services>`_.


Installation
------------

Create an ``ansible.cfg`` file in your project directory to tell
Ansible where to install your roles (optionally, set the
``ANSIBLE_ROLES_PATH`` environment variable to do the same thing, or
allow the roles to be installed into ``/etc/ansible/roles``) ::

    [defaults]
    roles_path = deployment/roles/

Create a ``requirements.yml`` file in your project's deployment
directory ::

    ---
    # file: deployment/requirements.yml
    - src: https://github.com/caktus/tequila-postgresql
      version: 0.1.0

Run ``ansible-galaxy`` with your requirements file ::

    $ ansible-galaxy install -r deployment/requirements.yml

or, alternatively, run it directly against the url ::

    $ ansible-galaxy install git+https://github.com/caktus/tequila-postgresql

The project then should have access to the ``tequila-postgresql`` role in
its playbooks.


Variables
---------

The following variables are made use of by the ``tequila-postgresql``
role:

- ``pg_version`` **default:** 9.3
- ``postgresql_config`` **default:** empty dict. Items in this dict configure
  specific things in the postgresql.conf. For now, you'll have to look through
  ``templates/postgresql.conf`` to see which keys are used for what.
- ``db_role`` **default:** master. Can be set to ``slave`` if this server should
  replicate from another. Again, see ``templates/postgresql.conf`` for details on
  what this affects.
- ``app_minions`` **required:** combined list of web servers and celery worker servers
  that will need to access this server.  See ``templates/pg_hba.conf`` for how this
  works, and below for a hint on setting this automatically from your inventory.
- ``pg_user`` **required:** username of a Postgres user to create that will have
  enough privileges to manage users and databases
- ``pg_password`` **required:** the password for ``pg_user``
- ``pg_user_attrs`` **default:** ``CREATEDB,CREATEROLE,LOGIN,NOSUPERUSER`` controls
  the permissions of the pg_user.
  See ``role_attr_flags`` in the
  `Ansible postgresql user module <https://docs.ansible.com/ansible/latest/modules/postgresql_user_module.html>`_
  for all the valid values, and
  `Postgresql CREATE ROLE <https://www.postgresql.org/docs/11/sql-createrole.html>`_
  for what they mean.

The ``app_minions`` variable can be constructed from Ansible's
inventory information, like ::

    app_minions: "{{ groups['web'] | union(groups['worker']) }}"

in one of your project's variable files.
