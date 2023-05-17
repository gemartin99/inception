# inception

Dockerfile de NGINX

<img width="713" alt="Screen Shot 2023-05-17 at 11 47 56 PM" src="https://github.com/gemartin99/inception/assets/66915274/e18e9e62-c452-47ca-abec-75c25b5535ca">

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

<img width="496" alt="Screen Shot 2023-05-18 at 12 55 17 AM" src="https://github.com/gemartin99/inception/assets/66915274/0eee6c36-aa3b-493b-b0de-39b4b6beb7ed">

Listen: El servidor virtual escucha en el puerto 443 para conexiones SSL/TLS. La segunda línea con [::] indica el soporte para conexiones IPv6.

SSL: Estas líneas configuran la seguridad SSL/TLS para el servidor virtual. ssl_protocols especifica los protocolos SSL/TLS permitidos: TLSv1.2 y TLSv1.3. ssl_prefer_server_ciphers indica que se deben preferir los cifrados del servidor. ssl_certificate y ssl_certificate_key especifican la ubicación de los archivos de certificado y clave privada utilizados para la comunicación segura (debe ser la misma ruta del dockerfile).

Server_name: Especifica los nombres de dominio o direcciones IP a los que el servidor virtual responderá. 

Root: Se define la ruta de acceso del directorio raiz, es decir, el directorio base desde el cual se sirven los archivos.

Index: Establece los nombres de archivo que se utilizarán como índices si se solicita un directorio.

Location /: La directiva try_files intenta servir la solicitud según el valor de $uri. Si no se encuentra el archivo solicitado, se devuelve una respuesta de estado 404 (no encontrado).

Que es la variable uri? La variable $uri en una configuración de NGINX, se refiere al URI de la solicitud actual. Por ejemplo, si la URL completa es https://example.com/path/file.html, entonces $uri representaría /path/file.html. Cuando ponemos location / nos referimos a la ubicacion raiz del servidor que hemos definido anteriormente, es ahi donde buscara los ficheros.

