# Attaching to an existing ASP.NET Core process running inside a Windows Docker container

## Quick Start

*~5 minutes, provided you have Visual Studio 2017 and Docker tools installed*

### 1 - Acquiring the .NET Core remote debugging tools

> As of December 2017 it looks like this is the only way to acquire the .NET Core version of the Remote Debugging tools. Using the full version of the tools found at `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Remote Debugger` doesn't work - Visual Studio hangs when trying to attach to the .NET Core process.

 - Ensure you have Visual Studio 2017 installed, along with Docker tools (Visual Studio Installer -> Modify -> Individual components -> Cloud, database, and server -> Container development tools)
 - Create a new ASP.NET Core web application, and select "Enable Docker Support"
 - Start the application through Visual Studio - this downloads the .NET Core remote debugging tools to `C:\Users\<USER>\onecoremsvsmon`
 
### 2 - Starting the application

 - Clone this repository
 - In the `docker-compose.yml` file, replace `<USER>` with your Windows username (and check the version number matches that of the .NET Core remote debugging tools downloaded in step 1)
 - Build the DockerDebug solution through Visual Studio
 - `docker-compose up --force-recreate --build`
 - Visit http://172.18.0.2/api/test/get

*Use the `-d` flag to run in detached mode, but be aware that this hides any startup errors*

### 3 - Debugging

 - Ensure Visual Studio has access through Windows Firewall (Windows Firewall -> Allow an app or feature through Windows Firewall -> scroll down to "Microsoft Visual Studio 2017")
 - Open VS2017 in Administrator mode
 - "Attach to Process" (`ctrl+alt+p`). Connection Type: Remote (no authentication), Connection Target: 172.18.0.2:4022 -> press enter
 - Ensure "Show processes from all users" is enabled
 - Find dotnet.exe and attach - when calling `/api/test/get`, a breakpoint in `TestController.Get` should now be hit 
	
## Useful commands

```
// view running containers including names/ids
docker container ls

// get IP address of container
docker inspect <container>

// attach to container via command line
docker exec -ti <container> powershell

// check running processes with powershell (should be running both msvsmon and dotnet)
Get-Process
```
