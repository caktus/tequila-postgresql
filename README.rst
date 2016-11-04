tequila-postgresql
==================

This repository holds an `Ansible <http://www.ansible.com/home>`_ role
that is installable using ``ansible-galaxy``.  This role contains
tasks used to install and set up a Postgres database for a Django
deployment.  It exists primarily to support the `Caktus Django project
template <https://github.com/caktus/django-project-template>`_.


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

- ``project_name`` **required**
- ``env_name`` **required**
- ``db_name`` **default:** ``"{{ project_name }}_{{ env_name }}"``
- ``db_user`` **default:** ``"{{ project_name }}_{{ env_name }}"``
- ``db_password`` **required**
- ``pg_version`` **default:** 9.3
- ``postgresql_config`` **default:** empty dict
- ``app_minions`` **required:** combined list of web servers and celery worker servers

The ``app_minions`` variable can be constructed from Ansible's
inventory information, like ::

    app_minions: "{{ groups['web'] | union(groups['worker']) }}"

in one of your project's variable files.
