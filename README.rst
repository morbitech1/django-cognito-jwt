.. start-no-pypi
.. image:: https://github.com/labd/django-cognito-jwt/workflows/Python%20Tests/badge.svg
    :target: https://github.com/labd/django-cognito-jwt/workflows/Python%20Tests/

.. image:: http://codecov.io/github/LabD/django-cognito-jwt/coverage.svg?branch=master
    :target: http://codecov.io/github/LabD/django-cognito-jwt?branch=master

.. image:: https://img.shields.io/pypi/v/django-cognito-jwt.svg
    :target: https://pypi.python.org/pypi/django-cognito-jwt/

.. image:: https://readthedocs.org/projects/django-cognito-jwt/badge/?version=latest
    :target: https://django-cognito-jwt.readthedocs.io/en/latest/?badge=latest
    :alt: Documentation Status
.. end-no-pypi


Django Cognito JWT
==================

An Authentication backend for Django Rest Framework for AWS Cognito JWT tokens


Installation
============

.. code-block:: shell

   pip install django-cognito-jwt

Usage
=====

Add the following lines to your Django ``settings.py`` file:

.. code-block:: python

    COGNITO_AWS_REGION = '<aws region>' # 'eu-central-1'
    COGNITO_USER_POOL = '<user pool>'   # 'eu-central-1_xYzaq'
    COGNITO_AUDIENCE = '<client id>'

(Optional) If you want to cache the Cognito public keys between requests you can
enable the ``COGNITO_PUBLIC_KEYS_CACHING_ENABLED`` setting (it only works if you
have the Django ``CACHES`` setup to anything other than the dummy backend).

(Optional) If you want to pre-package Cognito public keys as a json file enable ``COGNITO_PUBLIC_KEYS_CACHING_ENABLED`` and
provide the relative file path to the JSON file in ``COGNITO_PUBLIC_KEYS_FILEPATH``.

.. code-block:: python

    COGNITO_PUBLIC_KEYS_CACHING_ENABLED = True

    # when using DJANGO caches
    COGNITO_PUBLIC_KEYS_CACHING_TIMEOUT = 60*60*24  # 24h caching, default is 300s

    # when using pre-packaged file cache
    COGNITO_PUBLIC_KEYS_FILEPATH = "./jwt.json"

Also update the rest framework settings to use the correct authentication backend:

.. code-block:: python

    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': [
            ...
            'django_cognito_jwt.JSONWebTokenAuthentication',
            ...
        ],
        ...
     }



Be sure you are passing the ID Token JWT from Cognito as the authentication header.
Using the Access Token will work for authentication only but we're unable to use the `get_or_create_for_cognito` method with the Access Token.


(Optional) If you want to use a different user model then the default DJANGO_USER_MODEL
you can use the ``COGNITO_USER_MODEL`` setting.

.. code-block:: python

	COGNITO_USER_MODEL = "myproject.AppUser"

Development
=======================

In order to develop for this package, and allow testing, install package in the development mode (via `python setup.py develop` or `pip install --editable`.

Prepare Development environment
........................................

.. code-block:: bash

    pipenv install --dev


Run Tests
.................

.. code-block:: bash

    pipenv shell
    python setup.py develop
    pytest -v
