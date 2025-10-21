# Dockerfile
Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones que Docker utiliza para construir una imagen de contenedor Docker. Este conjunto de instrucciones define cómo se debe configurar y construir una imagen de contenedor, incluyendo qué sistema operativo base usar, qué software instalar, qué archivos copiar en el contenedor y cómo configurar el entorno de ejecución.
 ![Dockerfile](img/relacion.PNG)


Tradicionalmente, el archivo docker no tiene extensión. Simplemente se denomina Dockerfile sin ningún tipo de extensión. Adicionalmente, los Dockerfiles pueden ser creados usando la extensión .dockerfile. Esto se utiliza cuando hay una necesidad de almacenar múltiples archivos docker en un solo directorio.
Las instrucciones en un Dockerfile son simples y están diseñadas para ser leídas y comprendidas fácilmente. 

### FROM 
Especifica la imagen base que se utilizará como punto de partida para construir la nueva imagen de Docker. Por ejemplo, FROM nginx:alpine significa que la nueva imagen se basará en la imagen oficial de Nginx que está etiquetada como "alpine". Al utilizar una imagen base existente, se heredan todas las configuraciones y software incluidos en esa imagen, lo que facilita la creación de nuevas imágenes sin tener que empezar desde cero.

### RUN
Ejecuta comandos en el contenedor durante el proceso de construcción de la imagen. Puedes usar esta instrucción para instalar software, configurar el entorno, o realizar cualquier otra tarea necesaria para preparar la imagen. Por ejemplo, RUN apt-get update && apt-get install -y <paquete> instalará un paquete utilizando el administrador de paquetes apt en una imagen basada en Ubuntu.

### COPY
Copia archivos o directorios desde la máquina host al sistema de archivos del contenedor. Se utiliza para agregar archivos, scripts u otros recursos necesarios para la aplicación en la imagen de contenedor. Por ejemplo, COPY ./mi-app /app copiará el directorio mi-app desde la máquina host al directorio /app en el contenedor.

### ADD
Copia archivos o directorios desde el sistema de archivos de la máquina host al sistema de archivos del contenedor. Aunque es similar a la instrucción COPY, ADD tiene algunas características adicionales, como la capacidad de descomprimir automáticamente archivos y de copiar archivos desde una URL remota.

### CMD 
Define el comando predeterminado que se ejecutará cuando se inicie el contenedor. Puede haber solo una instrucción CMD en un Dockerfile. Si se proporciona más de una, solo la última tendrá efecto. Por ejemplo, CMD ["nginx", "-g", "daemon off;"] ejecutará el servidor web Nginx en modo daemon cuando se inicie el contenedor.

### ENTRYPOINT
Configura el comando o el script que se ejecutará cuando se inicie un contenedor basado en la imagen. A diferencia de la instrucción CMD, que especifica el comando predeterminado que se ejecutará y que puede ser sobrescrito desde la línea de comandos al iniciar el contenedor, ENTRYPOINT establece un comando que no se puede sobrescribir fácilmente al iniciar el contenedor.
Es importante destacar que, si se proporciona un comando en la línea de comandos al iniciar el contenedor (por ejemplo, docker run <imagen> <comando>), este comando se agregará como argumentos al comando especificado en ENTRYPOINT, en lugar de sobrescribirlo. Esto permite que el contenedor sea más versátil y se adapte a diferentes necesidades de uso.

### EXPOSE
Informa a Docker que el contenedor escuchará en un puerto específico en tiempo de ejecución. No abre realmente el puerto, solo documenta que la aplicación dentro del contenedor puede usar dicho puerto. Por ejemplo, EXPOSE 80 expone el puerto 80 en el contenedor, lo que permite que se acceda a la aplicación que se esté ejecutando en ese puerto desde fuera del contenedor.

### ENV
Define variables de entorno dentro del contenedor. Las variables de entorno definidas con ENV estarán disponibles para cualquier proceso en el contenedor durante su ejecución. Por ejemplo, ENV MYSQL_ROOT_PASSWORD=password define una variable de entorno llamada MYSQL_ROOT_PASSWORD con el valor password.

### VOLUME 
Esta instrucción se se utiliza para definir un punto de montaje para volúmenes dentro del contenedor.

##  Para construir una imagen de Docker a partir de un Dockerfile
```
docker build -t <nombre imagen>:<tag> .
```
- **-t** esta opción se utiliza para etiquetar la imagen que se está construyendo con un nombre y una versión.
- <nombre_imagen>:<tag> por ejemplo: myapp:1.0
- **.** este punto indica al comando docker build que busque el Dockerfile en el directorio actual, es decir especifica la ubicación del contexto de la construcción que incluye el Dockerfile y cualquier otro archivo necesario para la construcción de la imagen.

## Ejemplo
### Colocar las siguientes instrucciones en un Dockerfile, 
![Dockerfile](img/Dockerfile.PNG)

- apachectl: Es el script de control para el servidor web Apache. Se utiliza para iniciar, detener y controlar el servidor web.
- -D FOREGROUND: Esta opción le dice a Apache que se ejecute en primer plano. Por defecto, Apache se ejecuta como un servicio en segundo plano. Sin embargo, en un contenedor Docker, es preferible que el proceso principal (en este caso, Apache) se ejecute en primer plano para que Docker pueda monitorear el estado del proceso. Si Apache se ejecutara en segundo plano, Docker no podría saber si el servidor web está funcionando correctamente o no.

