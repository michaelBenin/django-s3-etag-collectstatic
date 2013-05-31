============================
django-s3-etag-collectstatic
============================

Overrides the core collectstatic command to compare the S3 etag and the md5 of
the local file.


Installation
============

This contains very little code, so I wont even bother submitting it to the PyPi. ::

    pip install git+git://github.com/mqsoh/django-s3-etag-collectstatic.git


In your Django settings::

    INSTALLED_APPS = (
        ...
        'django_s3_etag_collectstatic',
        ...
    )

You also need to follow the django-storages_ installation instructions. These
are the pertinent settings for this library.::

    DEFAULT_FILE_STORAGE = 'storages.backends.s3boto.S3BotoStorage'
    STATICFILES_STORAGE = DEFAULT_FILE_STORAGE


Rationale
=========

Heroku does something with the modified times of files during deployments. The
collectstatic management command uses the modified time to determine if a file
needs to be deleted (before being replaced). When you use the s3boto backend
with django-storages_ it often fails to see changes (the output of collecstatic
on Heroku will say 0 files copied).

Other people have had the opposite problem, where the Heroku deployment always
copies everything:

- `A stackoverflow question with someone having the opposite problem from me.`_
- `A Django snippet that prevents pointless copying.`_
- `A library wrapping the aforesaid Django snippet.`_


Solution
========

This app will override the core collecstatic command and prefer the etag and
md5 comparison to anything else.


.. _django-storages: http://django-storages.readthedocs.org/en/latest/
.. _A stackoverflow question with someone having the opposite problem from me.: http://stackoverflow.com/questions/14417322/django-collectstatic-from-heroku-pushes-to-s3-everytime
.. _A Django snippet that prevents pointless copying._: http://djangosnippets.org/snippets/2889/
.. _A library wrapping the aforesaid Django snippet._: https://github.com/AGoodId/django-s3-collectstatic