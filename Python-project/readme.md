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