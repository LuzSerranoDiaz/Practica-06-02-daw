# Practica-06-02-daw

## docker-compose

```yml
version: '3.4'

services:
  mysql:
    image: mysql:8.0
    environment: 
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes: 
      - mysql_data:/var/lib/mysql
    networks: 
      - backend-network
    restart: always
```
#### Servicio mysql
* Image: mysql:8.0, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * MYSQL_ROOT_PASSWORD indica la contraseña del usuario root.
  * MYSQL_DATABASE indica el nombre de la base de datos.
  * MYSQL_USER indica el usuario.
  * MYSQL_PASSWORD indica la contraseña del usuario.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
```yml
  phpmyadmin:
    image: phpmyadmin:5.2
    ports:
      - 8080:80
    environment:
      - PMA_HOST=mysql
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
```
* Image: phpmyadmin:8.0, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * PMA_HOST indica el host donde se encuentra mysql, al poner `mysql` indicamos que ese contenedor es el host.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
* depends_on: indica de que servicios depende.
```yml
  wordpress:
    image: bitnami/wordpress:6.4.3
    environment:
      - WORDPRESS_DATABASE_NAME=${MYSQL_DATABASE}
      - WORDPRESS_DATABASE_USER=${MYSQL_USER}
      - WORDPRESS_DATABASE_PASSWORD=${MYSQL_PASSWORD}
      - WORDPRESS_DATABASE_HOST=mysql
      - WORDPRESS_ENABLE_HTTPS=yes
    volumes:
      - wordpress_data:/var/www/html
    networks: 
      - backend-network
      - frontend-network
    restart: always
    depends_on: 
      - mysql
```
* Image: bitnami/wordpress:8.0, indica la imagen que se usara para este contenedor y su versión.
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
  * WORDPRESS_DATABASE_NAME indica el nombre de la base de datos.
  * WORDPRESS_DATABASE_USER indica el usuario de la base de datos.
  * WORDPRESS_DATABASE_PASSWORD indica la contraseña del usuario de la base de datos.
  * WORDPRESS_DATABASE_HOST indica el host de la base de datos, en este caso el contenedor de mysql.
  * WORDPRESS_ENABLE_HTTPS indica si habilitar https para wordpress.
* Volumes: indica donde se localiza el volumen dentro del contenedor.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
* depends_on: indica de que servicios depende.
```yml
  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    restart: always
    environment:
      DOMAINS: '${DOMAIN} -> http://wordpress:8080'
      STAGE: 'production' 
    networks:
      - frontend-network
```
* Image: steveltn/https-portal:1, indica la imagen que se usara para este contenedor y su versión.
* Ports:
* Enviroment: indica las variables y distintos valores que necesitará la aplicación.
* Networks: indica a que conexión a la que pertenece este contenedor.
* Restart: indica las acciones que tomar al parar o reiniciar el contenedor, al poner `always` siempre se reinicia si se paró anteriormente.
```yml

volumes:
  mysql_data:
  wordpress_data:

networks: 
  backend-network:
  frontend-network:
```
