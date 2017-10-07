# Docker

Starting with version 3.0.3.2, ASF is now also available as **[docker container](https://www.docker.com/what-container)**. Running ASF in docker container typically has no advantages for casual users, but it might be an excellent way of making use of ASF on servers, ensuring that ASF is being run in sandboxed environment separated from all other apps. Our docker repo can be found **[here](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Tags

ASF is available through 4 main types of **[tags](https://hub.docker.com/r/justarchi/archisteamfarm/tags)**:


### `master`

This tag always points to the ASF built from latest commit in master branch, which works the same as experimental AppVeyor build described in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. Typically you should avoid this tag, as it's the highest level of bugged software dedicated to developers and advanced users for development purposes. The image is being updated with each commit in the master GitHub branch, therefore you can expect very often updates, just like in our AppVeyor build.


### `released`

Very similar to the above, this tag always points to the latest **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** ASF version, including pre-releases. Compared to `master` tag, this image is being updated each time a new GitHub tag is pushed. Dedicated to advanced and power users that love to live on the edge of what can be considered stable and fresh at the same time.


### `latest`

This tag in comparison with previous two, as the first one includes ASF `AutoUpdates` feature and will typically point to the one of the stable versions, but not necessarily the latest one. The objective of this tag is to provide a sane default Docker container that is capable of running self-updating ASF. Because of that, the image doesn't have to be updated as often as possible, as included ASF version will always be capable of updating itself if needed. Of course, `AutoUpdates` feature can be safely turned off, but in this case you should probably use frozen `A.B.C.D` release instead.

### `A.B.C.D`

In comparison with above tags, this tag is completely frozen, which means that the image won't be updated once published. This works similar to our GitHub releases that are never touched after the initial release, which guarantees you stable and frozen environment. Typically you should use this tag when you want to use some specific ASF release and you don't want to use `AutoUpdates` that are offered in `latest` tag.

---

## Which tag is the best for me?

That depends what you're looking for. For majority of users, `latest` tag should be the best one as it offers exactly what desktop ASF does, just in special Docker container as a service. People that are rebuilding their images quite often and would instead prefer to have ASF version tied to given release are welcome to use `released` tag. If you instead want some specific frozen ASF version that will never change without your clear intention, `A.B.C.D` releases are available for you as fixed ASF milestones you can always fallback to.

We generally discourage trying `master` builds, just like automated AppVeyor builds - this build is here for us to mark current state of ASF project. Nothing guarantees that such state will work properly.

---

## Usage

For complete reference you should use **[official docker documentation](https://docs.docker.com/engine/reference/run/)**, we'll cover only basic `latest` tag usage in this guide, you're more than welcome to dig deeper.

### Hello ASF!

Firstly we should verify if our docker is even working correctly, this will serve as our ASF "hello world":

```
docker pull justarchi/archisteamfarm
docker run -it --name asf justarchi/archisteamfarm
```

`docker pull` command ensures that you're using up-to-date `justarchi/archisteamfarm` image, just in case you had outdated local copy in your cache. `docker run` creates a new ASF docker container for you and runs it in the foreground.

If everything ended successfully, after pulling all layers and starting container, you should notice that ASF properly started and informed us that there are no defined bots, which is good - we verified that ASF in docker works properly. Hit `CTRL+P` then `CTRL+Q` in order to quit foreground docker container, then stop ASF container with `docker stop asf`, and remove it with `docker rm asf`.


### Using a volume

If you're using ASF in docker container then obviously you need to configure the program itself. You can do it in various different ways, but the recommended one would be to create ASF `config` directory on local machine, then mount it as shared volume in ASF docker container.

For example, we'll assume that your ASF config folder is in `/home/archi/ASF/config` directory. This directory contains core `ASF.json` as well as bots that we want to run. Now all we need to do is simply attaching that directory as shared volume in our docker container, where ASF expects its config directory (`/app/config`).

```
docker pull justarchi/archisteamfarm
docker run -dit -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

And that's it, now your ASF docker container will use shared directory with your local machine in read-write mode, which is everything you need for configuring ASF. We also added `-d` option in order to start ASF container in detached mode.

Of course, this is just one specific way to achieve what we want, nothing is stopping you from e.g. creating your own `Dockerfile` that will copy your config files into `/app/config` directory inside ASF docker container.

---

## Pro tips

When you already have your ASF docker container ready, you don't have to use `docker run` every time. You can easily stop/start ASF docker container with `docker stop asf` and `docker start asf`. Keep in mind that if you're not using `latest` tag then updating ASF will still require from you to `docker stop`, `docker rm`, `docker pull` and `docker run` again. This is because you must rebuild your container from fresh ASF docker image every time you want to use ASF version included in latest image.

As hinted by above, ASF in tag other than `latest` won't automatically update itself, which means that **you** are in charge of using up-to-date `justarchi/archisteamfarm` repo. This has many advantages as typically the app should not touch its own code when being run, but we also understand convenience that comes from not having to worry about ASF version in your docker container.

You should typically run ASF in docker container with `Headless: true` global setting. This will clearly tell ASF that you're not here to provide missing details and it should not ask for those. Of course, for initial setup you should consider leaving that option at `false` so you can easily set up things, but in long-run you're typically not attached to ASF console, therefore it'd make sense to inform ASF about that.