# COMPLETAR  
Comparando sus conocimientos antes de hacer la práctica con sus conocimientos después de hacer la tarea, explicar los principales aprendizajes logrados para beneficio de su formación profesional.  
Si solucionó un problema presentado al realizar la práctica también se debe documentar.


Antes de realizar la práctica, mis conocimientos sobre Docker eran principalmente teóricos. Conocía los conceptos básicos sobre imágenes, contenedores y su utilidad general, pero no comprendía en profundidad cómo se construyen y gestionan las capas de una imagen ni cómo se configuran correctamente los repositorios dentro de un contenedor. Tampoco tenía claridad sobre cómo se realiza el mapeo de puertos entre el contenedor y el host ni cómo aprovechar el sistema de caché de Docker para optimizar los tiempos de construcción.

Después de desarrollar la práctica, logré comprender de manera más completa el proceso de creación de una imagen a partir de un Dockerfile. Aprendí la función de cada instrucción (`FROM`, `RUN`, `COPY`, `EXPOSE`, `CMD`) y cómo cada una genera una capa independiente dentro de la imagen. También pude observar cómo Docker reutiliza las capas anteriores al construir una nueva versión, ejecutando únicamente los pasos que han cambiado, lo que mejora la eficiencia y reduce considerablemente el tiempo de compilación.

Otro aprendizaje importante fue la identificación y eliminación de imágenes huérfanas. Mediante los comandos docker images -f "dangling=true" y docker image prune, pude reconocer y limpiar las imágenes que ya no estaban asociadas a ningún contenedor, optimizando el uso del espacio en disco y manteniendo un entorno de trabajo más ordenado.

En conclusión, esta práctica me permitió adquirir conocimientos prácticos sobre la creación, gestión y optimización de imágenes Docker. Comprendí mejor cómo funciona el sistema de capas, cómo aprovechar la caché para agilizar el proceso, cómo realizar un mapeo correcto de puertos y cómo resolver problemas de compatibilidad con los repositorios del sistema base. Todo esto contribuye significativamente a mi formación profesional, fortaleciendo mis competencias en el ámbito de DevOps y despliegue de aplicaciones en entornos aislados y portátiles.
