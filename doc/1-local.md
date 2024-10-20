# Local Setup and Hosting

This section is about getting started with local hosting.

## Podman
We need a way to run containers locally.  We'll use [Podman](https://github.com/containers/podman/blob/main/docs/tutorials/podman-for-windows.md) to run our containers.

1. **Install podman**.  I used Windows, using the WSL2 install option.
2. **Verify installed**.  After installing podman, open a new command prompt, and type `podman`.  You should get information about command 
line options.

### First machine
Create first machine with  `podman machine init`

Start the machine with `podman machine start`
This starts the machine running.

### Podman cheat sheet

* `podman machine ls` - shows all the machines running
* `podman machine ssh` - ssh into the virtual machine
* `podman machine start`
* `podman machine stop`
* `podman machine rm`


### Build docker container
1. `cd sys\container`
2. `podman build .`

Got error:
```
Package dotnet-sdk-6.0 is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source
```

Let's see what's up with that.
* (Install .NET Runtime on Debian)[https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian]
    * This seems to indicate that dotnet-sdk-8.0 is available. Let's try that.


* `podman build . -t uoserver`
* `podman image ls`

