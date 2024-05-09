# Use an existing base image
FROM ubuntu:20.04

# Copy files from the host machine to the container
COPY . .
#---

# ARG   -- Add variables
# 1. Pass default value
ARG argument=argument1
# 2. If you code ARG AFTER using it -> they are NOT evaluate it
#---



# ENV  -- Add environment variables --
# 1. Invalid to pass variables in another line
#ENV
#GIRL_NAME=rosi

ENV GIRL_NAME=rosi QUOTE_REMOVED="Quotes removed" SET_WHITESPACE=Alfred\ whitespace ARGUMENT=${argument}
ENV ARGUMENT_NOT_EVALUATED=${argumentnotevaluated}
#SET_WHITESPACE=Alfred \ whitespace         \ must be passed close to the previous word
# ---


# ARG, point 2
ARG argumentnotevaluated=argumentnoteval




# Define the command to run when the container starts
CMD ["sleep", "3600"]
