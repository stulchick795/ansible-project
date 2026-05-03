# Nginx + Docker: Automated Web Server Deployment

## What is this?

This project automatically sets up a web server on any Linux machine
with a single command. It installs and configures everything needed
to run a "Hello World" web application:

- **Nginx** — the web server that handles incoming requests
- **Docker** — runs the Hello World application in an isolated container
- **Ansible** — automates the entire setup process

When everything is running, visiting your server's address in a browser
will show a Hello World page.

## Requirements

- A local machine running macOS or Ubuntu
- A server running Ubuntu (e.g. AWS, Hetzner, DigitalOcean)
- SSH access to that server
- Python 3 on the target server (usually pre-installed on Ubuntu)
- Internet connection

## How it works

```
Your Browser → Nginx (receives request) → Docker (serves the app) → Hello World page
```

## Prerequisites

### Opening the Terminal

- **macOS** — press `Cmd + Space`, type `Terminal`, press `Enter`
- **Ubuntu** — press `Ctrl + Alt + T`

### Installing Homebrew (macOS only)

Homebrew is a package manager for macOS. If you do not have it installed, open your Terminal and run:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Installing Git

```bash
# macOS
brew install git

# Ubuntu/Debian
sudo apt install git -y
```

### Installing Ansible

```bash
# macOS
brew install ansible

# Ubuntu/Debian
sudo apt install ansible -y
```

### Setting up SSH access to your server

SSH is how your local machine communicates securely with your server. To use Ansible, your machine must be able to connect to the server via SSH.

**Step 1 — Check if you already have an SSH key:**
```bash
ls ~/.ssh/
```
If you see `id_rsa` or `id_ed25519` — you already have a key and can skip to Step 2. If not — generate one:
```bash
ssh-keygen -t ed25519
```
Press `Enter` on all prompts to use the default settings.

**Step 2 — Add your public key to the server:**

Your SSH key has two parts:
- **Private key** (`id_ed25519`) — stays on your machine, never share it
- **Public key** (`id_ed25519.pub`) — needs to be added to your server

To copy your public key to the server:
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub YOUR_USER@YOUR_SERVER_IP
```

**If you are using AWS** — download the `.pem` key when creating your instance. AWS handles the key setup automatically. Skip Steps 1 and 2.

**If you are using Hetzner or DigitalOcean** — when creating your server, you will be asked to add an SSH key. Run the following command and paste the output into the SSH key field:
```bash
cat ~/.ssh/id_ed25519.pub
```

**Step 3 — Verify the connection:**
```bash
ssh YOUR_USER@YOUR_SERVER_IP
```
If you see a terminal prompt from your server — everything is set up correctly. Type `exit` to disconnect.

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
git clone https://github.com/stulchick795/ansible-project
cd ansible-project
```

2. Edit `inventory.ini` with your server details. Open the file:
```bash
nano inventory.ini
```

Replace the placeholders with your actual values:
```ini
[webservers]
myserver ansible_host=YOUR_SERVER_IP ansible_user=YOUR_USER ansible_ssh_private_key_file=~/.ssh/YOUR_KEY
```

For example, if your server IP is `127.0.0.1`, you are using Hetzner and your key is `id_ed25519`:
```ini
[webservers]
myserver ansible_host=127.0.0.1 ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_ed25519
```

Or if you are using AWS with a `.pem` key:
```ini
[webservers]
myserver ansible_host=127.0.0.1 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/yourkey.pem
```

Save and exit: press `Ctrl+X`, `Y`, then `Enter`.

3. Run the playbook:
```bash
ansible-playbook -i inventory.ini playbook.yml
```

Ansible will automatically install and configure everything on your server. This may take a few minutes.

4. Once complete, open your browser and go to:
```
http://YOUR_SERVER_IP
```

You should see the Hello World page. That's it — your server is up and running!

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
```

## Troubleshooting

| Problem | Solution |
|---|---|
| Cannot connect to server | Check your SSH key and server IP |
| Page not loading | Ensure ports 80 and 443 are open |
| 502 Bad Gateway | Run `docker start hello-world` on the server |
