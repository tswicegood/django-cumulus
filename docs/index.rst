django-cumulus
==============

The aim of django-cumulus is to provide a set of tools to utilize Rackspace Cloud Files through Django. It currently includes a custom file storage class, CloudFilesStorage.

.. toctree::
   :maxdepth: 2
   :hidden:
   
   changelog

.. comment: split here

Installation
************

To install the latest release (currently 0.2.2) from PyPI using pip::

    pip install django-cumulus

To install the development version using pip::

    pip install -e hg+http://bitbucket.org/richleland/django-cumulus/#egg=django-cumulus

Or you can download the tarball and install::

    wget http://bitbucket.org/richleland/django-cumulus/get/0.2.2.tar.gz
    tar -xzvf django-cumulus-0.2.2.tar.gz
    cd django-cumulus
    python setup.py install

Usage
*****

Add the following to your project's settings.py file::

    CUMULUS_USERNAME = 'YourUsername'
    CUMULUS_API_KEY = 'YourAPIKey'
    CUMULUS_CONTAINER = 'ContainerName'
    DEFAULT_FILE_STORAGE = 'cumulus.storage.CloudFilesStorage'

Alternatively, if you don't want to set the DEFAULT_FILE_STORAGE, you can do the following in your models::

    from cumulus.storage import CloudFilesStorage
    
    cloudfiles_storage = CloudFilesStorage()
    
    class Photo(models.Model):
        image = models.ImageField(storage=cloudfiles_storage, upload_to='photos')
        alt_text = models.CharField(max_length=255)

Then access your files as you normally would through templates::

    <img src="{{ photo.image.url }}" alt="{{ photo.alt_text }}" />

Or through Django's default ImageField or FileField api::

    >>> photo = Photo.objects.get(pk=1)
    >>> photo.image.width
    300
    >>> photo.image.height
    150
    >>> photo.image.url
    http://c0000000.cdn.cloudfiles.rackspacecloud.com/photos/some-image.jpg

Requirements
************

* Django >= 1.1.1
* python-cloudfiles >= 1.7.0

You can install these dependencies yourself, or use the requirements file included in the package::

    pip install -r http://bitbucket.org/richleland/django-cumulus/raw/0.2.2/requirements.txt

Tests
*****

To run the tests, add ``cumulus`` to your ``INSTALLED_APPS`` and run::

    django-admin.py test cumulus

This will upload two very small files to your container and delete them when the tests have finished running.

Issues
******

To report issues, please use the issue tracker at http://bitbucket.org/richleland/django-cumulus/issues/.
