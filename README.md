
# Project 5: Automated Configuration with Ansible (Provision Jenkins + Docker + Deploy App)

This repository contains the setup to deploy an Nginx web server running a Flask app in Docker. The app is containerized with Docker, and Nginx is used as a reverse proxy to serve the Flask application.

## Prerequisites

Before getting started, make sure you have the following installed:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Ansible](https://www.ansible.com/) (for automation)

### Operating System

This guide was tested on **Ubuntu 24.04**, but it should work on any system with Docker and Docker Compose installed.

## Project Structure

```
.
├── inventory.ini              # Ansible inventory file
├── playbook.yml               # Ansible playbook to configure the server
├── docker-compose.yml         # Docker Compose configuration for the Flask app and Nginx
├── nginx.conf                 # Nginx configuration for the Flask app
└── README.md                  # Project setup guide (this file)
```

## Setup Instructions

### 1. Clone the Repository

Start by cloning this repository to your server or local machine:

```bash
git clone <repository_url>
cd <repository_folder>
```

### 2. Configure Ansible Inventory

The `inventory.ini` file is used by Ansible to target the server that will be configured. Update the file with the appropriate IP address of the target machine. 

For example:
```
[web]
your-server-ip
```

### 3. Configure Nginx

Modify `nginx.conf` to fit your environment or application specifics. This config file tells Nginx how to serve the Flask app. By default, it assumes the app will be running on port `8081`.

### 4. Run the Ansible Playbook

Use Ansible to set up the environment on the server. The playbook will:

- Install Docker and Docker Compose
- Set up the Flask app in a Docker container
- Configure Nginx as a reverse proxy for the Flask app

Run the following command to apply the playbook:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

This will automate the installation and setup process.

### 5. Docker Setup

The Docker Compose file (`docker-compose.yml`) will define two services:

- **Flask app** (running on a Python-based image)
- **Nginx** (configured to reverse proxy to the Flask app)

Check the `docker-compose.yml` file to make sure the Flask app container is listening on the correct port (default: `8081`).

### 6. Test the Setup

Once the playbook runs successfully, check the status of your containers:

```bash
docker ps
```

You should see a container running the Flask app and another running Nginx.

You can now access the app by navigating to the IP address of your server on port `8081`:

```
http://your-server-ip:8081
```

Nginx should be serving your Flask app, and the web page should load successfully.

### 7. Troubleshooting

- If the containers keep restarting, check the logs of the container for errors:
  ```bash
  docker logs <container_id>
  ```
- If you run into a `502 Bad Gateway` error, verify that the Flask app container is running correctly and check the Nginx configuration for any typos.

### 8. Clean-up

To stop the services and remove the containers, run:

```bash
docker compose down
```

To remove unused containers and images:

```bash
docker system prune -a
```

## Notes

- The **Flask app** in this example is a simple Python app. You can replace it with your own Flask app code.
- The **Nginx configuration** is designed for reverse proxying to a single Flask container. Modify it if you have different routing requirements.

## Conclusion

This guide should help you set up a basic Nginx + Flask app environment with Docker and Ansible automation. Feel free to extend it by adding your own features, such as multi-container setups, databases, or SSL for Nginx.

