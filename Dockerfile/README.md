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

