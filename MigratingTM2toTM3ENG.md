# Migrating the OpenStreetMap Colombia Task Server to version 3

For more than 4 years OpenStreetMap Colombia has made available to all Latin America and some other parts of the world a task server based on the Task Manager code, developed by HOTOSM, on this server we have coordinated the realization of the map the attention of the risk, for workshops, maphatones and different exercises that aim to contribute to the OpenStreetMap.org map.

Since the end of last year HOTOSM launched version 3 of the Task Manager, the development team decided to rewrite all the code and change the whole set of technologies that make this service work, therefore the installation of the new version and migration of the server that we had in version 2 required a completely new server installation and run a database migration script to avoid losing users or the history of projects stored in https://tareas.openstreetmap.co

## How did we do it?

1. We follow the following guide to the letter: https://github.com/hotosm/tasking-manager/wiki/TM3-Production-with-Nginx-Setup-Guide
1. And we migrated the database with the script that is mentioned in the following link: https://github.com/hotosm/tasking-manager/wiki/Migrating-from-TM2-to-TM3

## What problems did we have?

After following the guide that we mentioned in point 1 above and having the task server running we are faced with the migration of the database.
As our postgreSQL knowledge is limited we had to read the postgres documentation and some help forums to understand the migration script that the developers of the new version of Tasking Manager did for this task.

What we did was the following:

- We create a database that we call hotnew such as the database of the script entering the administration of postgres by the console.


~~~bash
sudo -u postgres psql
CREATE USER "hottm" PASSWORD 'tupassword';
CREATE DATABASE "hotnew" OWNER "hottm";
\c "hotnew";
CREATE EXTENSION postgis;
\q
~~~

- Inside the database we created two "schemas" of PostgreSQL to store the tables of the new database and the database of the version 2 of the task server, the procedure was as follows:

~~~bash
sudo -u postgres psql
\c hotnew
CREATE SCHEMA hotnew;
CREATE SCHEMA hotold;
- We will use the hotold schema to store the server tables in version 2
- Then we must give permissions to the users who manage the databases so that they can administer the schemas:
GRANT USAGE ON SCHEMA hotnew,hotold TO postgres ;
GRANT USAGE ON SCHEMA hotnew,hotold TO hottm ;
GRANT ALL ON ALL TABLES IN SCHEMA hotnew,hotold TO postgres,hottm ;
GRANT ALL ON ALL SEQUENCES IN SCHEMA hotnew,hotold TO postgres,hottm ;
GRANT ALL ON ALL FUNCTIONS IN SCHEMA hotnew,hotold TO postgres,hottm ;

~~~

- The next step is to import the backups of the databases and put the tables of each of the databases in the corresponding schema in order to make the migration of the data
- The first thing we did was import a Backuo from the newly installed TM3 that had been exported to have a clean database that could be used for tests, the procedure was as follows:

~~~bash
sudo -u postgres psql
\c hotnew
\i backupTM3DB.sql
- After importing this database we move the tables to the newnew schema like this:
ALTER TABLE users SET SCHEMA hotnew;
ALTER TABLE licenses SET SCHEMA hotnew;
ALTER TABLE messages SET SCHEMA hotnew;
ALTER TABLE priority_areas SET SCHEMA hotnew;
ALTER TABLE project_allowed_users SET SCHEMA hotnew;
ALTER TABLE project_chat SET SCHEMA hotnew;
ALTER TABLE project_info SET SCHEMA hotnew;
ALTER TABLE project_priority_areas SET SCHEMA hotnew;
ALTER TABLE projects SET SCHEMA hotnew;
ALTER TABLE tags SET SCHEMA hotnew;
ALTER TABLE task_history SET SCHEMA hotnew;
ALTER TABLE tasks SET SCHEMA hotnew;
ALTER TABLE users_licenses SET SCHEMA hotnew;
ALTER TABLE spatial_ref_sys SET SCHEMA hotnew;
ALTER TABLE alembic_version SET SCHEMA hotnew;
~~~

- After changing the schema of the new database we proceed to import the backup of the old task server and put its tables in the schema hotold, the procedure was as follows:

