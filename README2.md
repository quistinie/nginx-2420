# Part 2: Adding a Firewall and Reverse Proxy
## 1. Set up Firewall with UFW

### Install UFW
First, install UFW on the droplet.
```bash
sudo pacman -S ufw
```

### Enable the UFW service
It's important you only enable ufw.service right now. Use the command below to do so. Don't enable ufw yet.
```bash
sudo systemctl enable --now ufw.service
```

### Configure UFW
Set the defaults. This will deny all incoming traffic and allow all outgoing traffic.
```bash
sudo ufw default deny incoming
```
```bash
sudo ufw default allow outgoing
```

Allow ssh, this will allow traffic in port 22.
```bash
sudo ufw allow ssh
```
Also allow http because we are creating a website. This will allow traffic in port 80.
```bash
sudo ufw allow http
```

## 2. Enable UFW
Once we have allowed ssh, it is safe to enable UFW.
```bash
sudo ufw enable
```

## 3. Backend
Make a backend directory inside your `/web` directory.
```bash
sudo mkdir backend
```

Move the backend binary into this directory.
```bash
sudo mv /home/christine/hello-server /web/backend
```

### Make the service
```bash
cd /etc/systemd/system
```
```bash
sudo vim backend.service
```
Type this into
```plaintext
[Unit]
Description=Backend Service

[Service]
Type=simple
ExecStart=/web/backend/hello-server

[Install]
WantedBy=multi-user.target
```