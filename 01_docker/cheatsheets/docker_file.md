# Dockerfile Sample

```Dockerfile
# The base image
FROM python:3.10

# Defines an input with a default
ARG user1=someuser

# Add some metadata
LABEL "creator"="Some Dude"
LABEL "project"="Amazing Containers"

# Set working directory
WORKDIR /app

# Copy files from host to image
COPY . /app

# Execute shell statements
RUN pip install -r requirements.txt

# Let Docker know that the container listens on the given port
EXPOSE 8080

# The main process
ENTRYPOINT ["python"]

# Argument to pass to main process
CMD ["app.py"]
```