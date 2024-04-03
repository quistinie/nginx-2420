# How to setup a basic website on an arch linux server

You'll need a digitalocean droplet for this guide.

## 1. Install Vim and Nginx

Connect to your server and use the command line to install Vim and Nginx.

```
sudo pacman -Sy vim nginx
```

Don't forget to start and enable nginx by using these two commands.

```
sudo systemctl start nginx
sudo systemctl enable nginx
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

## 4. Edit Nginx.conf to include sites-enabled

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

## 5. Create a symbolic link

Next create a symlink to enable your new website. This takes your website in the sites-available directory and links it to the sites-enabled directory to enable your site.

```
sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf
```

## 6. Add the demo html code

Use these commands to make the directories and the index file.

```
cd /
sudo mkdir web
cd web
sudo mkdir html
cd html
sudo mkdir nginx-2420
cd nginx-2420
sudo touch index.html
sudo vim index.html
```

Paste this html code into your `index.html` file

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        * {
            color: #db4b4b;
            background: #16161e;
        }
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>All your base are belong to us</h1>
</body>
</html>

```

Save and exit by pressing ESC and typing `:wq`

## 7. Restart Nginx

You must restart Nginx for the config file changes to be recognized.

```
sudo systemctl restart nginx
```

## 8. Check out your new website

You can see your new website by opening a web browser on your computer and typing your DigitalOcean Droplet IP address into the address bar.

![Screenshot](https://github.com/quistinie/nginx-2420/blob/main/Screenshot_1.jpg?raw=true)