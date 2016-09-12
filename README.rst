===========================
Django EnvConf
===========================

.. image:: https://travis-ci.org/achedeuzot/django-envconf.svg?branch=master
    :target: https://travis-ci.org/achedeuzot/django-envconf.svg?branch=master

.. image:: https://coveralls.io/repos/github/achedeuzot/django-envconf/badge.svg?branch=master
    :target: https://coveralls.io/github/achedeuzot/django-envconf?branch=master


Django EnvConf allows you to configure your application using environment variables
as recommended by the 12factor methodology.

Shamelessly forked & updated from https://github.com/joke2k/django-environ

-----------
Quick start
-----------

1. Add "envconf" at the top of your ``settings.py`` file like so:

.. code-block:: python

    import envconf
    env = envconf.Env(  # Set default values and casting
        DEBUG=(bool, False)
    )
    env.read_dot_env()  # read .env file at the project root. You can pass an explicit path to it if needed.


2. Create a ``.env`` file at the root of your project

.. code-block:: shell

    DEBUG=on  # or off / false
    # DJANGO_SETTINGS_MODULE=myapp.settings.dev
    SECRET_KEY=Tom-Marvolo-Riddle
    DATABASE_URL=psql://urser:un-githubbedpassword@127.0.0.1:8458/database
    # DATABASE_URL=sqlite:///my-local-sqlite.db  # sqlite
    CACHE_URL=memcache://127.0.0.1:11211,127.0.0.1:11212,127.0.0.1:11213
    REDIS_URL=rediscache://127.0.0.1:6379:1?client_class=django_redis.client.DefaultClient&password=redis-un-githubbed-password

3. Then fetch the variable you want from the environment in your ``settings.py`` file:


.. code-block:: python

    DEBUG = env('DEBUG')  # Defaults to False
    SECRET_KEY = env('SECRET_KEY')  # Raises ImproperlyConfigured exception if SECRET_KEY is not set
    DATABASES = {
        'default': env.db(),  # Raises ImproperlyConfigured exception if DATABASE_URL not in os.environ
        'extra': env.db('SQLITE_URL', default='sqlite:////tmp/my-tmp-sqlite.db')
    }

------------
Installation
------------

Through Pypi

.. code-block:: shell

    (venv)$ pip install django-envconf

Directly from git

.. code-block:: shell

    (venv)$ pip install git+https://github.com/achedeuzot/django-envconf.git
    # or
    (venv)$ git clone https://github.com/achedeuzot/django-envconf.git && cd django-envconf
    (venv)$ python setup.py install

-----
Usage
-----

Supported Types
===============
- str
- bool
- int
- float
- json
- list as CSV (FOO=a,b,c)
- tuple (FOO=(a,b,c))
- dict (dict (BAR=key=val,foo=bar)  # envconf.Env(BAR=(dict, {}))
- dict (BAR=key=val;foo=1.1;baz=True)  # envconf.Env(BAR=(dict(value=unicode, cast=dict(foo=float,baz=bool)), {}))
- url
- path (environ.Path)
- db_url
  - PostgreSQL: postgres://, pgsql://, psql:// or postgresql://
  - PostGIS: postgis://
  - MySQL: mysql:// or mysql2://
  - MySQL for GeoDjango: mysqlgis://
  - SQLITE: sqlite:// (sqlite://:memory: for in-memory database)
  - SQLITE with SPATIALITE for GeoDjango: spatialite://
  - Oracle: oracle://
  - LDAP: ldap://
- cache_url
  - Dummy: dummycache://
  - Database: dbcache://
  - File: filecache://
  - Memory: locmemcache://
  - Memcached: memcache://
  - Python memory: pymemcache://
  - Redis: rediscache://
- search_url
  - ElasticSearch: elasticsearch://
  - Solr: solr://
  - Whoosh: whoosh://
  - Xapian: xapian://
  - Simple cache: simple://
- email_url
  - Dummy mail: dummymail://
  - SMTP: smtp://
  - SMTP+SSL: smtp+ssl://
  - SMTP+TLS: smtp+tls://
  - Console mail: consolemail://
  - File mail: filemail://
  - LocMem mail: memorymail://


-----
Tests
-----
Clone the repo and run the tests ;)

.. code-block:: shell

    (venv)$ git clone git@github.com/achedeuzot/django-envconf.git
    (venv)$ cd django-envconf
    (venv)$ python setup.py test

-------
License
-------
Django-envconf is licensed under the BSD License - see the LICENSE file for details


-------
Credits
-------

- `django-environ`_

.. _django-environ: https://github.com/joke2k/django-environ


Tested on `Django`_ 1.9.9, 1.10.1 with Python 2.7, 3.4, 3.5

.. _Django: http://www.djangoproject.com/

---------
Changelog
---------

0.1.0 - 12 Sept 2016

- Fork from `django_environ`_ and update of codebase: removal of six dependencly, better oracle support,
