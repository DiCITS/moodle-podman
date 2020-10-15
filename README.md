# Moodle-Podman

*How to deploy Moodle platform on podman - The easy way*

This is the tool used for online courses within the laboratory and other workplaces. For the deployment we have used podman instead of docker within a Fedora 31 environment.

The steps to get the system up and running are as follows: 

1. Run a pod with postgresql:

``
podman run --name postgres_moodle -e POSTGRES_USER=moodle -e POSTGRES_PASSWORD=yourpass -e POSTGRES_DB=moodle   -p 8432:5432 -d postgres:11
``

2. Create a folder to store Moodle source code, for instace (if not created):

``mkdir /var/www/``

2.1. Create a folder to store Moodle data (mandatory if you want to share content out of podman):

``mkdir /var/www/moodledata/``

2.2. Change permissions of ``/moodledata`` folder:

``
chmod 777 /var/www/moodledata/
``

3. Clone Moodle source code in this folder:

``
cd /var/www/
git clone https://github.com/moodle/moodle.git
``
*Note: Clone will create a folder: /var/www/moodle/ with the source code inside*

4. Run a pod with PHP-APACHE-etc.

```
podman run --name imuds-moodle -e MOODLE_DOCKER_DBTYPE=pgsql -e MOODLE_DOCKER_DBNAME=moodle -e MOODLE_DOCKER_DBUSER=moodle -e MOODLE_DOCKER_DBPASS=yourpass -e MOODLE_DOCKER_BROWSER=firefox  -v /var/www/moodle:/var/www/html:z -v /var/www/html/moodledata:/var/www/moodledata:z  -p 8431:80 -d moodlehq/moodle-php-apache:7.2
```



