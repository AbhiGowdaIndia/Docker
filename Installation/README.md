# Docker Installation

## Docker Installation on Ubuntu

### Update the package index

```cmd
sudo apt update
``` 
### Install required packages

```cmd
sudo apt install -y ca-certificates curl gnupg
```
  * ca-certificates

    - Provides trusted Certificate Authority (CA) certificates.

    - Docker's repository uses HTTPS, so Ubuntu must trust Docker's SSL certificate.
   
  * curl

    - A command-line utility to download or upload data.

    - We'll use it to download Docker's GPG key.
   
  * gnupg

    - GNU Privacy Guard.

    - Used to verify software authenticity through GPG keys.

### Create Docker key directory

```cmd
sudo install -m 0755 -d /etc/apt/keyrings
```

### Download Docker's GPG key

```cmd
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### Allow everyone to read the key

```cmd
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Add Docker repository

```cmd
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Refresh package index

```cmd
sudo apt update
```

### Install Docker

```cmd
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Enable Docker at boot

```cmd
sudo systemctl enable docker
```

### Start Docker

```cmd
sudo systemctl start docker
```

### Verify installation

```cmd
docker --version
```

## RHEL

### Remove old Docker packages

```cmd
sudo dnf remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
```

### Install DNF plugin

```cmd
sudo dnf install -y dnf-plugins-core
```

### Add the Docker repository

```cmd
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```

### Install Docker

```cmd
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Enable and start Docker

```cmd
sudo systemctl enable docker
sudo systemctl start docker
```

### Verify the installation

```cmd
docker --version
sudo docker run hello-world
```
