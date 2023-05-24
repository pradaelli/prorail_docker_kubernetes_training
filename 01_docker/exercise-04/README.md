# Exercise 04

Sunny bikes now wants to productionize their API. 
This includes persistent storage and the possibility to scale out.

Update the `docker-compose.yaml` to run the following images: 
- `postgres:11-alpine`
- Image built from `Dockerfile` (from previous exercise)

The database should be secured with password `long-distance-ice-skating` and initialised with the `init-schema.sql` script

Expected result:
- Insert data on `127.0.0.1:8080/rent?name=bob&location=texas`
- Browse to url:8080 and see the inserted data


Some tips:
- the password needs to be assigned to environment variable `POSTGRES_PASSWORD` for postgres and `PG_PASSWORD` for flask.
- the `init-schema.sql` file needs to be mapped to the folder `./docker-entrypoint-initdb.d/`
- the Flask service should depend on the Postgres service

Run with `docker compose up --build`

Bonus:
- add an extra route to the `app.py` file `/hello` that displays a friendly message and then create a mount in the flask service such that changing the message in your local files is reflected immediately in the running app
- browse in the Postgres container with:
```bash
docker exec -ti <container name> psql -U postgres

# list tables
\d

# select from table bike_rides
SELECT * FROM bike_rides
```