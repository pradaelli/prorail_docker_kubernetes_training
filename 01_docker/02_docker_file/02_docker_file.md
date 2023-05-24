# Dockerfile



## Docker build and layers

In the folder `simple_flask`, in the terminal run

```sh
docker build -t simple_flask .
```

Take note of the logs, particularly those that begin with `[n/4]` when `n` is one of the steps.

Now build it again and note how long it takes and the new logs.  What do you notice?

Now change the greeting "Hello World!" in the `app.py` file to "Hi World!"

Again run the build.  What do you notice in the logs?

Run `docker history simple_flask` and look at the layers created.

Run the container using

```sh
docker run --rm -p 8000:5000 simple_flask:latest
```

And access `http://localhost:8000` to see a greeting.

## WORKDIR

Let's make use of `WORKDIR`. Adjust the `Dockerfile` to the following:

```dockerfile
FROM python:3.10-alpine

RUN pip3 install flask

WORKDIR /code

COPY app.py .

ENTRYPOINT ["python3"]
CMD ["app.py"]
```

Rebuild your image, check the logs and then ensure that it runs.  How has this impacted the layers?

## Copy multiple files and layers

For a Flask app you'll typically have to install multiple python packages.  Create a file `requirements.txt` containing

```
Flask==2.2.2
```

Adjust the `Dockerfile` to the following:

```dockerfile
FROM python:3.10-alpine

WORKDIR /code

COPY . .

RUN pip3 install -r requirements.txt

ENTRYPOINT ["python3"]
CMD ["app.py"]
```

Rebuild your image and check the logs and then ensure that it runs.  How has this impacted the layers?

Now again make a change to the greeting in `app.py` and then rebuild the image.  What do you see in the logs? (hint: how many cached layers are used?)

Adjust to this

```dockerfile
FROM python:3.10-alpine

WORKDIR /code

COPY requirements.txt .
RUN pip3 install -r requirements.txt

COPY . .

ENTRYPOINT ["python3"]
CMD ["app.py"]
```

Rebuild the image, checking the logs.  Then again make changes to the greeting in `app.py` and again rebuild and check the logs.  What do you see in terms of cached layers?

## `.dockerignore`

Run the following command to access the files of your container

```
docker run --rm -it --entrypoint sh simple_flask
```

Run `ls` and notice that your `README.md` file has also been copied to the container. Exit the container.

Add the file `.dockerignore` to your files (so alongside your `Dockerfile`) containing:

```
README.md
```

Now rebuild your image and then rerun the `docker run ...` command above and then use `ls` to check your files.

## ENTRYPOINT & CMD

Open the folder `entry_point_and_command` in your terminal and build the `Dockerfile`:

```sh
docker build -t myping .
```

Run a container

```
docker run --rm myping
```


Stop the container and then run the following:

```
docker run --rm myping www.facebook.com
```

Notice how you can override the CMD

## Linting

In the folder `linting` run the following:

```sh
docker run --rm -i hadolint/hadolint < Baddockerfile
```

