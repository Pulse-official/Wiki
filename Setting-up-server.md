# Setting up a server on an empty Ubuntu Linux installation
This documentation will go over how we can allow pages to display whilst allowing C# asp.net applications to run alongside it.

We will be using OVH as the provider.

## Connecting
We will use ``SSH`` on the Windows terminal to connect to the VPS.

```cmd
ssh username@ip
```
Then enter the password when prompted.

## Setting up

Follow these files in chronological order:
1. [Setting up Nginx](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Setting%20up%20Nginx.md)
2. [Setting up .NET Core](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Setting%20up%20.NET%20Core.md)
3. [Pointing the domain](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Pointing%20the%20domain.md)
4. [Setting up cloudflare](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Setting%20up%20cloudflare.md)
5. [Uploading from computer to server](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Uploading%20from%20computer%20to%20server.md)
6. [Setting up app service](https://github.com/Pulse-official/Wiki/blob/main/Setting%20up%20server/Setting%20up%20app%20service.md)
