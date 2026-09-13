# Laboratorio 02

- Profesor: Ing. Walter Ivan Leturia Rodriguez

## Stack
API
- Minimal API
- Debe retornar un mensaje incluyendo mi nombre
- Docker

BD
- PostgreSQL

# Indicaciones

## 1. Archivo de Variables de Entorno (`.env`)
Se separaron las credenciales y configuraciones sensibles en un archivo externo `.env` ubicado en la misma carpeta del proyecto:
```env
MESSAGE=<Hola, gusta la me milanesa>
POSTGRES_USER=milanesa
POSTGRES_PASSWORD=milanesa0324
POSTGRES_DB=mibd
```

## Configuración de contenedores y volumen
El archivo de yaml despliega tres instancias en paralelo de la API utilizando `deploy: replicas: 3` y define el mensaje personalizado:

```yaml
services:
  api:
    build: ./hello-world-api
    deploy:
      replicas: 3
    ports:
      - "3000"
    environment: 
      - MESSAGE=${MESSAGE}

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```
## Comandos principales
```bash
docker compose up -d
docker compose ps
docker compose down
```
## Configuración por entorno
```
MESSAGE=<Hola, me gusta la milanesa>
```
## Verificación del funcionamiento
### 1. Comprobar la API en el navegador

Para comprobar que funciona, ir a la URL en el navegador:
```
http://localhost:...
```
Después de los 2 puntos copiar el número que sale antes del "3000/tcp", que es el puerto para usarlo en el navegador.


### 2. Comprobar la BD PostgreSQL

```
docker exec -it dockercompose-db-1 psql -U milanesa -d mibd -c "SELECT current_database();"
```
### 3. Comprobar con Volúmenes
Para comprobar que los datos siguen al apagar los contenedores:
```
docker exec -it dockercompose-db-1 psql -U milanesa -d mibd

CREATE TABLE studiantes (id SERIAL PRIMARY KEY, nombre VARCHAR(50));

INSERT INTO studiantes (nombre) VALUES ('Facundo Javier Laredo Cuba');

\q

docker compose down

docker compose up -d

docker exec -it dockercompose-db-1 psql -U milanesa -d mibd -c "SELECT * FROM studiantes;"
```

## Control de Versiones (Conventional Commits)
El desarrollo del proyecto sigue la especificación de **Conventional Commits** para estructurar el historial de cambios de manera clara y profesional:
- `feat(stack): initialize docker compose laboratory with api, db, volumes and env`

# Tipos de redes y los tipos de volumen que existen en docker

Los tipos de redes que tenemos son bridge, host y none.
- bride: Tipo de red por defecto al crear contenedores
- host: Ofrece un servicio que tiene configurado en el puerto de red anfitrión. Permite puertos accesibles desde el host.
- none: Se suele usar para ejecutar trabajos por lotes. No tiene ninguna ip ni acceso a red externa.
- overlay: Usado con Docker Swarm para comunicar contenedores entre múltiples hosts
- macvlan: Asigna una IP directamente desde la red del host. Útil para apps legacy

Los tipos de volúmenes son docker volume (volúmenes gestionados), bind mounds y tmpfs:
- docker volume: Se guarda en ```/var/lib/docker/volumes``` del host. Se usa para compartir datos entre contenedores.
- bind mounds: Se guarda en cualquier ruta del host. Se usa en el desarrollo local y sincronización en vivo.
- tmpfs: Se guarda en la memoria RAM. Se usa en apps sensibles que no necesitan persistencia.

## EVIDENCIAS

assets/tresInstancias.png

assets/instanciasPuerto.png

assets/mensaje1.png

assets/BaseDatos.png

assets/BaseDatosInstancias.png

assets/entorno.png

assets/datos.png

assets/volume.png

assets/persistencia1.png

assets/persistencia2.png

assets/commits.png

assets/repositorio.png

### Páginas revisadas: 

- https://iesgn.github.io/curso_docker_2021/sesion4/tipos.html
- https://90daysdevops.295devops.com/semana-02/dia11/
# Alumno
- Facundo Javier Laredo Cuba