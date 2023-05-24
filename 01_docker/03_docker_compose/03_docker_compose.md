# Docker Compose

## Shifting from CLI to Docker Compose

Open `simple_flask` in your terminal.  It contains a `Dockerfile`.  Think about how you would build and run it using `docker build` and `docker run`

In the same folder, create a file 'docker-compose.yaml' containing the following:

```yaml
services:
  app:
    build: .
    container_name: my-app
    stop_signal: SIGINT
    ports:
      - 8000:5000
```

Now run the following:

```
docker compose up --build
```

Access `http://localhost:8000` in your browser

Stop running with `Ctrl+C`

Check for exited containers with `docker ps -a`

Now run

```sh
docker compose down
```

Again check for exited containers.

## About `--build`

Now in `app.py` change "Hello World!" to "Hi World!" and save.

Now run 

```sh
docker compose up # so without --build
```

Access http://localhost:8000.  What do you notice?

Now try again with `--build` and check the browser.

## Multiple containers

For this you will require two terminals.

Open `multiple_containers` in your first terminal

Run

```sh
docker compose up
```

And then go to `http://localhost:8000`.  Refresh the page and note what is displayed.

In your second terminal, run `docker ps` to see what is running.

Stop by using `Ctrl+C`

## Developing with Docker Compose (and mounts)

Still in `multiple_containers`, edit the `docker-compose.yaml` file and add the line:

```
		volumes:
      - .:/app
```

> `volumes:` should line up with `depends_on:`

Run

```
docker compose up --build
```

Go to http://localhost:8000 and observe the message being displayed.  

Now open `app.py` and adjust the "Hi" to "Hello" and save.

Return to http://localhost:8000 and refresh the page.  What you do see?

