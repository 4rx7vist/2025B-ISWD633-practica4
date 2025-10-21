# Limitar uso de procesador
Limitar la cantidad de núcleos de CPU:
```
--cpus=<número de núcleos>
```

Asignar núcleos de CPU específicos:
```
--cpuset-cpus=<lista de núcleos>
```

**¿Como saber el numero de procesadores virtuales que tiene una máquina?**

Yo puedo saber el numero de procesadores virtuales en una máquina a partir del comando
```
docker exec server-nginx nproc
```

Por ejemplo para la instancia de nginx
<img width="557" height="64" alt="image" src="https://github.com/user-attachments/assets/f3cc1a95-e24c-4726-aea6-952c0990a386" />


## Ejemplos
_Puedes copiar y ejecutar directamente cada uno de los comandos_

Limitar el uso de CPU a 1.5 núcleos
```
docker run -d --name server-nginx --cpus="1.5" nginx:alpine
```

<img width="952" height="79" alt="image" src="https://github.com/user-attachments/assets/f558ab07-cc60-4179-b7f3-c23a791883bd" />

Restringir el contenedor para que use los núcleos de CPU 0 a 2:
```
docker run -d --name server-nginx --cpuset-cpus="0-2" nginx:alpine
```

<img width="998" height="54" alt="image" src="https://github.com/user-attachments/assets/93fad06e-1bf1-42cd-9188-a35a0277b4db" />


Restringir el contenedor para que use los núcleos de CPU 1 y 3:
```
docker run -d --name server-nginx --cpuset-cpus="1,3" nginx:alpine
```

<img width="1007" height="57" alt="image" src="https://github.com/user-attachments/assets/4b0965cb-f4a1-4ec5-bafb-1fbc90dcbf75" />

