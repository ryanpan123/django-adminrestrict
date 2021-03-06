# Django Admin Restrict

[![build-status-image]][travis]
[![coverage-status-image]][codecov]
[![pypi-version]][pypi]

**Restrict admin pages using simple IP address rules.**

## Overview

``django-adminrestrict`` secures access to the Django admin pages. It works
by blocking requests for the admin page path unless the requests come from specific IP addresses
that you specify in a model.  


## Requirements

``django-adminrestrict`` requires Django 1.4 or later.  The
application is intended improve the security around the Django admin
login pages.

## Installation

Download and install ``django-adminrestrict`` using **one** of the following methods:

### pip

You can install the latest stable package running this command:

    $ pip install django-adminrestrict

### Setuptools

You can install the latest stable package running:

    $ easy_install django-adminrestrict


## Development

You can contribute to this project forking it from github and sending pull requests.


## Configuration

First of all, you must add this project to your list of ``INSTALLED_APPS`` in
``settings.py``

    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.sites',
        ...
        'adminrestrict',
        ...
    )

Next, install the ``AdminPagesRestrictMiddleware`` middleware:

    MIDDLEWARE_CLASSES = (
        'django.middleware.common.CommonMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'adminrestrict.middleware.AdminPagesRestrictMiddleware',
    )


Create the appropriate tables in your database that are necessary for operation.

For django(<1.7), run ``python manage.py syncdb``.

For django(>=1.7), run ``python manage.py makemigrations adminrestrict; python manage.py migrate``. 

IMPORTANT: When the package is configured in your project, an empty table called `AllowedIP`
will be created in your database. If this table is empty or has one record with
a "\*" the package will not restrict any IPs. If you want to add specific restrictions
please go to the next section.

## Usage

Using ``django-adminrestrict`` is extremely simple.  Once you install the application
and the middleware, all you need to do is update the allowed IP addresses `AllowedIP`
section of the admin pages.

### Adding allowed IP addresses

Login to the admin pages and browse to the Adminrestrict app, and
start creating recorded in the `AllowedIP` table.  Just type in the IP
addresses and save records.

### Adding allowed IP addresses with wildcards

Create a `AllowedIP` entries ending with a "\*" to any IPs that start
with the specified pattern. For example, adding `192.*` would allow
addreses starting matching 192.*.*.* to login to the admin pages.

### Adding * to disable all restrictions

Create a single `AllowedIP` record with "\*" as the IP address, to
temporarily disable restrictions. In this way, you do not have to
modify settings.py and remove the middleware if you need to disable.

Having at least one `AllowedIP` record with * as the IP address
effectively disables all restrictions.

[build-status-image]: https://secure.travis-ci.org/robromano/django-adminrestrict.svg?branch=master
[travis]: https://travis-ci.org/robromano/django-adminrestrict?branch=master
[pypi-version]: https://badge.fury.io/py/django-adminrestrict.svg
[pypi]: https://pypi.org/project/django-adminrestrict/
[coverage-status-image]: https://img.shields.io/codecov/c/github/robromano/django-adminrestrict/master.svg
[codecov]: https://codecov.io/github/robromano/django-adminrestrict?branch=master
