# Tarea 6

Para instalar PrestaShop con docker compose seguiremos los siguientes pasos:

Primero crearemos una carpeta en la cual trabajar creando el archivo .yaml que contenga la información del docker.

```
sudo mkdir compose_ps

sudo cd compose_ps

touch PrestaShop.yaml

nano PrestaShop.yaml
```

Dentro de la carpeta .yaml introduciremos la siguiente información, la cual contiene las imagenes empleadas, y los datos de acceso de la base de datos y de prestashop, junto a su IP. Formando la conexión entre ambas.

```
services:
  mysql:
    image: mysql:latest
    container_name: base_mysql 
    restart: always 
    environment:
      MYSQL_DATABASE: prestashop_db 
      MYSQL_ROOT_PASSWORD: admin 
    networks:
      - prestashop_network
  prestashop:
    image: prestashop/prestashop:latest
    container_name: prestashop
    restart: always
    depends_on:
      - mysql 
    ports:
      - 8080:80 
    environment:
      DB_SERVER: mysql
      DB_NAME: prestashop_db
      DB_USER: root 
      DB_PASSWD: admin
    networks:
      - prestashop_network
networks:
    prestashop_network:
```

Una vez hecho lanzamos el archivo y comprobamos que se ha ejecutado bien.

```
docker compose up -d
```
![](https://github.com/VictorQuinoa/Docker06/blob/main/Pull.png?raw=true)

Tras esto accedemos a la página creada mediante la ip y si todo ha ido bien se mostrará la página. En este caso comenzamos la instalación.

```
http://localhost:8080
```
![](https://github.com/VictorQuinoa/Docker06/blob/main/Instalador.png?raw=true)

En esta página, debemos introducir los datos establecidos en el .yaml para la configuración de la conexión. La dirección del servidor sera la establecida seguida de : y el puerto predeterminado de mysql.

![](https://github.com/VictorQuinoa/Docker06/blob/main/Configuracion.png?raw=true)

Cuando acabe la instalación veremos que no es posible acceder a la administración de la tienda, para ello necesitamos unos pasos finales:

```
docker exec -it prestashop rm -rf /var/www/html/install

docker exec -it prestashop mv /var/www/html/admin /var/www/html/admin552vw8sb9uvj8ucjobz

http://localhost:8080/admin552vw8sb9uvj8ucjobz
```

Con esto, eliminamos la carpeta install como se nos indica al finalizar la instalación y le cambiamos el nombre a la carpeta admin, nombre el cual usaremos junto a la direccion http://localhost:8080 para poder acceder a la administración, introduciendo el usuario y contraseña antes establecidos.

Y ya tendriamos todo listo.

![](https://github.com/VictorQuinoa/Docker06/blob/main/Screenshot_1.png?raw=true)