~~~bash
sudo -u postgres psql
\c hotnew
\i backupTM2OLDDB.sql
- Then we execute the following procedure to move the tables of the TM2 to the schema hotold
LTER TABLE alembic_version SET SCHEMA hotold;
ALTER TABLE areas SET SCHEMA hotold;          
ALTER TABLE label SET SCHEMA hotold;
ALTER TABLE label_translation SET SCHEMA hotold;
ALTER TABLE licenses SET SCHEMA hotold;
ALTER TABLE message SET SCHEMA hotold;       
ALTER TABLE priority_area SET SCHEMA hotold;
ALTER TABLE project SET SCHEMA hotold;  
ALTER TABLE project_allowed_users SET SCHEMA hotold;
ALTER TABLE project_labels SET SCHEMA hotold;
ALTER TABLE project_priority_areas SET SCHEMA hotold;
ALTER TABLE project_translation SET SCHEMA hotold;
ALTER TABLE task SET SCHEMA hotold;
ALTER TABLE task_comment SET SCHEMA hotold;
ALTER TABLE task_lock SET SCHEMA hotold;   
ALTER TABLE task_state SET SCHEMA hotold;
ALTER TABLE users SET SCHEMA hotold;     
ALTER TABLE users_licenses SET SCHEMA hotold;
- At the end we execute the command \ dn + to see the status of each of the schemas
~~~

- Now we just have to execute the migration script and move the data from the TM2 database to the new TM3 database, the procedure was as follows:

~~~bash
sudo -u postgres psql
\c hotnew
\i migrationscripts.sql
- At the start of the execution we will know if everything went well.

~~~

- The final step is to clean up our database, delete the old schema and pass the hotnew schema tables to the public schema so that we do not have to make other changes in the TM3 code, the procedure was as follows:

~~~bash
sudo -u postgres psql
\c hotnew
DROP SCHEMA hotold CASCADE ;
ALTER TABLE hotnew.users SET SCHEMA public;
ALTER TABLE hotnew.licenses SET SCHEMA public;
ALTER TABLE hotnew.messages SET SCHEMA public;
ALTER TABLE hotnew.priority_areas SET SCHEMA public;
ALTER TABLE hotnew.project_allowed_users SET SCHEMA public;
ALTER TABLE hotnew.project_chat SET SCHEMA public;
ALTER TABLE hotnew.project_info SET SCHEMA public;
ALTER TABLE hotnew.project_priority_areas SET SCHEMA public;
ALTER TABLE hotnew.projects SET SCHEMA public;
ALTER TABLE hotnew.tags SET SCHEMA public;
ALTER TABLE hotnew.task_history SET SCHEMA public;
ALTER TABLE hotnew.tasks SET SCHEMA public;
ALTER TABLE hotnew.users_licenses SET SCHEMA public;
ALTER TABLE hotnew.spatial_ref_sys SET SCHEMA public;
ALTER TABLE hotnew.alembic_version SET SCHEMA public;CHEMA hotold CASCADE;

~~~

- Ready! we already have a database with TM2 data in a database prepared for TM3, the only problem we found after running the TM3 with the new database is that user roles did not migrate correctly , that is, the migration script does not convert the values of TM2 roles to those of TM3, this change was made manually by editing the database using pgadmin4 installed on my local machine, to enter remotely I made a reverse tunnel using SSH.

~~~bash
ssh -L 5432:localhost:5432 tuusuarii@servido.tu

~~~

- The change I made in the users table in the role column was the following:

	- Admin roles must carry a 1
	- The roles Project managers must carry a 2
	- Basic roles carry a 0

I made this change manually, I know that you can do a script for that but it was quite efficient using pgAdmin4.

## Branding

Another part of the process is to change the image and translate some texts to adapt the server to the image of OpenStreetMap Colombia, the changes were made manually by time themes, then we will do it in a fork of the project in github, for now what We changed was the following:

1. The index.html file in the folder / opt / tasking-manager / server / web / static / dist
1. We put the OSM Colombia logo in the index.html
1. We edit the file app / home / home.html to translate and delete some views that we do not need to show.
1. We edit app / learn / learn.html to add some data and translate others
1. We edit the app / about / about.html file to add credits to the FOSM and the contact to the LATAM and .CO community
1. Rename in the file assets / styles / css / taskingmanager.min.css the color # fb6624 for # 4158A7 to change an orange color that comes out in the TM of the HOT and put a blue that is part of the image of the UMH and FOSMCo
1. We rewrite the assets / img / homepage-img.jpg file to change the color orange to blue.
1. We rewrite the file assets / img / hot-favicon.ico and assets / img / hot-favicon.ico with the favicon.ico of OSMColombia
1. In different files we replace the hot mail by info@openstreetmap.co
