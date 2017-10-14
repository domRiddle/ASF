# Docker

Starting with version 3.0.3.2, ASF is now also available as **[docker container](https://www.docker.com/what-container)**. Running ASF in docker container typically has no advantages for casual users, but it might be an excellent way of making use of ASF on servers, ensuring that ASF is being run in sandboxed environment separated from all other apps. Our docker repo can be found **[here](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Tags

ASF is available through 4 main types of **[tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `master`

This tag always points to the ASF built from latest commit in master branch, which works the same as experimental AppVeyor build described in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. Typically you should avoid this tag, as it's the highest level of bugged software dedicated to developers and advanced users for development purposes. The image is being updated with each commit in the master GitHub branch, therefore you can expect very often updates (and stuff being broken), just like in our AppVeyor build.


### `released`

Very similar to the above, this tag always points to the latest **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** ASF version, including pre-releases. Compared to `master` tag, this image is being updated each time a new GitHub tag is pushed. Dedicated to advanced and power users that love to live on the edge of what can be considered stable and fresh at the same time.


### `latest`

This tag in comparison with previous two, as the first one includes ASF `AutoUpdates` feature and will typically point to the one of the stable versions, but not necessarily the latest one. The objective of this tag is to provide a sane default Docker container that is capable of running self-updating ASF. Because of that, the image doesn't have to be updated as often as possible, as included ASF version will always be capable of updating itself if needed. Of course, `AutoUpdates` feature can be safely turned off, but in this case you should probably use frozen `A.B.C.D` release instead.

### `A.B.C.D`

In comparison with above tags, this tag is completely frozen, which means that the image won't be updated once published. This works similar to our GitHub releases that are never touched after the initial release, which guarantees you stable and frozen environment. Typically you should use this tag when you want to use some specific ASF release and you don't want to use `AutoUpdates` that are offered in `latest` tag.

---

## Which tag is the best for me?

That depends on what you're looking for. For majority of users, `latest` tag should be the best one as it offers exactly what desktop ASF does, just in special Docker container as a service. People that are rebuilding their images quite often and would instead prefer to have ASF version tied to given release are welcome to use `released` tag. If you instead want to use some specific frozen ASF version that will never change without your clear intention, `A.B.C.D` releases are available for you as fixed ASF milestones you can always fallback to.

We generally discourage trying `master` builds, just like automated AppVeyor builds - this build is here for us to mark current state of ASF project. Nothing guarantees that such state will work properly.

---

## Architectures

ASF docker image is currently available for 2 architectures - `x64` and `arm`. You can read more about them in **[compatibility](https://github.com/JustArchi/ArchiSteamFarm/wiki/Compatibility)** section.

Since multi-arch docker tags are still work-in-progress, builds for other architectures than default `x64` are currently available with `-{ARCH}` appended to the tag name. In other words, if you want to use `latest` tag for `arm` architecture, simply use `latest-arm`.

---

## Usage

For complete reference you should use **[official docker documentation](https://docs.docker.com/engine/reference/commandline/docker)**, we'll cover only basic usage in this guide, you're more than welcome to dig deeper.

### Hello ASF!

Firstly we should verify if our docker is even working correctly, this will serve as our ASF "hello world":

```
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

`docker pull` command ensures that you're using up-to-date `justarchi/archisteamfarm` image, just in case you had outdated local copy in your cache. `docker run` creates a new ASF docker container for you and runs it in the foreground.

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`, and remove it with `docker rm asf`.

If you take a closer look at the command then you'll notice that we didn't declare any tag, which automatically defaulted to `latest` one. If you want to use other tag than `latest`, for example `latest-arm`, then you should declare it explicitly:

```
docker pull justarchi/archisteamfarm:latest-arm
docker run -it --name asf justarchi/archisteamfarm:latest-arm
```

---

## Using a volume

If you're using ASF in docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as a shared volume in ASF docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply attaching that directory as shared volume in our docker container, where ASF expects its config directory (`/app/config`).

```
docker pull justarchi/archisteamfarm
docker run -it -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF docker container. We're only covering basic usage in this guide.

---

## Command-line arguments

ASF allows you to pass **[command-line arguments](https://github.com/JustArchi/ArchiSteamFarm/wiki/Command-line-arguments)** in docker container by using `ASF_ARGS` environment variable. This can be added on top of `docker run` with `-e` switch. For example:

```
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_ARGS=--server" --name asf justarchi/archisteamfarm
```

Of course, if you're advanced docker user you can always modify default `ENTRYPOINT` of ASF container to run `ENTRYPOINT ["dotnet", "ArchiSteamFarm.dll", "--server"]` or whatever you need, although if you just want to run ASF and not create your own build based on ASF image then `ASF_ARGS` should be enough.

---

## IPC

For using IPC, firstly you should configure ASF to launch it properly, which would be starting it with `ASF_ARGS=--server` explained above, as well as setting `IPCHost` **[global configuration property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)** to something like `*`.

Once we achieve that and ASF properly brings up IPC interface, we need to tell docker to map ASF `1242/tcp` port either with `-P` or `-p` switch.

For example, this command would expose ASF IPC interface to host machine (only):

```
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_ARGS=--server" -p localhost:1242:1242 --name asf justarchi/archisteamfarm
```

Assuming you set `IPCHost` properly to something like `*`, the above command will make **[IPC client examples](https://github.com/JustArchi/ArchiSteamFarm/wiki/IPC#client)** work from the host machine.

---

## Pro tips

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Keep in mind that if you're not using `latest` tag then updating ASF will still require from you to `docker stop`, `docker rm`, `docker pull` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use ASF version included in that image. In `latest` tag, ASF has included capability to auto-update itself, so rebuilding the image is not necessary for using up-to-date ASF (but it might still be a good idea to do it from time to time in order to use fresh .NET Core runtime and underlying OS).

As hinted by above, ASF in tag other than `latest` won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. This has many advantages as typically the app should not touch its own code when being run, but we also understand convenience that comes from not having to worry about ASF version in your docker container. If you care about good practices and proper docker usage, `released` tag is what we'd suggest instead of `latest`, but if you can't be bothered with it and you just want to make ASF both work and auto-update itself, `latest` will do.

You should typically run ASF in docker container with `Headless: true` global setting. This will clearly tell ASF that you're not here to provide missing details and it should not ask for those. Of course, for initial setup you should consider leaving that option at `false` so you can easily set up things, but in long-run you're typically not attached to ASF console, therefore it'd make sense to inform ASF about that and use `!input` command if need arises.