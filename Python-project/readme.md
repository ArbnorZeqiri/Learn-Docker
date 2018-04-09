# Test docker using a simple Python app

## Define a container with `Dockerfile`

`Dockerfile` defines what goes on in the environment inside your container.

### Create a `dockerfile`

Create an empty directory, enter in the new directory and create a file called `Dockerfile` with this content:

```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]

```
This `Dockerfile` refers to a couple of files we haven't created yet, namely `app.py` and `requirements.txt`. Let's create those next.

## Create python app files

Create two more files, `requirements.txt` and `app.py` in the same folder with the `Dockerfile`.

Now we see that `pip install -r requirements.txt` installs the Flask and Redis libraries for Python, and the app prints the environment vairable `NAME`, as well as the output of a call to `socket.gethostname()` .

Finally, because Redis isn't running (we have not install Redis), we should expect that the attempt to use it here fails and produces the error message.

## Build and run project

To build the app you should run the following command: `docker build -t myPythonProject .`

To run the project, you should run the following command: `docker run -p 4000:80 myPythonProject`.

Here, we have mapped the host port `4000` to container port `80`. So, when we want to access to this application from a browser or curl, we should navigate to `http://localhost:4000`.

Example:

```
$ curl http://localhost:4000

<h3>Hello World!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>

```

If you want to run the application in background, run this command: `docker run -d -p 4000:80 myPythonProject` .

To see the container list, `docker container ls` is used.

```

$ docker container ls

CONTAINER ID        IMAGE               COMMAND             CREATED
1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago

```

## Stopping the process

Use `docker container stop CONTAINER_ID` to stop the container.
For example: `docker container stop 1fa4ab2cf395` .