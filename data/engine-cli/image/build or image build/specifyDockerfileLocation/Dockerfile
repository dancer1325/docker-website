# Use an existing base image
FROM ubuntu:20.04

# Set the working directory inside the container
WORKDIR /app

# Copy files from the host machine to the container
COPY . .

# Install dependencies
RUN apt-get update && \
    apt-get install -y \
    python3 \
    python3-pip

# Install Python dependencies
RUN pip3 install flask

# Expose a port for communication
EXPOSE 5000

# Define the command to run when the container starts
CMD ["python3", "app.py"]
