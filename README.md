# inception

Dockerfile de NGINX


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

Nginx.conf 

<img width="581" alt="Screen Shot 2023-06-08 at 3 54 36 PM" src="https://github.com/gemartin99/inception/assets/66915274/e8b46a12-c31c-4561-bd22-84fa9b1a9046">


Listen: El servidor virtual escucha en el puerto 443 para conexiones SSL/TLS. La segunda línea con [::] indica el soporte para conexiones IPv6.

SSL: Estas líneas configuran la seguridad SSL/TLS para el servidor virtual. ssl_protocols especifica los protocolos SSL/TLS permitidos: TLSv1.2 y TLSv1.3. ssl_prefer_server_ciphers indica que se deben preferir los cifrados del servidor. ssl_certificate y ssl_certificate_key especifican la ubicación de los archivos de certificado y clave privada utilizados para la comunicación segura (debe ser la misma ruta del dockerfile).

Server_name: Especifica los nombres de dominio o direcciones IP a los que el servidor virtual responderá. 

Root: Se define la ruta de acceso del directorio raiz, es decir, el directorio base desde el cual se sirven los archivos.

Index: Establece los nombres de archivo que se utilizarán como índices si se solicita un directorio.

Location /: La directiva try_files intenta servir la solicitud según el valor de $uri. Si no se encuentra el archivo solicitado, se devuelve una respuesta de estado 404 (no encontrado).

Que es la variable uri? La variable $uri en una configuración de NGINX, se refiere al URI de la solicitud actual. Por ejemplo, si la URL completa es https://example.com/path/file.html, entonces $uri representaría /path/file.html. Cuando ponemos location / nos referimos a la ubicacion raiz del servidor que hemos definido anteriormente, es ahi donde buscara los ficheros.

Comandos mas comunes en docker:

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
