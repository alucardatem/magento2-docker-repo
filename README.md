
# This repository is for local docker development.

## IF WORKING ON SIMPLE PROJECT FOLLOW STEPS TO GET STARTED
## If working on multiple projects at once, instead of step 7

7. Run

````bash
$> docker-compose -f traefik/docker-compose.yml up --build -d
$> docker-compose -f docker-compose.yml up --build -d
````

the command 
````bash
$> docker-compose -f traefik/docker-compose.yml up --build -d
````
will create a traefik magento and then in the docker-compose.yml change the indicated data in the comments

If starting a new project go into the fpm container and run:

````bash
./bin/magento setup:install  \
--base-url=http://<project_name>.local/ \
--db-host=<db_project_container>_dbSO_1 \
--db-name=<db-name-from-the-env-file> \
--db-user=<db-user-from-the-env-file> \
--db-password=<db-password-from-the-env-file> \
--admin-firstname=<admin-first-name> \
--admin-lastname=<admin-last-name> \
--admin-email=<admin-email> \
--admin-user=<admin-user> \
--admin-password=<admin-password> \
--language=en_GB \
--currency=GBP \
--timezone=Europe/London \
--use-rewrites=1 \
--search-engine=elasticsearch7 \
--elasticsearch-host=reverse-proxy_elasticsearch_1 \
--elasticsearch-port=9200
````

And do not forget to replace what is in the ```<>``` with the correct data

## Steps to get started:

1. Install docker on the machine that you are working on
2. Clone the project
3. Copy the directories and files :
	- .docker
	- mysql-dump  (In this folder please add the mysql database dump file to be imported on the docker container instances )
	- mysql-conf
	- mysql-integration-dump
	- nginx-conf
	- .env
	- docker-compose.yaml
	
on the magento root project folder

4. modify .env file accordingly to the project needs
5. On the dump file for the mysql database run: 
 ````bash
 $> cd mysql-dump
 $> sed -i 's/REPLACE_THIS_WITH_LIVE_BASE_URL/ADD_HERE_THE_LOCAL_URL/g' FILE.sql
 ````
 this will replace the base url to the one desired to have locally


>  Example:
>  ````bash
>  $> cd mysql-dump
>  $> sed -i 's/website.co.uk/website.dev.local/g' livedb.sql
>  ````

7. Run

````bash
$> docker-compose -f docker-compose.yaml up --build -d
````

6. to stop the docker cotainers ( *nix command < linux and osx > )

````bash
$> docker stop $(docker ps -a -q)
````

7. In case there is a need to remove all of the containers:

````bash
$> docker stop $(docker ps -a -q)
$> docker rm $(docker ps -a -q)
$> docker volume rm $(docker volume ls -q)
$> docker system prune -a
````

then run the 

````bash
$> docker-compose -f docker-compose.yaml up --build -d
```` 
to rebuild everything once again


8. Adjust the memory for the system ( in the `nginx-conf/conf.d/default.conf` change the line 144 from 

`fastcgi_param  PHP_VALUE "memory_limit=4G \n max_execution_time=18000";`

to the required memory limit for php depending on your system )

9. the same is available for the `php.ini` from `.docker/PHP/conf/php.ini`

change the line 1
`memory_limit = 4G`

to the same memory limit value as the nginx

10. If on **linux / windows WSL** do remember to update the `sysctl.conf` as root user ( in the `/etc/sysctl.conf `add

````bash
$> sudo nano /etc/sysctl.conf
````
at the bottom of the file add the following lines
````bash
fs.inotify.max_user_watches = 5242880
vm.max_map_count=262144
````
)

11. save and exit then reload sysctl on linux

````bash
$> sudo sysctl -p
````

On **OSX** or on **Windows with HyperV** if the docker container for *mysql* and or *elasticsearch* restarts, do make sure to have the specs set up properly within **docker-desktop**

## Minimum requirements:

 - [ ] 3 CPU's
 - [ ] 6 GB ram
 - [ ] 3 GB swap
 - [ ] and a minimum of a 40gb disk image size
And set the file sharing to the project folder and the folder containing the project folder.
