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

5. Go to the asp.net project and add this to the ``appsettings.json`` file
```json
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://0.0.0.0:5000"
      }
    }
  },
```

6. Publish the project to a folder.

7. Open windows terminal and run this:
```cmd
scp -r PUBLISH_PATH USERNAME@IP:\home\ubuntu
```

8. Go back to the ssh instance then cd to /home/ubuntu/PUBLISH_NAME

9. Then run this:
```cmd
dotnet --roll-forward LatestMajor PUBLISH_NAME.dll
```

Hooray! Its set up.
