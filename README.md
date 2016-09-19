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

Affected versions
=================

Please email me at zoobab AT gmaildotcom with the docker version you run if you
are able to reproduce it.
