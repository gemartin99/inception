# inception

### Comandos mas comunes en docker:

◦ ```docker run```: Este comando se utiliza para ejecutar un contenedor a partir de una imagen. Con el dlag -d se ejecuta en segundo plano y con el flag -it se ejecuta en primer plano.

◦ ```docker build```: Este comando se utiliza para construir una imagen de Docker a partir de un archivo de Dockerfile. El dockerfile es un archivo de configuración que define cómo se construira la imagen.

◦ ```docker pull```: Con este comando puedes descargar una imagen de Docker desde un registro publico o privado.

◦  ```docker push```: Este comando se utiliza para subir una imagen de Docker a un registro, como podria ser Docker Hub. Esto permite que otras personas puedan descargar tu imagen y la utilicen en sus sistemas.

◦ ```docker stop```: Detiene la ejecucion de uno o mas contenedores. Puedes especificar el nombre o el ID del contenedor.

◦ ```docker rm```: Este comando detiene la ejecucion de una o mas contenedores. Puedes especificar el nombre o el ID del contenedor.

◦ ```docker ps```: Muestra una lista de los contenedores que estan en ejecucion actualmente. Con la flag -a tambien se muestran los que no estan en ejecucion.

◦ ```docker images```: Este comando muestra una lista de las imagenes que tienes localmente en tu sistema. Tambien muestra informacion como el tamaño de las imagenes y el ID.

◦ ```docker exec```: Este comando se utiliza para ejecutar un comando dentro de un contenedor en ejecucion.

◦ ```docker-compose up```: Inicia todos los servicios definidos en un archivo (docker-compose.yml).

◦ ```docker logs```: Muestra los registros generados por un contenedor en ejecucion. Con el flag -f lo puedes seguir en tiempo real.

◦ ```docker inspect```: Proporciona informacion detallada sobre un objeto de Docker, como un contenedor, una imagen , una red, etc.

◦ ```docker network```: Permite administrar las redes de Docker.

◦ ```docker restart```: Reinicia uno o mas contenedores en ejecucion.

◦ ```docker kill```: Detiene abruptamente la ejecucion de uno o mas contenedores.

◦ ```docker pause```: Pausa la ejecución de uno o mas contenedores.

◦ ```docker unpause```: Reanuda la ejecucion de uno o mas contenedores pausados previamente.

◦ ```docker stats```: Muestra en tiempo real las estadisticas de uso de recursos (CPU, memoria, etc) de los contenedores en ejecucion.

◦ ```docker-compose down```: Detiene y elimina los servicios definidos en un archivo docker-compose.yml. 



## NGINX


### Dockerfile de NGINX


<img width="669" alt="Screen Shot 2023-06-08 at 3 51 39 PM" src="https://github.com/gemartin99/inception/assets/66915274/d55a55b3-1db4-4009-a405-c06dc38d0cd2">

FROM: Creamos un contenedor vacio con todo lo basico que trae debian:buster (SO).

RUN: ejecutamos apt get y get-install -y nginx openssl para instalar esos paquetes dentro de nuestro contenedor.

RUN: creamos la carpeta ssl en el directorio /etc/nginx/

RUN: utilizamos openssl req ya que permite crear una nueva solicitud de certificado, lo que hara el comando entero con todos sus flags es generar una nueva clave privada RSA de 2048 bits.

-out ruta: Se especifica el archivo de salida donde se guardara el certificado.
-keyout ruta: Se especifica el archivo de salida donde se guardara la clave privada.
-subj ruta: Contiene información identificativa sobre la entidad. En este caso, se establece la siguiente información del sujeto:
    C (Country): "ES" para España.
    ST (State): "Madrid" como estado o provincia.
    L (Locality): "Madrid" como localidad o ciudad.
    O (Organization): "Mi Organización" como nombre de la organización.
    OU (Organizational Unit): "Mi Unidad" como unidad organizativa.
    CN (Common Name): "ejemplo.es" como nombre común (generalmente el nombre de dominio).

### Nginx.conf 

