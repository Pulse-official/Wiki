# Setting up Nginx

1. Now lets install nginx
```cmd
sudo apt update
sudo apt install nginx
```

2. Cd to ``/etc/nginx/sites-available``, then ``sudo nano default``

3. Press ``Ctrl + 6`` at the end of the file, then move to the start of the file and hit ``Alt + T``

4. Press ``Ctrl + 6`` again, then change up this code, paste it in and hit ``Ctrl + O``, ``Enter`` then ``Ctrl + X``
```nginx
server {
 
  listen 80 default_server; 
  server_name yourdomain.com *.yourdomain.com; 

  root /PATH/TO/WEBROOT;

  index  index.html index.htm;

  location / {
      try_files $uri /index.html;
  }

  location ~ /\.ht {
    deny  all;
  }
}
```

For a reverse proxy, do something like this:
```nginx
server {
    listen        80;
    server_name   yourdomain.com *.yourdomain.com;

    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```
