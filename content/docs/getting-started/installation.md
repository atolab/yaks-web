---
title: "Installation"
weight : 1010
menu:
  docs:
    parent: getting_started
---

To get up and running with <b>Yaks</b> you will have to install the infrastructure and then get hold of the API you would like to use to write your applications. 

## Installing Yaks's Infrastructure
The Yaks service is a plugin of <b>[zenoh](https://zenoh.io)</b>. Therefore, when installing Yaks, zenoh is automatically installed, if not already present on your host.

At the present stage Yaks and zenoh's infrastructures are supported only on Linux and MacOS. Below are the detailed information on how to install on supported platforms.

### MacOS
The first step is to tap our brew package repository:

```bash
$ brew tap atolab/homebrew-atobin
```    

Then simply install zenoh as follows:

```bash
$ brew install yaks
```

### Linux
The Linux installation procedure depends on the package manager supported by your distribution. Below are detailed information for <b>apt</b> and <b>yum</b> based distros.

#### Debian and Friends
As a first step add our package repository to your package manager configuration by running the following command:

```bash
$ echo "deb [trusted=yes] http://pkgs.adlink-labs.tech/debian ./" | sudo tee -a /etc/apt/sources.list > /dev/null
```

Then update the packages list by:

```bash
$ sudo apt update
```

Now you can install Yaks:

```bash
$ sudo apt install yaks
```

#### RedHat and Friends
The first step is to add the ATOLab repository to <b>yum</b>, this can be done by editing the file:

```bash
$ sudo vi /etc/yum.repos.d/atolab.repo
```

and writing the following content:

```toml,ignore
[atolab-repo]
name=Atolab RPM Package Repo
baseurl=http://pkgs.adlink-labs.tech/centos/     # server-name or repo-server-ip
enabled=1
gpgcheck=0
```

At this point update the packages and install as follows:

```bash
$ yum repolist
$ yum update
$ yum install yaks
```

----
## Testing Your Installation
To test the installation, try to see the Yaks man page by executing the following command:

```bash
$ zenohd --yaks.help
```
You should see the following output on your console:

```text
YAKSD(1)                         Yaksd Manual                         YAKSD(1)



NAME
       yaksd

SYNOPSIS
       yaksd [OPTION]...

OPTIONS
       --help[=FMT] (default=auto)
           Show this help in format FMT. The value FMT must be one of `auto',
           `pager', `groff' or `plain'. With `auto', the format is `pager` or
           `plain' whenever the TERM env var is `dumb' or undefined.
[...]           
```

----
## Pick Your Programming Language
To interact with Yaks, you will need the API.
Below is the list of supported programming languages as well as links to the installation instruction:

- [Python API](https://github.com/atolab/yaks-python)
- [Java API](https://github.com/atolab/yaks-java)
- [Go API](https://github.com/atolab/yaks-go)
- [OCaml API](https://github.com/atolab/yaks-ocaml)

