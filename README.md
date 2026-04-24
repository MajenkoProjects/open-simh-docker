OpenSIMH Docker Images
======================

These are base images intended to be used to create docker containers for
OpenSIMH VAX and PDP-11 emulators.  The aim is to have everything self contained
and all the configuration for the emulator generated on the fly from environment
variables in the docker compose configuration file.

Sample configuration:

```yaml
services:
  vax:
    image: majenko/open-simh-vax:latest
    restart: always
    network_mode: host
    cap_add:
      - NET_ADMIN
    volumes:
      - type: bind
        source: /tank/machine/ACE
        target: /machine
      - type: bind
        source: /dev/net
        target: /dev/net
    environment:
      - SYSTEM=ACE
      - BRIDGE=dn0
      - CONSOLE=3001
      - RQ0=RA92
      - RQ1=RA92
      - RQ2=RA92
      - RQ3=CDROM
      - NET=DEQNA
      - MEM=64M
      - IDLE=VMS
```

The main things to configure are the location for storing configuration and disk images (best to
put that on a bind volume so you can replace the generated images with real images with your
chosen OS on it), the disk types, network bridge device (see below) and OS type.

Networking
----------

Networking is set up through TAP using a bridge device to the host. It is your responsibility to
create and manage said bridge device. A special TAP interface (named vax.<system>) is created and
added to the specified bridge.

Keep Ini File
-------------

If you want to use your own INI file, or keep the INI file untouched after the
first generation, specify `KEEP_INI=YES` in the environment. This will only
generate an INI file if there isn't currently one existing.

Caution
=======

This is very much a work-in-progress and things are liable to change at any time. Ideas and suggestions
for how to improve the system are more than welcome.
