# Docker

* Docker is an open-source containerization platform / toll that allows developers to build, package, and run applications in lightweight, portable containers.

* Docker provides OS-level abstraction by isolating applications in containers while sharing the host OS kernel.

* Docker follows a client–server architecture that allows users to build, run, and manage containers efficiently.

### Docker Client

* This is what users interact with.

* Commands like docker build, docker pull, docker run are issued from the client.

* The client communicates with the Docker Daemon using REST APIs over:

  * Unix socket (Linux)

  * TCP socket

### Docker Daemon (dockerd)

* The core background service that runs on the host machine.

* Responsible for:

  * Building Docker images

  * Running and managing containers

  * Handling networks and volumes

  * Listens for requests from the Docker Client.

### Docker Images

* Read-only templates used to create containers.

* Built using a Dockerfile.

* Images consist of multiple layers for efficiency.

* Stored locally or in a registry.

### Docker Containers

* Running instances of Docker images.

* Lightweight and isolated using:

  * Linux namespaces

  * cgroups

* Share the host OS kernel.

### Docker Registry

* Stores and distributes Docker images.

* Examples:

  * Docker Hub (public)

  * Amazon ECR, Azure ACR, GCR (private)

* Used by Docker daemon to pull and push images.
