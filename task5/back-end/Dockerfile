# Use Ubuntu as the base image
FROM ubuntu:latest

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package list and upgrade existing packages
RUN apt-get update && apt-get upgrade -y

# Install Python 3 and pip3
RUN apt-get install -y python3 python3-pip

# Fix the "externally managed environment" error
RUN rm -f /usr/lib/python*/EXTERNALLY-MANAGED

# Install Flask using pip3 (not via apt)
RUN pip3 install flask
RUN pip3 install flask-cors

# Set /app as the working directory inside the container
WORKDIR /app

# Copy the Python script into the image
COPY ./api.py /app/api.py

# Command to run the Flask server when the container starts
CMD ["python3", "api.py"]
