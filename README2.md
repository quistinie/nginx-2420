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
Type this into your `backend.service` file.
```plaintext
[Unit]
Description=Backend Service

[Service]
Type=simple
ExecStart=/web/backend/hello-server

[Install]
WantedBy=multi-user.target
```
Use `:wq` to save and exit vim.

### Enable and Start Service
Reload the daemon, then enable and start the backend service:
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl enable backend
```
```bash
sudo systemctl start backend
```

## 4. Configure Nginx Reverse Proxy
Open the config file we created in the first tutorial.
```bash
sudo vim /etc/nginx/sites-available/nginx-2420.conf
```
Add the `/hey` and `/echo` location blocks to the server block.
```bash
# /etc/nginx/sites-available/nginx-2420.conf

server {
    listen 80;
    server_name your_ip;

    root /web/html/nginx-2420;

    location / {
        index index.html;
    }

    location /hey {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /echo {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```
Again, ensure `your_ip` is the IP of your Digital Ocean droplet. We did this in part 1 so it should already be there.