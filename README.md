[![noswpatv3](http://zoobab.wdfiles.com/local--files/start/noupcv3.jpg)](https://ffii.org/donate-now-to-save-europe-from-software-patents-says-ffii/)
[![noswpatv3](http://zoobab.wdfiles.com/local--files/start/noupcv3.jpg)](https://ffii.org/donate-now-to-save-europe-from-software-patents-says-ffii/)
How Docker crashes itself by logging to stdout
==============================================

I had a docker process running on a small box, where the main application was
logging quite heavily to stdout. Suddenly, after a certain period of time, the
docker process would die.

How to reproduce
================

I tested version 1.9.1, but could not install a more recent version due to a
systemd dependency.

To reproduce, just run:

```
docker run -d dockercrash:latest
```

Or if you want an already built image:

```
docker run -d zoobab/dockercrash:latest
```

Then watch the docker process in htop eating all the memory.

Warnings
========

I tried this on my dedicated server, the docker process was killed by the
kernel when I had swap activated. When swap was not activated, I could not SSH
to the box anymore (SSH has some weird behaviour when a machine is fully
loaded).

Fixed version
=============

I quickly tested the latest release Docker version (1.13.0-rc1) on Ubuntu
16.04.1, and the issue seems to be fixed:

```
$ docker version
Client:
 Version:      1.13.0-rc1
 API version:  1.25
 Go version:   go1.7.3
 Git commit:   75fd88b
 Built:        Fri Nov 11 19:47:07 2016
 OS/Arch:      linux/amd64

Server:
 Version:             1.13.0-rc1
 API version:         1.25
 Minimum API version: 1.12
 Go version:          go1.7.3
 Git commit:          75fd88b
 Built:               Fri Nov 11 19:47:07 2016
 OS/Arch:             linux/amd64
 Experimental:        false
```

Affected versions
=================

Please email me at zoobab AT gmaildotcom with the docker version you run if you
are able to reproduce it.

* 1.9.1 (coreos)
* 1.10 (coreos)
```
$ docker  version
Client:
 Version:      1.10.3
 API version:  1.22
 Go version:   go1.5.3
 Git commit:   8acee1b
 Built:        
 OS/Arch:      linux/amd64

Server:
 Version:      1.10.3
 API version:  1.22
 Go version:   go1.5.3
 Git commit:   8acee1b
 Built:        
 OS/Arch:      linux/amd64

```

* 1.11 (sabayon-gentoo):
```
$ docker version
Client:
 Version:      1.11.0
 API version:  1.23
 Go version:   go1.6.2
 Git commit:   4dc5990
 Built:        
 OS/Arch:      linux/amd64

Server:
 Version:      1.11.0
 API version:  1.23
 Go version:   go1.6.2
 Git commit:   4dc5990
 Built:        
 OS/Arch:      linux/amd64

```

* 1.12.1 (coreos) (booted from https://github.com/coreos/coreos-vagrant.git commit 19af1c3355bbb4bf64b14614ddd2822983049d36) :
```
core@core-01 ~ $ docker version
Client:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   f1e1b83
 Built:        
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   f1e1b83
 Built:        
 OS/Arch:      linux/amd64
```

* 1.12.3 (ubuntu):
```
$ docker version
Client:
 Version:      1.12.3
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   6b644ec
 Built:        Wed Oct 26 21:44:32 2016
 OS/Arch:      linux/amd64
```

Links
=====

* Docker Daemon OOM Crash -- [Reproducible via docker run] #9799: https://github.com/docker/docker/issues/9799
* memory leak in buffer grow #9139 https://github.com/docker/docker/issues/9139
* Possible fix in master branch: https://github.com/docker/docker/pull/22982
