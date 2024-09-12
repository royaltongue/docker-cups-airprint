# Attribution
Forked From: znetwork/cups-avahi-airprint [GitHub](https://github.com/quadportnick/docker-cups-airprint) || [DockerHub](https://hub.docker.com/r/znetwork/synology-airprint)

# Description

This Ubuntu 22.04-based Docker image runs a CUPS instance that is meant as an AirPrint relay for printers that are already on the network but not AirPrint capable.

Among some other items of cleanup, this fork will install `printer-driver-all` instead of a select few drivers. For more information about which drivers are included, please see [the Ubuntu packages page](https://packages.ubuntu.com/jammy/printer-driver-all).

It's also worth mentioning that this installs cups-PDF as well, although that's from the upstream.

# Setup
Through Docker Compose:
```
services:
  cups:
    container_name: cups
    image: curiouscocoon/docker-cups-pdf-airprint:latest
    environment:
      - CUPSADMIN=CHANGE_ME
      - CUPSPASSWORD=CHANGE_ME
    volumes:
      - ./cups/services:/services
      - ./cups/config:/config

    network_mode: host
    restart: unless-stopped
```

### Volumes:
* `/config`: where the persistent printer configs will be stored
* `/services`: where the Avahi service files will be generated
* `/var/spool/cups-pdf/ANONYMOUS` : where cups-pdf will output prints

### Variables:
* `CUPSADMIN`: the CUPS admin user you want created - default is `admin` if unspecified
* `CUPSPASSWORD`: the password for the CUPS admin user - default is `admin` username if unspecified

### Ports/Network:
* Must be run on host network. This is required to support multicasting which is needed for Airprint.
 

# Usage

### Add and setup printer:
* CUPS will be configurable at http://[host ip]:631 using the CUPSADMIN/CUPSPASSWORD.
* Make sure you select `Share This Printer` when configuring the printer in CUPS.
* ***After configuring your printer, you need to close the web browser for at least 60 seconds. CUPS will not write the config files until it detects the connection is closed for as long as a minute.***