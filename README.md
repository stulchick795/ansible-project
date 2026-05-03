# Nginx + Docker: Automated Infrastructure Deployment

This project automates the deployment of Nginx as a reverse proxy
and a Hello World application running in Docker, using Ansible.

## Architecture
Browser → Nginx (port 80) → Docker container (port 8080)

## Prerequisites

- Ansible installed on your local machine
- Target server running Ubuntu
- SSH access to the target server
- Python 3 on the target server

## Project Structure
```
ansible-project/
├── inventory.ini    # List of target servers
├── playbook.yml     # Ansible playbook
└── README.md        # This file
```

## How to Use

1. Clone the repository:
```bash
git clone https://github.com/your-username/ansible-project
cd ansible-project
```

2. Edit inventory.ini with your server details:
```ini
[webservers]
myserver ansible_host=YOUR_SERVER_IP ansible_user=YOUR_USER
```

3. Run the playbook:
```bash
ansible-playbook -i inventory.ini playbook.yml
```

4. Open your browser and go to:
http://YOUR_SERVER_IP

## Logging

Nginx logs are stored on the target server:

- Access log: `/var/log/nginx/hello-world.access.log`
- Error log: `/var/log/nginx/hello-world.error.log`

To view access logs in real time:
```bash
tail -f /var/log/nginx/hello-world.access.log
```
To view error logs in real time:
```bash
tail -f /var/log/nginx/hello-world.error.log
