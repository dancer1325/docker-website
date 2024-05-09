# Use an existing base image
FROM ubuntu:20.04

# Copy files from the host machine to the container
COPY . .

# ENV  -- Add environment variables --
# 1. Invalid to pass variables in another line
#ENV
#GIRL_NAME=rosi

ENV GIRL_NAME=rosi QUOTE_REMOVED="Quotes removed" SET_WHITESPACE=Alfred\ whitespace
#SET_WHITESPACE=Alfred \ whitespace         \ must be passed close to the previous word


# Define the command to run when the container starts
CMD ["sleep", "3600"]
