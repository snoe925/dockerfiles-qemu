Docker configuration to run FreeBSD in qemu in Docker.  Provides novnc for web base console on http://localhost:8080/vnc.html

This is *FROM scratch* using https://partner-images.canonical.com/core/xenial/current/ubuntu-xenial-core-cloudimg-amd64-root.tar.gz. You will need to download that into the directory containing the Dockerfile.

```
docker run -t -i -v $HOME/images:/images -p 8080:8080 -p 10022:10022 snoe/qemu
```
## How it works, thanks qemu, novnc and websockify

```
# Run these two programs in parallel to get VNC console on 8080
qemu-system-x86_64 -m 512M -hda /images/*  -net nic -net user -vnc :0 -redir tcp:10022::22
websockify --web=/usr/share/novnc 8080 localhost:5900
```
## Some useful FreeBSD configurations once booted

```
dhclient em0
service sshd onestart
```

Get the QEMU image 

```
curl http://ftp.freebsd.org/pub/FreeBSD/snapshots/VM-IMAGES/11.0-STABLE/amd64/Latest/ | \
   xz -d >$HOME/images/freebsd.qcow2
```
