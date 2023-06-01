# Exercise 1
1. Create a yaml file with a namespace definition for sunnybikes
2. Create a yaml file with a pod definition for the sunnybikes application. For this, use the docker image `pugillum/sunnybikes:stable`.
3. Create a yaml file with a pod definition for the postgres application. For this, use the docker image `postgres:11-alpine`.
4. Apply the configurations and check the logs of the pods with: `kubectl –n sunnybikes logs <podname>`

Check the logs to see if everything has started correctly!

# Exercise 2
1. Define an environment variable for the app pod `PG_PORT` with value `"5432"`
1. Create a yaml file with a secret definition for the postgres password. Add the secret as environment variable to the sunny and postgres pods. SunnyBikes needs a `PG_PASSWORD` env variable and postgres needs a `POSTGRES_PASSWORD` env variable
1. Create a yaml file with a config map definition for the postgres init schema. Mount the init script in the postgres container in the folder `docker-entrypoint-initdb.d` as file `init-schema.sql`. The schema is as follows:
```
CREATE TABLE public.bike_rides (
    uuid UUID PRIMARY KEY,
    name VARCHAR(80) NOT NULL,
    location VARCHAR(80) NOT NULL,
    created TIMESTAMPTZ NOT NULL
)
```
1. Create a role and binding that only allows “list” on secrets in the `sunnybikes` namespace for your user.

Hint: Check the postgres logs for `CREATE TABLE`

# Exercise 3
1. Sunny Bikes has a “/healthz” endpoint that returns status code 200 if the service is up. Use that for a readiness Probe

# Exercise 4
1. Migrate the pod definitions for Sunny Bikes and postgres to a deployment
2. The Sunny Bikes pods must be replicated at least 3 times
3. Make sure that when a new update of Sunny Bikes is deployed at least 1 pod is always available during upgrading
4. Add some annotations/labels that you deem useful

# Exercise 5
1. Make Postgres available from only within the cluster to the Sunny Bikes pod. 
1. SunnyBikes takes 2 environment variables to configure the postgres host and port, `PG_HOST` and `PG_PORT` respectively. 
> Hint: DNS of service will be in form <br>`<service name>.<namespace>.svc.cluster.local`
2. Make Sunny Bikes available on port 80 from outside world <br>
To test: Swagger docs are available on `http://<ip>:80/docs`

# Exercise 6
If the postgres pod dies,all the data is lost. 😭 <br>Make sure all the postgres data is persistent even throughout pod restarts

Hint: Add data via the Swagger interface<br>
Hint: Postgres stores its data in `/var/lib/postgresql/data`

Verify the volume is working.

