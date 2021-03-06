Installation
============
(on Ubuntu 14.04 - some packages are named differrently in other versions)
First you need a few essentials:
```
$ sudo apt-get install python-dev build-essential
```

Now you need pip and virtualenv:

```
$ sudo apt-get install python-pip
$ sudo pip install virtualenv
```
#### Install pip libraries (including django)
Needed for pycurl:

```$ sudo apt-get install libcurl4-openssl-dev```

Needed for psycog2:

```$ sudo apt-get install libpq-dev```

Everything else:

```$ . activate```

If you hit an error like this:

```Could not find a version that satisfies the requirement djangorestframework-gis>=0.8.1 (from -r libraries.txt (line 6)) (from versions: 0.8, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8)```

Enter the following command to download an alpha of this version:

```$ pip install https://github.com/djangonauts/django-rest-framework-gis/tarball/master```


#### PostgreSQL and PostGIS
Install all postgres things needed:

```
$ sudo apt-get install postgresql-9.3 postgresql-9.3-postgis-2.1 pgadmin3 postgresql-contrib
$ sudo su - postgres
```

Create your database:
```
$ createdb django_test
$ exit
```


GeoDjango (ref: https://docs.djangoproject.com/en/dev/ref/contrib/gis/install/geolibs/)

Install GEOS

```
$ wget http://download.osgeo.org/geos/geos-3.4.2.tar.bz2
$ tar xjf geos-3.4.2.tar.bz2
$ cd geos-3.4.2
$ ./configure
$ make
$ sudo make install
$ cd ..
```

Install PROJ.4

```
$ wget http://download.osgeo.org/proj/proj-4.8.0.tar.gz
$ wget http://download.osgeo.org/proj/proj-datumgrid-1.5.tar.gz
$ tar xzf proj-4.8.0.tar.gz
$ cd proj-4.8.0/nad
$ tar xzf ../../proj-datumgrid-1.5.tar.gz
$ cd ..
$ ./configure
$ make
$ sudo make install
$ cd ..
```

Set up PostGIS

```
$ sudo su - postgres
$ psql django_test
```

```
> CREATE USER django_user WITH PASSWORD 'dj4ng0_t3st';
> CREATE EXTENSION adminpack;
> CREATE EXTENSION postgis;
> CREATE EXTENSION postgis_topology;
> \q
```

```
$ exit
```

#### Scheduled jobs (optional)

(use sudo to add to root, without to add to user's crontab):

```(sudo) crontab -e```

then select an editor and add the following line:

```0 3 * * * /(path)/manage.py runjobs daily```

(runs daily at 3 AM)
