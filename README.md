# Python/Odoo Programmer Test
## Odoo Implementation
I used a Docker Hub image using WSL2 with an Ubuntu 20.04 LTS virtual machine to run Odoo, using the 13th version. Docker-compose was the tool of preference to deploy the containers needed in order for Odoo to be displayed and customized.

The containers are:

* PostgreSQL:13
* Odoo:13

It's a requirement to create a file called "odoo_pg_pass" for managing the keys that PostgreSQL and Odoo ask.

The yaml file used:
```dockerfile
version: '3.1'
services:
  web:
    image: odoo:13.0 "13th version"
    depends_on:
      - db
    ports:
      - "8069:8069" "the port where it's going to be listening"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - PASSWORD_FILE=/run/secrets/postgresql_password
    secrets:
      - postgresql_password
  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD_FILE=/run/secrets/postgresql_password
      - POSTGRES_USER=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
    secrets:
      - postgresql_password
volumes:
  odoo-web-data:
  odoo-db-data:

secrets:
  postgresql_password:
    file: odoo_pg_pass "local file"
```
After Odoo is deployed