Por ejemplo dentro del ejemplo he creado un archivo /web/index.html para mostrarlo para el caso 
<img width="512" height="94" alt="image" src="https://github.com/user-attachments/assets/ed689806-fc00-4f9c-b82a-25850701ee5e" />

Tomando en cuenta que la version 7 de centos cuenta con errores, obtengo un error de configuracion de DNS 
```
=> ERROR [2/4] RUN yum update -y                                                                                                         1.9s
------
 > [2/4] RUN yum update -y:
0.887 Loaded plugins: fastestmirror, ovl
1.194 Determining fastest mirrors
1.451 Could not retrieve mirrorlist http://mirrorlist.centos.org/?release=7&arch=x86_64&repo=os&infra=container error was
1.451 14: curl#6 - "Could not resolve host: mirrorlist.centos.org; Unknown error"
1.462
```

Agregare las siguientes lineas 
```
# Reemplazar mirrors antiguos por los del Vault
RUN sed -i 's|mirrorlist=|#mirrorlist=|g' /etc/yum.repos.d/CentOS-*.repo && \
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*.repo
```


### Ejecutar el archivo Dockerfile y construir una imagen en la versión 1.0
No olvides verificar en qué directorio se encuentra el archivo Dockerfile

Primero construyo la imagen a partir del dockerfile 
```
docker build -t centos7-apache .
```
Para posteriormente ejecuatar el contenedor:

```
docker run -d -p 8080:80 --name centos7-web centos7-apache
```


**¿Cuántos pasos se han ejecutado?**
Se han ejecutado 7 pasos (incluido la configurar por cuestion de los mirrors antiguos) en la construccion y configuración de la imagen.

### Inspeccionar la imagen creada
Aqui se muestra la imagen creada con el archivo index.html que salia de ./web
<img width="1855" height="936" alt="image" src="https://github.com/user-attachments/assets/4df9ef43-ef76-4641-9c88-4aa0dab6ec66" />


**Modificar el archivo index.html para incluir su nombre y luego crear una nueva versión de la imagen anterior**
**¿Cuántos pasos se han ejecutado? ¿Observa algo diferente en la creación de la imagen**

Se siguen ejecutando los 7 pasos anteriores del archivo Dockerfile, sin distincion, sin embargo, corrigiendo el archivo index.html, con el contenido se debe colocar una nueva reconstruccion de la imagen en version2.

```
docker build -t centos7-apache:v2 .
```
y
```
docker run -d -p 8080:80 --name centos7-web_v2 centos7-apache:v2
```
<img width="1811" height="861" alt="image" src="https://github.com/user-attachments/assets/6fe262f6-a956-4a68-a1c1-ff0ffdd42dcc" />


## Mecanismo de caché
Docker usa un mecanismo de caché cuando crea imágenes para acelerar el proceso de construcción y evitar la repetición de pasos que no han cambiado. Cada instrucción en un Dockerfile crea una capa en la imagen final. Docker intenta reutilizar las capas de una construcción anterior si no han cambiado, lo que reduce significativamente el tiempo de construcción.

- Instrucción FROM: Si la imagen base ya está en el sistema, Docker la reutiliza.
- Instrucciones de configuración (ENV, RUN, COPY, etc.): Docker verifica si alguna instrucción ha cambiado. Si no, reutiliza la capa correspondiente de la caché.
- Instrucción COPY y ADD: Si los archivos copiados no han cambiado, Docker reutiliza la capa de caché correspondiente.
![mapeo](img/dockerfile-cache.PNG)

### Crear un contenedor a partir de las imagen creada, mapear todos los puertos
```
docker run -d -P --name contenedor-apache centos7-apache:v2
```

### ¿Con que puerto host se está realizando el mapeo?
<img width="1722" height="95" alt="image" src="https://github.com/user-attachments/assets/3756b3f4-619c-48e8-beb5-141802d3f7bb" />
De acuerdo a lo que ha realizado docker, el contenedor expone el puerto 80 y Docker lo ha mapeó automaticamente en el puerto 32768 del host 

**¿Qué es una imagen huérfana?**
Una imagen huérfana (o dangling image) es una imagen sin etiqueta (<none>) que ya no está asociada a ningún contenedor ni versión activa.
Estas aparecen normalmente cuando se reconstruye una imagen y docker remplaza las capas antiguas

### Identificar imágenes huérfanas
```
docker images -f "dangling=true"
```
<img width="631" height="78" alt="image" src="https://github.com/user-attachments/assets/41d204c6-1d93-4948-ace1-57c1904b7788" />


### Listar los IDS de las imágenes huérfanas
```
docker images -f "dangling=true" -q
```
<img width="624" height="56" alt="image" src="https://github.com/user-attachments/assets/d860f90b-cebc-4b6a-a863-d001a2d25224" />


### Eliminar imágenes huérfanas
Este comando eliminará todas las imágenes que no estén asociadas a ningún contenedor en ejecución. Antes de ejecutarlo, asegúrate de revisar las imágenes que serán eliminadas para evitar la pérdida de imágenes importantes. 
```
docker image prune
```
<img width="596" height="106" alt="image" src="https://github.com/user-attachments/assets/6873e629-1ac5-4333-a6bd-77203b009752" />

### Para Ejecutar un archivo Dockerfile que tiene otro nombre
```
docker build -t <nombre imagen>:<tag> -f <ruta y nombre del Dockerfile> .
```

## Por ejemplo
docker build -t imagen:1.0 -f Dockerfile-custom .


