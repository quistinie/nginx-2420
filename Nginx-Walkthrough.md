# How to setup a basic website on an arch linux server

You'll need a digitalocean droplet for this guide.

## 1. Install Vim and Nginx

Connect to your server and use the command line to install Vim and Nginx.

```
sudo pacman -Sy vim nginx
```

## 2. Make the project directory

Make the directory that will hold our project files in the root folder.

```
sudo mkdir -p /web/html/nginx-2420
```

## 3. Make a config file

