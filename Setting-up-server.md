> TODO: Segment each section into seperate files in a folder, then summarize what files are the correct order for setting up a server in this fashion in one document.

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

7. Press ``Ctrl + 6`` again, then change up this code, paste it in and hit ``Ctrl + O``, ``Enter`` then ``Ctrl + X``
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

## Setting up app service
Create a service file
```
sudo nano /etc/systemd/system/YOURNAME.service
```

Change up this config and paste it
```yaml
[Unit]
Description=The official Pulse web app

[Service]
WorkingDirectory=/home/ubuntu/PulseWebsitePublish
ExecStart=/usr/bin/dotnet /home/ubuntu/PulseWebsitePublish/PulseWebsite.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Development
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Enable the service
```
sudo systemctl enable YOURNAME.service
```

And finally start it
```
sudo systemctl start YOURNAME.service
```

If you want to see a status on it, you can use this
```
sudo systemctl status YOURNAME.service

◝ YOURNAME.service - Example .NET Web API App running on Linux
    Loaded: loaded (/etc/systemd/system/YOURNAME.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/YOURNAME.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

NOTE: If you have changedd the service file, you have to run this first before stopping
```
sudo systemctl daemon-reload
```
Then
```
sudo systemctl stop pulse-webapp.service
```

## Linking domain using OVH
Go to your domain provider and add a new A record like this:
Record: A
Host: @
Value: IP of your VPS

Then wait for it to propagate (may take several hours).

Hooray! Its set up.
