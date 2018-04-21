# Migrando el Servidor de Tareas de OpenStreetMap Colombia a la versión 3

Desde hace mas de 4 años OpenStreetMap Colombia ha puesto a disposición de todo Latino América y algunas otras partes del mundo un servidor de Tareas basado en el código del Task Manager, desarrollado por HOTOSM, en este servidor hemos coordinado la realización del mapa la atención del riesgo, para talleres, maphatones y diferentes ejercicios que tienen como objetivo contribuir al mapa de OpenStreetMap.org.

Desde el finales del año pasado el HOTOSM lanzó la versión 3 del Task Manager, el equipo de desarrollo decidió reescribir todo el código y cambiar todo el conjunto de tecnologías que hacen funcionar este servicio, por lo tanto la instalación de la nueva versión y la migración del servidor que teníamos en la versión 2 requería de una instalación de un servidor completamente nuevo y ejecutar un script de migración de la base de datos para no perder los usuarios ni el historial de proyectos almacenados en https://tareas.openstreetmap.co

## ¿Como lo hicimos?

1. Seguimos al pie de la letra la siguiente guía: https://github.com/hotosm/tasking-manager/wiki/TM3-Production-with-Nginx-Setup-Guide
1. Y migramos la base de datos con el script que se menciona en el siguiente enlace: https://github.com/hotosm/tasking-manager/wiki/Migrating-from-TM2-to-TM3

## ¿Que problemas tuvimos?

Después de seguir la guía que mencionamos en el punto 1 anterior y tener corriendo el servidor de tareas nos enfrentamos a la migración de la base de datos.
Como nuestro conocimiento de postgreSQL es limitado tuvimos que leer la documentación de postgres y algunos foros de ayuda para entender el script de migración que los desarrolladores de la nueva versión de Tasking Manager hicieron para esta tarea.

Lo que hicimos fue lo siguiente:

- Creamos una base de datos que llamamos hotnew tal como la base de datos del script ingresando a la administración de postgres por la consola.

~~~bash
sudo -u postgres psql
CREATE USER "hottm" PASSWORD 'tupassword';
CREATE DATABASE "hotnew" OWNER "hottm";
\c "hotnew";
CREATE EXTENSION postgis;
\q
~~~

- Al interior de la base de datos creamos dos "schemas" de PostgreSQL para almacenar las tablas de la base de datos nuevo y de la base de datos de la versión 2 del servidor de tareas, el procedimiento fue el siguiente:


~~~bash
sudo -u postgres psql
\c hotnew
CREATE SCHEMA hotnew;
CREATE SCHEMA hotold;
-- Usaremos el schema hotold para almacenar las tablas del servidor en versión 2
-- Luego debemos darle permisos a los usuarios que manejan las bases de datos para que puedan administrar los schemas:
GRANT USAGE ON SCHEMA hotnew,hotold TO postgres ;
GRANT USAGE ON SCHEMA hotnew,hotold TO hottm ;
GRANT ALL ON ALL TABLES IN SCHEMA hotnew,hotold TO postgres,hottm ;
GRANT ALL ON ALL SEQUENCES IN SCHEMA hotnew,hotold TO postgres,hottm ;
GRANT ALL ON ALL FUNCTIONS IN SCHEMA hotnew,hotold TO postgres,hottm ;

~~~

- El siguiente paso consiste en importar los Backups de las bases de datos y poner las tablas de cada una de las bases de datos en el schema correspondiente para poder hacer la migración de los datos
- Lo primero que hicimos fue importar un Backuo del TM3 recién instalado que había exportado para tener una base de datos limpia que pudiera usar para pruebas, el procedimiento fue el siguiente:

~~~bash
sudo -u postgres psql
\c hotnew
\i backupTM3DB.sql
-- Después de importar esta base de datos movemos las tablas al schema hotnew así:
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

- Luego de cambiar el schema de la base de datos nueva procedemos importar el Backup del servidor de tareas viejo y poner sus tables en el schema hotold, el procedimiento fue el siguiente:

~~~bash
sudo -u postgres psql
\c hotnew
\i backupTM2OLDDB.sql
-- Luego ejecutamos el siguiente procedimiento para mover las tablas del TM2 al schema hotold
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
-- Al finalizar ejecutamos el comand \dn+ para ver el estado de cada uno de los schemas
~~~

- Ahora solo tenemos que ejecutar el script de migración y mover los datos de la base de datos del TM2 a la nueva base de datos del TM3, el procedimiento fue el siguiente:

~~~bash
sudo -u postgres psql
\c hotnew
\i migrationscripts.sql
-- En la salida de la ejecución sabremos si todo salio bien.

~~~

- El paso final es limpiar nuestra base de datos, eliminar el schema viejo y pasar las tablas del schema hotnew al schema public para no tener que hacer otros cambios en el codigo del TM3, el procedimiento fue el siguiente:

~~~bash
sudo -u postgres psql
\c hotnew
DROP SALTER TABLE hotnew.users SET SCHEMA public;
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

- Listo! ya tenemos una base de datos con los datos del TM2 en una base de datos preparada para el TM3, el único problema que encontramos después de poner a correr el TM3 con la base de datos nueva es que los roles de los usuarios no los migró correctamente, es decir el script de migración no convierte los valores de los roles del TM2 a los del TM3, este cambio lo hice de forma manual editando la base de datos usando pgadmin4 instalado en mi maquina local, para ingresar de forma remota hice un tunel reverso usando SSH.

~~~bash
ssh -L 5432:localhost:5432 tuusuarii@servido.tu

~~~


- El cambio que hice en la tabla users en la columna role fue el siguiente:

	- Los roles administradores deben llevar un 1
	- Los roles Administradores de proyectos deben llevar un 2
	- Los roles básicos llevan un 0

	Este cambio lo hice de forma manual, se que se puede hacer un script para eso pero fue bastante eficiente usando pgAdmin4.


## Branding

Otra parte del proceso es cambiar la imagen y traducir algunos textos para adaptar el servidor a la imagen de OpenStreetMap Colombia, los cambios se hicieron de forma manual por temas de tiempo, luego lo haremos en un fork del proyecto en github, por ahora lo que cambiamos fue lo siguiente:

1. El archivo index.html en la carpeta /opt/tasking-manager/server/web/static/dist
1. Pusimos el logo de OSM Colombia en el index.html
1. Editamos el archivo app/home/home.html para traducir y eliminar algunas vistas que no nos parece necesario mostrar.
1. Editamos app/learn/learn.html para agregar algunos datos y traducir otros
1. Editamos el archivo app/about/about.html para agregar créditos a la FOSM y el contacto a la comunidad LATAM y .CO
1. Renombramos en el archivo assets/styles/css/taskingmanager.min.css el color #fb6624 por #4158A7 para cambiar un color naranja que sobre sale en el TM del HOT y poner un azul que es parte de la imagen de la UMH y FOSMCo
1. Reescribimos el archivo assets/img/homepage-img.jpg para cambiarle el color naranja por un azul.
1. Resscribimos el archivo assets/img/hot-favicon.ico y assets/img/hot-favicon.ico con el favicon.ico de OSMColombia
1. En diferentes archivos remplazamos el correo del hot por info@openstreetmap.co
