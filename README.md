# Legacy DHIS2 Core images

 The images are built from official war files by [DHIS2](https://dhis2.org/) for legacy versions that aren't available in the official [DHIS2 core](https://hub.docker.com/r/dhis2/core) docker images
 ## Available versions

    * 2.25
    * 2.27
    * 2.28
    * 2.29
    * 2.30
## How to setup

 The images require environmental variables

     *  DHIS_HOME  
             - DHIS2 core home directory for all DHIS2 related volume artefacts storage
     *  POSTGRES_HOST 
            - The PostgreSQL database for DHIS2 instance
     *  DB_NAME
            - Database name
     *  POSTGRES_USER
            - Postgres user with access to the database DHIS2 is connecting to
     *  POSTGRES_PASSWORD
          - Password for the postgres user connecting to the database

## Sample docker-compose setup

         version: '3.2'
         
         services:
           dhis-25-db:
             container_name: dhis-25-db
             image: mdillon/postgis:10-alpine
             command: postgres -c max_locks_per_transaction=1000 -c max_connections=3000
             environment:
               - POSTGRES_USER=dhis
               - POSTGRES_DB=dhis
               - POSTGRES_PASSWORD=dhis
             restart: always
             volumes:
               - db-volume:/var/lib/postgresql/data
           dhis-25-dhis:
             container_name: dhis-25-dhis
             image: baharajr/dhis2:2.25
             restart: always
             environment:
               - DHIS_HOME=/opt/dhis2
               - POSTGRES_HOST=dhis-25-db
               - DB_NAME=dhis
               - POSTGRES_USER=dhis
               - POSTGRES_PASSWORD=dhis
             volumes: 
               - ./conf:/DHIS2_home
             depends_on: 
               - dhis-25-db
             healthcheck:
               test: ["CMD", "curl", "-f", "http://localhost:8080/"] 
               interval: 30s
               timeout: 30s
             ports:
               - 8080:8080
         volumes:
           db-volume:
             driver: local