<img width="581" alt="Screen Shot 2023-06-08 at 3 54 36 PM" src="https://github.com/gemartin99/inception/assets/66915274/e8b46a12-c31c-4561-bd22-84fa9b1a9046">


Listen: El servidor virtual escucha en el puerto 443 para conexiones SSL/TLS. La segunda línea con [::] indica el soporte para conexiones IPv6.

SSL: Estas líneas configuran la seguridad SSL/TLS para el servidor virtual. ssl_protocols especifica los protocolos SSL/TLS permitidos: TLSv1.2 y TLSv1.3. ssl_prefer_server_ciphers indica que se deben preferir los cifrados del servidor. ssl_certificate y ssl_certificate_key especifican la ubicación de los archivos de certificado y clave privada utilizados para la comunicación segura (debe ser la misma ruta del dockerfile).

Server_name: Especifica los nombres de dominio o direcciones IP a los que el servidor virtual responderá. 

Root: Se define la ruta de acceso del directorio raiz, es decir, el directorio base desde el cual se sirven los archivos.

Index: Establece los nombres de archivo que se utilizarán como índices si se solicita un directorio.

Location /: La directiva try_files intenta servir la solicitud según el valor de $uri. Si no se encuentra el archivo solicitado, se devuelve una respuesta de estado 404 (no encontrado).

Que es la variable uri? La variable $uri en una configuración de NGINX, se refiere al URI de la solicitud actual. Por ejemplo, si la URL completa es https://example.com/path/file.html, entonces $uri representaría /path/file.html. Cuando ponemos location / nos referimos a la ubicacion raiz del servidor que hemos definido anteriormente, es ahi donde buscara los ficheros.

## MARIADB

### Dockerfile de MariaDB

<img width="565" alt="Screen Shot 2023-06-19 at 6 24 21 PM" src="https://github.com/gemartin99/inception/assets/66915274/c89f56ab-2531-4bc3-863a-475176765e22">

FROM: Creamos un contenedor vacio con todo lo basico que trae debian:buster (SO).

RUN: Ejecutamos apt-get update para actualizar la lista de paquetes y apt-get install -y mariadb-server para instalar el paquete de mariadb.

COPY: Se copiará el archivo de configuración 'mariadb.conf' ubicado en la carpeta 'conf' a la ruta especificada ```/etc/mysql/mariadb.conf.d/mariadb.conf``` dentro de nuestro contenedor recien creado.

RUN: ```mkdir -p /var/run/mysqld``` creamos la siguiente carpeta. Con la opción -p lo que hacemos es asegurar que se crean todos los directorios intermedios. Acto seguido hacemos uso del comando ```chown -R mysql:mysql /var/run/mysqld``` este lo que hará será cambiar el propietario y grupo del directorio, lo asignamos al usuario mysql y al grupo mysql. Por último con el comando ```chmod 777 /var/run/mysqld``` establecemos los permisos de lectura, escritura y ejecucion a todos los usuarios del sistema en la ruta especificada.

EXPOSE: Se está indicando que el contenedor expondrá el puerto 3306, que es el puerto por defecto utilizado por MariaDB/MySQL para aceptar las conexiones de clientes. Es importante saber que la instruccion EXPOSE como tal no abre los puertos, simplemente indica cuales son los puertos a exponer, para que realmente se abran se debera utilizar la flag -p al ejecutar el contenedor, por ejemplo: ```docker run -p 3306:3306 nombre_de_la_imagen```.

COPY: Copiamos el script ubicado en la carpeta 'tools' a la ruta especificada ```/usr/local/bin/``` dentro de nuestro contenedor.

RUN: Con el comando ```chmod 755 /usr/local/bin/mariadb.sh``` modificaremos los permisos para que el propietario del archivo tenga permisos de lectura, escritura y ejecucion, mientras que el resto de usuarios solo tendran permisos de lectura y ejecucion.

RUN: Con el comando ```mysql_install_db``` ejecutaremos el script mysql_install_db dentro de nuestro contenedor y este script lo que hara es inicializar la estructura de directorios y archivos necesarios, crea la base de datos de sistema, archivos de registro y las tablas de permisos.

