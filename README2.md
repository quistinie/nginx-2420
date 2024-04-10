# Part 2: Adding a Firewall and Reverse Proxy
## 1. Set up Firewall with UFW

### Install UFW
First, install UFW on the droplet.
```bash
sudo pacman -S ufw
```

### Enable UFW
Allow SSH, this is very important. Only enable UFW once SSH is allowed. If you do this in the wrong order you may be unable to access your droplet.
```bash
sudo ufw allow OpenSSH
sudo ufw --force enable
```