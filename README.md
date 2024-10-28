# Installing-GitLab-in-Docker-Compose

## GitLab Docker Setup

This repository provides a quick setup to deploy GitLab using Docker Compose. The configuration is based on GitLab Community Edition (CE) and exposes the necessary ports for HTTP, HTTPS, and SSH access.

## Prerequisites

- Docker
- Docker Compose

## Configuration

The `docker-compose.yml` file in this repository sets up a GitLab container with the following configuration:

- Exposed Ports:
  - `443` (HTTPS)
  - `80` (HTTP)
  - `22` (SSH)
- Persistent Volumes:
  - `gitlab_config`: Stores GitLab configuration files
  - `gitlab_logs`: Stores GitLab log files
  - `gitlab_data`: Stores GitLab application data

## Getting Started

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd <repository-folder>
```

### Step 2: Start GitLab

Run the following command to start the GitLab container in detached mode:

```bash
docker-compose up -d
```

### Step 3: Configure GitLab External URL

After starting the container, set the `external_url` by running:

```bash
docker exec -it gitlab bash -c "echo \"external_url 'http://localhost'\" >> /etc/gitlab/gitlab.rb && gitlab-ctl reconfigure"
```

> Replace `http://localhost` with your desired external URL if accessing GitLab from a remote location.

### Step 4: Retrieve Initial Root Password

To obtain the initial root password for GitLab:

```bash
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

This command outputs the root password, allowing you to log into GitLab at the configured `external_url`.

## Stopping GitLab

To stop the GitLab container:

```bash
docker-compose down
```

## Troubleshooting

For any configuration changes, update the `docker-compose.yml` or connect to the GitLab container directly using:

```bash
docker exec -it gitlab /bin/bash
```

After making changes, reconfigure GitLab with:

```bash
gitlab-ctl reconfigure
```

## Notes

- The `docker-compose.yml` is set to restart GitLab automatically (`restart: always`) to ensure high availability.
- Make sure to back up the `gitlab_data` volume if migrating or removing the setup.

