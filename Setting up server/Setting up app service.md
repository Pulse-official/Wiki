# Setting up app service

Create a service file
```
sudo nano /etc/systemd/system/YOURNAME.service
```

Change up this config and paste it
> NOTE - System.Net.Quic.QuicImplementationProviders was removed in .NET 7. If you get an exception about this not existing, dont roll forward.
```yaml
[Unit]
Description=DESCRIPTION

[Service]
WorkingDirectory=/home/ubuntu/FOLDER
ExecStart=/usr/bin/dotnet --roll-forward LatestMajor /home/ubuntu/FOLDER/APP.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=ubuntu
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
