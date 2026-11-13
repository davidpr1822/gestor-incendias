# Gestor de incidencias - Despliegue con Docker Compose

Autor: David Pino

## Cómo arrancarlo

    cp .env.sample .env
    docker compose up -d
    docker compose ps

La aplicación queda en http://localhost:8080

## Preguntas

### 1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?

Porque el frontend solo muestra la web y pasa las peticiones /api/ al backend. El que habla con la base de datos es el backend.

### 2. ¿Por qué el backend pertenece a dos redes?

Porque hace de puente: recibe lo del frontend por una red y consulta la base de datos por la otra.

### 3. ¿Qué servicio necesita publicar un puerto en el anfitrión?

El frontend, porque es al que entro desde el navegador (localhost:8080). Los otros dos se hablan por dentro.

### 4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?

Porque los contenedores se comunican por la red interna, no por los puertos publicados. No arreglaría el fallo y dejaría la base de datos expuesta.

### 5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?

Todo está en un fichero y se levanta con un solo comando, en vez de hacer muchos docker run, redes y volúmenes a mano. Además es repetible y se puede subir a Git.

### 6. ¿Por qué hemos dividido la arquitectura en dos redes?

Para que cada servicio solo vea lo que necesita. Así el frontend no puede llegar a la base de datos y es más seguro.

### 7. ¿Por qué el backend utiliza db como dirección de PostgreSQL?

Porque Docker resuelve los nombres de servicio dentro de la red. localhost sería el propio backend, y la IP cambia al recrear el contenedor.
### 8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?

Al detenerlo (stop o down) los datos se quedan guardados en el volumen. Con down -v se borra el volumen y se pierden los datos.

### 9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?

Perdería todas las incidencias y su historial. Por eso hacen falta copias de seguridad.
