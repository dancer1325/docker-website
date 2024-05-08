# Use an existing base image
FROM python:3.8-slim

# Set the working directory inside the container
WORKDIR /app

# Add the Python script from a remote URL
ADD https://example.com/app.py /app/app.py

# Define the command to run when the container starts
CMD ["python", "app.py"]