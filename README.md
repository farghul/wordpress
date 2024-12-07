# Kubernetes WordPress Stack

Create a Kubernetes driven WordPress Stack with NGiNX, MariaDB, and WordPress ( including Composer and WP-CLI ) using Ansible and Docker.

![Stack](images/stack.webp)
 
## Prerequisites

1. Applications:

  - `Docker Hub` or `Podman Desktop`.
  - Docker CLI ( or Podman CLI ).
  - `Visual Studio Code`or equivilent code editor.
  - Access to `docker.io`, `ghcr.io`, or `quay.io` ( RedHat ) to store images.
  - `Minikube` and kubernetes-cli.

2. Variables (defaults/main.yaml):

  - HOME: Root project folder.
  - DOCKER: Folder location of the Docker image files.
  - MANIFESTS: Folder location of the YAML config files.
  - MARIADB: `[repo]/[name]:[version]` of the MariaDB image.
  - NGiNX: `[repo]/[name]:[version]` of the NGiNX image.
  - PHP: `[repo]/[name]:[version]` of the PHP image.
  - DB_ROOT_PASSWORD: Mariadb Root password.
  - DB_PASSWORD: Mariadb Admin password.
  - DB_NAME: Database name.
  - DB_USER: Default database user.
  - EMAIL: WordPress Admin email.
  - TITLE: WordPress site title.
  - WP_USER: WordPress Admin user.

3. An `.env` file with the following values:

``` Dotenv
DB_NAME=''
DB_USER=''
DB_PASSWORD=''
DB_ROOT_PASSWORD=''
DB_HOST=''
WP_ENV=''
WP_HOME='http://<site_url>'
WP_SITEURL='http://<site_url>/wp'

# Generate your keys here: https://roots.io/salts.html
AUTH_KEY=''
SECURE_AUTH_KEY=''
LOGGED_IN_KEY=''
NONCE_KEY=''
AUTH_SALT=''
SECURE_AUTH_SALT=''
LOGGED_IN_SALT=''
NONCE_SALT=''
```

4. An `inventory.yaml` file for Ansible to reference the hosts value.

``` yaml
---
apple:
  hosts:
    darwin:
      ansible_connection: local
...
```

## Build

Individually, the images can be built, tagged, and pushed to a repo for easy access.

``` zsh
docker build -f Dockerfile -t [name] .

docker image tag [name]:latest [repo]/[name]:[version]

docker push [repo]/[name]:[version]
```

Or, using Ansible to automate the process:

``` zsh
ansible-playbook docker.yaml -i ~/inventory.yaml
```

## Run

1. Start the Minikube environment:

``` zsh
> minikube start                                                                                 
😄  minikube v1.33.1 on Darwin 14.5
    ▪ MINIKUBE_IN_STYLE=1
    ▪ MINIKUBE_ROOTLESS=true
✨  Using the hyperkit driver based on user configuration
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🔥  Creating hyperkit VM (CPUs=2, Memory=4000MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.0.2 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

2. Create the Kubernetes deployments:

``` zsh
ansible-playbook playbook.yaml -i ~/inventory.yaml
```

3. Monitor the deployments using the Minikube Dashboard:

``` zsh
minikube dashboard
```

4. Create the Roots/Bedrock project:

``` zsh
ansible-playbook bedrock.yaml -i ~/inventory.yaml
```

5. Navigate to the locally hosted site:

``` zsh
minikube service nginx
```

You should be greeted by the WordPress install screen:

![Install](images/install.webp)

## Delete

To remove all deployments:

``` zsh
ansible-playbook delete.yaml -i ~/inventory.yaml
```

## License

Code is distributed under [The Unlicense](https://github.com/farghul/kuberpress/blob/main/LICENSE.md) and is part of the Public Domain.
