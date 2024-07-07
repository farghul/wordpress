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
    - CONFIGS: Folder location of the yaml config files.
    - MARIADB:  `[repo]/[name]:[version]` of the MariaDB image.
    - WORDPRESS:  `[repo]/[name]:[version]` of the PHP image.
    - NGiNX:  `[repo]/[name]:[version]` of the NGiNX image.
    - DB_ROOT_PASSWORD: Mariadb Root password.
    - DB_PASSWORD: Mariadb Admin password.
    - DB_NAME: Database name.
    - DB_USER: Default database user.


3. An `.env` file with the following values:

```bash
DB_NAME=''
DB_USER=''
DB_PASSWORD=''
DB_ROOT_PASSWORD=''
DB_HOST=''
WP_ENV=''
WP_HOME='http://'
WP_SITEURL='http://wp'

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

```ansible
---
apple:
  hosts:
    darwin:
      ansible_connection: local
...
```

## Build

Individually, the images can be built, tagged, and pushed to a repo for easy access.

```bash
docker build -f Dockerfile -t [name] .
docker image tag [name]:latest [repo]/[name]:[version]
docker push [repo]/[name]:[version]
```

Or, using Ansible to automate the process:

```bash
ansible-playbook docker.yaml -i ~/inventory.yaml
```

## Run

Create the deployments:

```bash
ansible-playbook playbook.yaml -i ~/inventory.yaml
```

Check that the deployments are running:

```bash
minikube dashboard
```

Create the Roots/Bedrock project:

```bash
ansible-playbook bedrock.yaml -i ~/inventory.yaml
```

Navigate to the site:

```bash
minikube service nginx
```

You should be greeted by the WordPress install screen:

![Install](images/install.webp)

## License

Code is distributed under [The Unlicense](https://github.com/farghul/kuberpress/blob/main/LICENSE.md) and is part of the Public Domain.