ENTRYPOINT: Al definir un entrypoint lo que hacemos sera establecer que comando o script debe ejecutarse automaticamente al iniciar el contenedor, en nuestro caso ejecutaremos el script ubicado en ```/usr/local/bin/mariadb.sh``` que es el que hemos copiado previamente de la carpeta 'tools'.

### Mariadb.conf

Este archivo de configuración es utilizado por el servicio MySQL para especificar como debe funcionar el servidor de bases de datos.

<img width="421" alt="Screen Shot 2023-06-20 at 11 44 37 AM" src="https://github.com/gemartin99/inception/assets/66915274/2143199f-8262-45dc-9805-8ce698366673">

[mysqld]: Esta linea indica que las siguientes configuraciones se aplicaran al servidor de MySQL.

user: Especifica el usuario bajo el cual se ejecutara el servidor MySQL.

port: Indica el numero de puerto en el que el servidor esuchara las conexiones entrantes.

datadir: Establece la ruta del directorio de datos de MySQL. Ahi se almacenaran las bases de datos y archivos relacionados.

socket: Especifica el archivo de socket que se utilizara para las conexiones locales al servidor. Cuando un cliente de MySQL se conecta al servidor utilizando una conexión local, en lugar de una conexión a través de la red, se utiliza este archivo de socket para la comunicación. El cliente y el servidor interactúan a través de este archivo compartido en lugar de utilizar una dirección IP y un puerto como lo harían en una conexión remota.

pid-file: Indica la ruta del archivo donde se almacenara el PID del servidor.

bind-address: Determina la direccion IP a la que se enlazara el servidor. He puesto 0.0.0.0 , de esta manera el servidor estara disponible en todas las interfaces de red.

basedir: Especifica la ruta del directorio base de instalacion de MySQL.

tmpdir: Indica el directorio temporal donde se almacenaran los archivos temporales generados por el servidor.

log_error: Establece la ubicacion del archivo de registro de errores del servidor de MySQL. En esta ruta se almacenaran los mensajes de error y eventos importantes.

character-set-server: Define el conjunto de caracteres predeterminados que se utilizaran para las nuevas bases de datos y tablas. Ponemos este para asegurarnos que se guardaran y mostraran correctamente caracteres especiales como la 'ñ' etc.

query_cache_size: Especifica el tamaño de la memoria cache de consultas. Al almacenar en cache consultas frecuentes MySQL puede responder a esas consultas de manera mas rapida, de esta manera aceleramos el rendimiento.

### Script MariaDB

<img width="805" alt="Screen Shot 2023-06-20 at 12 07 06 PM" src="https://github.com/gemartin99/inception/assets/66915274/0af6986c-f984-42c1-8a05-551b21ef3e25">

🔴 Inicia el servicio MySQL. El servidor estara lito para aceptar conexiones.

🟠 Condicion que si no existe el directorio de la base de datos entrara al if. Si no se cumple la condicion directamente pasara al paso 🟤.

🟡 Nos conectamos a MySQL mediante nuestro usuario y contraseña (utilizamos las variables de entorno) y ejecutamos la instruccion ```CREATE DATABASE $MYSQL_DATABASE;``` para crear una nueva base de datos con el nombre almacenado en la variable de entorno.

🟢 Crearemos un usuario dentro de la base de datos especificando el nombre y la contraseña (utilizamos las variables de entorno).

🌐 Le concedemos todos los permisos al usuario recien creado para que pueda acceder a la base de datos y realizar cualquier operación en las tablas.

🔵 Actualizamos los privilegios para que los cambio realizados se apliquen.

🟣 Cambiamos la contraseña por defecto del usuario root de MySQL a la que tenemos almacenada en nuestra variable de entorno.

🟤 Apagamos el servidor utilizando el usuario y contraseña de root.

⚪️ Iniciamos el servidor MySQL de nuevo. Hemos realizado este reinicio para que el servidor se inicie con la configuracion y cambios realizados en el script.

### Dockerfile Wordpress

