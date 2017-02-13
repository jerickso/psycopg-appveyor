Building and testing of Windows psycopg2 wheel packages
=======================================================

.. image:: https://ci.appveyor.com/api/projects/status/nsd8l9debbotxb60/branch/master?svg=true
       :target: https://ci.appveyor.com/project/psycopg/psycopg2-appveyor/branch/master
       :alt: Build Status

This project is used to create and test Windows binary packages of psycopg2_
using the AppVeyor_ continous integration service.

.. _psycopg2: http://initd.org/psycopg/
.. _AppVeyor: https://www.appveyor.com/


Building packages
=================
This builds whl/egg/exe packages, 32bit and 64bit for Python 2.7/3.3/3.4/3.5/3.6.
If a new git tag is created, it uploads the whl files to TestPyPi.

When a new psycopg2 release is ready, upload the submodule to the release
tag and push, duplicate the tag in this repository and push::
    cd psycopg2
    git checkout 2_7_BETA_1
    cd ..
    git add psycopg2
    git commit -m "Building packages for psycopg2 2.7 beta 1"
    git push
    git tag 2_7_BETA_1
    git push --tags

Linked Libraries
================

This project builds libraries from the following projects and links to them:
  - OpenSSL_: 1.0.2k
  - PostgreSQL_: 9.6.2

If newer versions of these libraries are available and want to be used, the
following files need to be updated to link against the newer versions:
  - README.rst - This readme informing what version is used
  - cache.rebuild - A change in this file forces a rebuild of the AppVeyor Cache
  - appveyor.yml - Update download links and extracted directories

.. _PostgreSQL: https://www.postgresql.org/
.. _OpenSSL: https://www.openssl.org/


AppVeyor Cache
=================

AppVeyor has a persistant cache that survives between builds (provided the build
is successful, an unsuccessful build does not update the cache).  This cache is
used for the OpenSSL and PostgreSQL link libraries.  To invalidate the cache,
the file 'cache.rebuild' needs to be updated.

Using the cache speeds up the build process from ~38 minutes to ~18 minutes.


Note on OpenSSL >= 1.1.0
========================

On Windows, OpenSSL >= 1.1.0 renamed the link libraries from libeay32/ssleay32
to libcrypto/libssl.  Currently, both PostgreSQL's libpq and psycopg2 expect the
pre-1.1.0 library names.  If libpq ever supports the new names, we will update
psycopg2 to use the new names, too.

