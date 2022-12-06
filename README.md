# WebTin++n

## Depends

[TinTin++](https://tintin.mudhalla.net)

[ttyd](https://github.com/tsl0922/ttyd)

## Usage

### Quickstart

Install Docker:

* (Debian/Ubuntu): `sudo apt install docker.io`
* (Arch/Manjaro): `sudo pacman -S docker`

Add yourself to the docker group (this is optional, but avoids the need for calling `sudo` a bunch of times):

```
sudo usermod -aG docker ${USER}
```

Logout to take effect. Then:

```
docker run -it -p 3000:3000 -v $(pwd):/data webtin
```

Point your web browser to _localhost:3000_ and it's done.

Tintin will read a _config.tin_ file script from the directory where the container was launched. This can be changed by substituting `$(pwd)` with the absolute path of your choice.

### Port forwarding

`-p <x>:<y>` means matching port \<x> on the host to port \<y> within the container. By construction, Webtin listens to port 3000, but you can change \<x> to whichever port you like, e.g. `-p 80:3000` for default http port.

### Networking from the cointainer perspective

When trying to connect to the Docker host from within the container, you have to look up its IP on the docker network. You can easily do this by running `ip a` on the host to find out the IP address of the `docker0` adapter. In this case __172.17.0.1:__
```
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500
    link/ether 02:42:e8:a9:95:58 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```
An alternative would be to use `--network host` in your `docker run` command. See [here](https://docs.docker.com/network/host) for more info.

### Important note on security

Add `#CONFIG {CHILD LOCK} ON` to _config.tin_ in order to prevent users from __getting command line access to the insides of the container__.

Although this is not a big concern, given that it still is _just_ a container, it definitely is conceptually wrong when serving mulptiple users, as they could theoretically kill any active session.

### Running it in the background

Launch the container in detached mode:

```
docker run -d -p 3000:3000 -v $(pwd):/data webtin
```

See `docker run --help` and `docker stop --help`.

### Restarting the container at boot

[Here](https://docs.docker.com/config/containers/start-containers-automatically) be info.

### Color handling

If there are any issues with colors the best thing to try is setting `#config color_patch on` in tintin.

### Resource constraints

Docker provides runtime options for limiting CPU and memory usage. [See here](https://docs.docker.com/config/containers/resource_constraints).

## About Docker

[Docker intro](https://docs.docker.com/get-started/overview/)

## Acknowledgements

Many thanks to Pulvazza for getting this figured out and written up.
