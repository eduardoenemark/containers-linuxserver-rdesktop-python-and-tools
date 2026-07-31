# LinuxServer Rdesktop with Python and Tools

A Docker container based on [linuxserver/rdesktop](https://hub.docker.com/r/linuxserver/rdesktop) with Ubuntu/Xfce desktop environment, enhanced with Python development, VS Code, and other tools.

## Description

This container extends the official linuxserver/rdesktop image with:

- Ubuntu XFCE desktop environment
- Python 3 and pip
- Visual Studio Code (with --no-sandbox flag enabled by default)
- Essential development tools (build-essential, git, curl, wget, nano, vim, file, etc.)
- SSH client tools (openssh-client, ssh-tools, sshfs)
- Archive utilities (zip, unzip, 7zip, gzip, bzip2, xz-utils)
- Document viewers (okular)
- Text editors (nano, vim, gedit)
- Network tools (net-tools)
- Compilers and build tools (gcc, g++)
- Other utilities (caja, less)

Based on: `docker.io/linuxserver/rdesktop:ubuntu-xfce-version-aaa0dc4e`

## Usage

### Docker Compose (Recommended)

```yaml
version: '3.8'
services:
  rdesktop:
    image: yourusername/linuxserver-rdesktop-xfce-python-and-tools:1.0
    container_name: linuxserver-rdesktop-xfce-python-and-tools
    security_opt:
      - seccomp:unconfined
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - ./home:/config
    ports:
      - 3350:3350
      - 3389:3389
    shm_size: "4gb"
    restart: unless-stopped
```

### Docker CLI

```bash
docker run -d \
  --name=linuxserver-rdesktop-xfce-python-and-tools \
  --security-opt seccomp=unconfined \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Etc/UTC \
  -p 3350:3350 \
  -p 3389:3389 \
  -v $PWD/home:/config \
  --shm-size="4gb" \
  --restart unless-stopped \
  yourusername/linuxserver-rdesktop-xfce-python-and-tools:1.0
```

## Parameters

Container images are configured using parameters passed at runtime (see below). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 3389:3389` would expose port `3389` from inside the container to be accessible from the host's IP on port `3389` outside the container.

| Parameter | Function |
|-----------|----------|
| `-p 3350:3350` | Alternative RDP port |
| `-p 3389:3389` | Standard RDP access port |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Etc/UTC` | specify a timezone to use, see [this list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List) |
| `-v /config` | abc users home directory where config files will be stored |
| `--shm-size=` | We set this to 4gb to prevent modern web browsers from crashing |
| `--security-opt seccomp=unconfined` | For Docker Engine only, many modern gui apps need this to function as syscalls are unknown to Docker |

### User / Group Identifiers

When using volumes (`-v` flags), permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id your_user` as below:

```bash
id your_user
```

Example output:

```shell
uid=1000(your_user) gid=1000(your_user) groups=1000(your_user)
```

## Default Credentials

The default username and password is: `abc/abc`

**IMPORTANT**: For security reasons, please change the default password after first login using the `passwd` command.

## Supported Features

### Open Source GPU Acceleration

For accelerated apps or games, render devices can be mounted into the container and leveraged by applications using:

```shell
--device /dev/dri:/dev/dri
```

## Tags

- `1.0` - Initial release with Python, VS Code, and essential development tools.

## Image Information

- **Base Image**: linuxserver/rdesktop:ubuntu-xfce-version-aaa0dc4e
- **Maintainer**: Eduardo Vieira (Telegram/X: @eduardoenemark)
- **Source Repository**: https://github.com/eduardoenemark/containers/tree/main/linuxserver-rdesktop-python-and-tools/xfce
- **License**: GPL-3.0-only

---

*Built with ❤️ using LinuxServer.io base images*
