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

The following commands will set up the directories and the html file that we will show on our website. If you are not the root user it is important to do these steps in this order to avoid errors.

```
cd /etc/nginx
sudo mkdir sites-available
sudo mkdir sites-enabled
cd sites-available
sudo touch nginx-2420.conf
sudo vim nginx-2420.conf
```

Add the following configuration to the file. You will need to press I to enter insert mode in Vim.

```
# /etc/nginx/sites-available/nginx-2420.conf

server {
    listen 80;
    server_name your_ip;

    root /web/html/nginx-2420;

    location / {
        index index.html;
    }
}

```

Make sure you replace `your_ip` with your DigitalOcean droplet IP address.

Save and exit by pressing ESC and typing `:wq`

Go to nginx directory

```
cd /etc/nginx/
```

Edit the `nginx.conf` file to include the directory we made

```
sudo vim nginx.conf
```

Add this to the end of the http block. (one line above the last curly brace, make sure it is indented)

```
include sites-enabled/*;
```

Then create a symlink to enable your new website. This takes your website in the sites-available directory and links it to the sites-enabled directory to enable your site.

```
sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf
```

## 4. Add the demo html code