<img width="1048" alt="Screen Shot 2023-06-21 at 12 00 57 PM" src="https://github.com/gemartin99/inception/assets/66915274/daeb64a8-ecc3-4f0f-9e2c-044cadb3fe3e">

FROM: Creamos un contenedor vacio con todo lo basico que trae debian:buster (SO).

RUN: Actualizamos la lista de paquetes y luego instalamos los paquetes necesarios para ejecutar PHP-FPM y MySQL. Instalamos la utilidad curl que permite realizar solicitudes HTTP y descargar archiovs desde una URL. php7.3-fpm es un modulo de php que gestiona la ejecucion de scripts php. php7.3-mysqli este paquete es una extension de PHP para interactuar con la base de datos MySQL. mariadb-client permite interactuar con una base de datos MariaDB o MySQL desde linea de comandos.

RUN: Descargamos el archivo ejecutable de WP-CLI, este es una herramienta que proporciona una interfaz de linea de comandos para administrar y gestionar instalaciones de Wordpress. Este archivo se guarda con el nombre wp-cli.phar en el directorio actual del contenedor. Con el siguiente comando ```chmod +x wp-cli.phar``` se otorgan permisos de ejecucion al archivo. Por ultimo con ```mv wp-cli.phar /usr/local/bin/wp``` movemos el archivo al directorio especificado y le cambiamos el nombre,  se hace accesible globalmente en el sistema de archivos del contenedor.

COPY: Copiamos el archivo www.conf de nuestra maquina a la ruta /etc/php/7.3/fpm/pool.d/ de nuestro contenedor.

RUN: con el comando ```mkdir -p /run/php``` creamos el directorio indicado y sus subdirectorios si es que no existen.

RUN: Con el siguiente comando ```chmod 755 /run/php``` modificaremos los permisos para que el propietario del archivo tenga permisos de lectura, escritura y ejecucion, mientras que el resto de usuarios solo tendran permisos de lectura y ejecucion.

COPY: Copiamos el archivo wordpress.sh de nuestra maquina y lo pegamos en la ruta /usr/local/bin/ de nuestro contenedor.

RUN: Otorgamos permisos de ejecucion al archivo.

EXPOSE: Exponemos el puerto 9000 del contenedor. Esto permite que otros contenedores o el host accedan al servicio PHP-FPM en el puerto 9000.

WORKDIR: Establecemos la ruta ```/var/www/html/``` como el directorio raiz donde se alojaran los archivos de WordPress.

ENTRYPOINT: Cuando el contenedor se inicie, se ejecutará el script wordpress.sh

### www.conf

Este archivo es utilizado por PHP-FPM para configurar los procesos en la ejecucion de PHP.

<img width="767" alt="Screen Shot 2023-06-21 at 12 18 57 PM" src="https://github.com/gemartin99/inception/assets/66915274/01ad93b0-fc82-4de3-9c50-10997d0aaf66">

[ www ]: Indica el bloque de configuración específico para el pool de trabajadores predeterminado.

user: Establece el usuario bajo el cual se ejecutaran los scripts de PHP.

group: Establece el grupo al que pertenece el user.

listen: Especifica el puerto de escucha eb el cual PHP-FPM aceptara conexiones.

listen.owner: Establece el propietario del socket de escucha.

listen.group: Establece el grupo del socket de escucha.

listen.mode: Establece los permisos del socket de escucha, se establece en 0660, de mode que el propietario y el grupo tienen permisos de lectura y escritura, mientras que otro no tienen permisos.

pm: Define la estrategia de administracion de procesos para PHP-FPM. Utilizamos dinamico para que la cantidad de procesos se ajusten automaticamente segun la carga del servidor.

pm.max_children: Establecemos el numero maximo de procesos hijos que se pueden generar.

pm.start_servers: Especificamos el numero de procesos hijos que se deben iniciar automaticamente al iniciar PHP-FPM.

pm.min_spare_servers: Definimos el numero minimo de procesos hijos inactivos que se deben mantener en espera para manejar las solicitudes.

pm.max_spare_servers: Establecemos el numero maximo de procesos hijos inactivos que se deben mantener en espera.


