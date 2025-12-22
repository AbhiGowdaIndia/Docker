# Dockerfile Explanation

* A Dockerfile is a text file with instructions that tells Docker how to build an image.

* Each instaruction in dockerfile creates a layer in the image.

* The **correct and standard name of a Dockerfile** is:

  **Dockerfile**

* You can use a different name, but you must specify it during build.

  **docker build -t \<image-name> -f <dockerfile-name>**

### Example Dockerfile

```dockerfile
# =========================
# Build-time argument
# =========================
ARG BASE_IMAGE=ubuntu:22.04

# =========================
# Base image
# =========================
FROM ${BASE_IMAGE}

# =========================
# Maintainer (DEPRECATED but included)
# =========================
MAINTAINER AbhishekaB <abhisheka@example.com>

# =========================
# Metadata (modern replacement of MAINTAINER)
# =========================
LABEL app="sample-dockerfile"
LABEL environment="demo"

# =========================
# Set environment variables
# =========================
ENV APP_ENV=production
ENV APP_PORT=8080

# =========================
# Set working directory
# =========================
WORKDIR /app

# =========================
# Add files (supports auto-extract)
# =========================
ADD sample.tar.gz /app/

# =========================
# Copy files from local system
# =========================
COPY app.sh /app/app.sh
COPY config/ /app/config/

# =========================
# Run commands during image build
# =========================
RUN apt-get update && \
    apt-get install -y curl && \
    chmod +x /app/app.sh && \
    useradd -m appuser

# =========================
# Switch to non-root user
# =========================
USER appuser

# =========================
# Expose application port
# =========================
EXPOSE 8080

# =========================
# Declare volume for persistent data
# =========================
VOLUME ["/app/data"]

# =========================
# Entry point (fixed command)
# =========================
ENTRYPOINT ["/app/app.sh"]

# =========================
# Default command (arguments to ENTRYPOINT)
# =========================
CMD ["--start"]
```

### Common Dockerfile Instructions

#### ARG

  * Build-time Variable

  * Defines a variable used only during image build

  * Can be overridden at build time:

    **docker build --build-arg BASE_IMAGE=ubuntu:20.04 .**

#### FROM

  * Specifies the base image

  * Mandatory (except for scratch images)

  * Example:

  ```dockerfile
  FROM ubuntu:22.04
  ```
#### MAINTAINER

  * MAINTAINER wass used to specify the image author, but it is deprecated and replaced by the LABEL instruction.

  Example:

  ```dockerfile
  MAINTAINER Abhisheka B <abhisheka@example.com>
  ```
#### LABEL

  * Image Metadata

  * Adds metadata to the image

  * Replacement for deprecated MAINTAINER

  * Useful for automation, documentation, filtering images

  * Example:

  ```dockerfile
  LABEL maintainer="AbhishekaB <abhisheka@example.com>"
  LABEL app="sample-dockerfile"
  LABEL environment="demo"
  ```

  * To check Label

  ```cmd
  docker inspect image_name
  ```

#### ENV

* Sets environment variables available at build and runtime.

* Example:

  ```dockerfile
  ENV APP_ENV=production
  ENV APP_PORT=8080
  ```

#### WORKDIR

* Sets working directory inside container.

* Example:

  ```dockerfile
  WORKDIR /app
  ```

#### ADD

* Copy files or directories into a Docker image, with a few extra capabilities beyond COPY.

* Automatically extract compressed archives

* Download files from a URL

* Example:

  ```dockerfile
  ADD app.py /usr/src/app/
  ADD sample.tar.gz /app/
  ADD https://example.com/config.json /etc/config.json
  ```

#### COPY

* Copy files or directories from your local system (build context) into a Docker image.

* Example:
 
  ```dockerfile
  # copy file
  COPY app.sh /app/app.sh
  # copy directory
  COPY src/ /app/src/
  # copy multiple files
  COPY file1.txt file2.txt /data/
  # copy with wildcards
  COPY *.sh /scripts/
  ```

#### RUN

* To execute commands during the image build process.

* It creates a new image layer with the results of those commands.

* Example:

  ```dockerfile
  RUN apt-get update && \
    apt-get install -y curl && \
    chmod +x /app/app.sh && \
    useradd -m appuser
  # Exec (JSON) form
  RUN ["apt-get", "install", "-y", "nginx"]
  ```

#### USER

* Specifies which user runs commands in the container.

* Example:

  ```dockerfile
  USER appuser
  ```

#### EXPOSE

* The EXPOSE instruction is used to document which network port(s) a container listens on at runtime.

* Example:

  ```dockerfile
  # expose single port
  EXPOSE 8080
  # expose multiple ports
  EXPORT 8080 443
  # can also specify protocol
  EXPOSE 8080/tcp
  EXPOSE 53/udp
  ```

#### VOLUME

* To declare a mount point inside the container for persistent or shared data.

* Data stored in a volume is not lost when the container is deleted.

* Example:
 
  ```dockerfile
  VOLUME ["/app/data"]
  # JSON form:
  VOLUME ["/app", "/data"]
  ```

#### ENTRYPOINT

* Defines the main command that always runs when a container starts.

* It makes the container behave like a standalone executable.

* Example:

  ```dockerfile
  ENTRYPOINT ["/app/app.sh"]
  ```

#### CMD

* Defines default command to run when container starts.

* Example:

  ```dockerfile
  CMD ["--start"]
  # Example
  CMD ["nginx", "-g", "daemon off;"]
  ```



