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


### Resources
* [Volume mounting](https://github.com/containers/podman/blob/main/docs/tutorials/podman-for-windows.md)
* [Using volumes 2](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md)

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

* `podman start --interactive --attach uoserver`
* ``

Let's log into the container to look around.
```
podman run -it uoserver bash
```
We can see that `entrypoint.sh` is going to go into the `/UO` folder and issue a `dotnet` command.  Let's verify that we have a UO folder, and verify that the `dotnet` command is available.
```
cd /
ls
```
We can see that we have a UO folder, and a UOData folder.  If we do `ls UO` and `ls UOData` we can see nothing is in there.  This is as expected, because those are remote mounted volumes, hosted on our main machine and passed into the container.  We haven't set those up yet so this is to be expected.
```
dotnet
```
When we execute that command we see usage information, so this isready to be used.

## External Volumes: /UO and /UOData directories
Let's download ServUO and set up our program directory.

Go to [ServUO releases](https://github.com/ServUO/ServUO/releases) and go into the latest there.  Copy that into directory `src/ServUO`.

Now that we have the code there, let's mount it.

The format of the podman `--volume` argument is `(local_dir):(container_dir)`.

```bash
# Run this from the top directory of this project.
podman run --interactive --tty --volume WorldData:/UOData --volume src/ServUO:/UO uoserver bash
```

When I go in there and poke around, I can see that the `/UO` directory is mounted there as I expect.  I also created a file from inside the container at /UOData and verified it appeared on my local machine at /WorldData .


## Build
Now let's see if we can get the build to work.

Use the `podman run` command from above, and do `dotnet build -c Release`

We see the error
```
/usr/lib/dotnet/sdk/8.0.110/Microsoft.Common.CurrentVersion.targets(1241,5): error MSB3644: The reference assemblies for .NETFramework,Version=v4.8 were not found. To resolve this, install the Developer Pack (SDK/Targeting Pack) for this framework version or retarget your application. You can download .NET Framework Developer Packs at https://aka.ms/msbuild/developerpacks [/UO/Ultima/Ultima.csproj]
```
