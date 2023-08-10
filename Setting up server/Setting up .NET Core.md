# Setting up .NET Core
[dotnet-install.sh documentation](https://learn.microsoft.com/en-us/dotnet/core/install/linux-scripted-manual#scripted-install)

1. Run this to set up .NET core on Linux
```shell
wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
chmod +x ./dotnet-install.sh
./dotnet-install.sh --version latest
```

2. Go to the asp.net project and add this to the ``appsettings.json`` file
```json
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://0.0.0.0:5000"
      }
    }
  },
```
