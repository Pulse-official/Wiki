# Setting up a server on an empty Ubuntu Linux installation
This documentation will go over how we can allow pages to display whilst allowing C# asp.net applications to run alongside it.

We will be using OVH as the provider.

## Connecting
1. We will use ``SSH`` on the Windows terminal to connect to the VPS.

```cmd
ssh username@ip
```

2. After connecting, cd to /home/ubuntu

3. Run this to set up .NET core on Linux
```shell
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
chmod +x ./dotnet-install.sh
./dotnet-install.sh --version latest
```

4. Now lets install nginx
```cmd
sudo apt update
sudo apt install nginx
```

5. Cd to ``/etc/nginx/sites-available``, then ``sudo nano default``

6. Press ``Ctrl + 6`` at the end of the file, then move to the start of the file and hit ``Alt + T``

7. Press ``Ctrl + 6`` again, then change up this code, paste it in and hit ``Ctrl + W``
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

8. Go to the asp.net project and add this to the ``appsettings.json`` file
```json
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://0.0.0.0:5000"
      }
    }
  },
```

7. Publish the project to a folder.

8. Open windows terminal and run this:
```cmd
scp -r PUBLISH_PATH USERNAME@IP:\home\ubuntu
```

9. Go back to the ssh instance then cd to /home/ubuntu/PUBLISH_NAME

10. Then run this:
```cmd
dotnet --roll-forward LatestMajor PUBLISH_NAME.dll
```

## Linking domain using OVH
Go to your domain provider and add a new A record like this:
Record: A
Host: @
Value: IP of your VPS

Then wait for it to propagate (may take several hours).

Hooray! Its set up.
