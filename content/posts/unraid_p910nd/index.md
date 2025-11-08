---
title: "Install and Use p910nd on Unraid 7.x"
draft: false
date: 2025-11-01
authors:
  - name: Moritz J.
    # link: https://github.com/XYZ
    # image: https://github.com/XYZ
tags:
  - Printer
  - Unraid
showToc: true
params:
  images:
    - "/images/og/unraid_p910nd.png"
---

This guide explains how to install [p910nd](https://github.com/kenyapcomau/p910nd) on your Unraid server, since it’s not available in the Community Apps Store.

<!--more-->

## Introduction

> [!QUESTION]- **p910nd** − port 9100+n printer daemon
> **p910nd** is a small daemon that copies any data received on the port it is listening on to the corresponding printer port.   
> [...]  
> The default is port 9100 to /dev/lp0.  
> Source: https://man.cx/p910nd

After some searching, I found this post on the [Unraid Forum](https://forums.unraid.net/topic/2802-simple-printing-service-for-unraidbubbaraid/#comment-63092), but unfortunately the attached file is no longer available.  
So here is how to build & install **p910nd** yourself:

## Building p910nd

> [!INFO]
> This guide assumes that you are building p910nd directly on your Unraid machine.

### 1. Create a build directory & the Dockerfile

Let’s start by building the **p910nd** binary inside a lightweight Docker container.

```bash
mkdir -p /boot/custom/build/p910nd
cd /boot/custom/build/p910nd

cat <<'EOF' > Dockerfile
FROM alpine:latest
RUN apk add --no-cache build-base git
WORKDIR /src
RUN git clone https://github.com/kenyapcomau/p910nd
WORKDIR /src/p910nd
RUN make CFLAGS="-static" LDFLAGS="-static"
CMD ["cp", "p910nd", "/out/p910nd"]
EOF
```

### 2. Build the Docker image

```bash
docker build -t p910nd-builder .
```

### 3. Run the container to extract the compiled binary

```bash
mkdir -p /boot/custom/bin
docker run --rm -v /boot/custom/bin:/out p910nd-builder
```

The compiled binary is now available at `/boot/custom/bin/p910nd`.

## Installing p910nd (persistent) 

By default, Unraid boots from a flash drive and loads the OS into RAM.  
Because of this, we need to ensure the binary is copied into the live system and started automatically at every boot.
We can achieve this by editing our `/boot/config/go` file.

### 1. Add the following to `/boot/config/go`

```bash
cp /boot/custom/bin/p910nd /usr/local/bin/p910nd
chmod +x /usr/local/bin/p910nd
/usr/local/bin/p910nd -f /dev/usb/lp0 -b
```

This copies the binary, make it executable and starts it.  
`-f` specifies the USB printer device (e.g. `/dev/usb/lp0`)  
`-b` runs the daemon in the background.

Your Unraid system should now automatically start **p910nd** on boot, making your printer available over the network.

### 2. Reboot Unraid

## Check if p910nd is running correctly

### Verify that **p910nd** is running

```bash
# pgrep -a p910nd
4978 /usr/local/bin/p9100d -f /dev/usb/lp0 -b

# ss -tlnp | grep 9100
LISTEN 0      5       0.0.0.0:9100       0.0.0.0:*    users:(("p910nd",pid=4978,fd=4))
```

### Test Print
You can send a test print through `p910nd` using:  
`echo "Hello World!" | nc UNRAIDSERVERIP 9100`

In Unraid, under **Tools** → **System Log**, you should see something like:
```
Nov  1 11:12:31 miniNAS p9100d[4975]: Connection from 192.168.10.26 port 58628 accepted
Nov  1 11:12:31 miniNAS p9100d[4975]: Finished job: 13/13 bytes sent to printer, 0/0 bytes sent to network
```