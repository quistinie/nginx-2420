# How to Setup a Basic Website on an Arch Linux Server

You'll need a DigitalOcean Droplet for this guide.

## 1. Install Vim and Nginx

Connect to your server and use the command line to install Vim and Nginx.

```bash
sudo pacman -Sy vim nginx
```

Don't forget to start and enable the nginx service by using these two commands.

```bash
sudo systemctl start nginx
```
```bash
sudo systemctl enable nginx
```

## 2. Make the Project Directory

Make the directory that will hold our project files in the root folder.

```bash
sudo mkdir -p /web/html/nginx-2420
```

## 3. Make a Config File

The following commands will set up the directories and the html file that we will show on our website. If you are not the root user it is important to do these steps in this order to avoid errors.

Go to `/etc/nginx` directory

```bash
cd /etc/nginx
```

Make `sites-available` directory

```bash
sudo mkdir sites-available
```

Make `sites-enabled` directory

```bash
sudo mkdir sites-enabled
```

Go into `sites-available` directory

```bash
cd sites-available
```

Create a new config file

```bash
sudo touch nginx-2420.conf
```

Edit the config file

```bash
sudo vim nginx-2420.conf
```

Add the following configuration to the file. You will need to press I to enter insert mode in Vim.

```nginx
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

## 4. Edit the Nginx Config File to Include `sites-enabled`

Go to nginx directory

```bash
cd /etc/nginx/
```

Edit the `nginx.conf` file to include the directory we made

```bash
sudo vim nginx.conf
```

Add this to the end of the http block. (one line above the last curly brace, make sure it is indented)

```nginx
include sites-enabled/*;
```

## 5. Create a Symbolic Link

Next create a symlink to enable your new website. This takes your website in the sites-available directory and links it to the sites-enabled directory to enable your site.

```bash
sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf
```

## 6. Add the Demo HTML Code

Use these commands to make the directories and the index file.

Go to root directory

```bash
cd /
```

Make `web` directory

```bash
sudo mkdir web
```

Go into the directory you made

```bash
cd web
```

Make another new directory called `html`

```bash
sudo mkdir html
```

Go into the `html` directory

```bash
cd html
```

Make another new directory called `nginx-2420`

```bash
sudo mkdir nginx-2420
```

Go into the `nginx-2420` directory

```bash
cd nginx-2420
```

If you type `pwd` right now you should see `/web/html/nginx-2420` in the terminal
Now create your `index.html` file

```bash
sudo touch index.html
```

Edit the HTML file

```bash
sudo vim index.html
```

Paste this HTML code into your `index.html` file:

```html
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

```bash
sudo systemctl restart nginx
```

## 8. Check Out Your New Website

You can see your new website by opening a web browser on your computer and typing your DigitalOcean Droplet IP address into the address bar.

![Screenshot](https://github.com/quistinie/nginx-2420/blob/main/Screenshot_1.jpg?raw=true)