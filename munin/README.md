# Fritzbox monitoring with munin

This directory contains the files to build a container with kiwi to monitor
a Fritzbox with munin and some scripts around it to start and manage the
container with systemd and podman.

The fritzbox monitoring container image uses munin to monitor your fritzbox.
Values are e.g. bandwith, uptime, CPU temperature, CPU usage, memory usage
and power consumption.
The IP address and the password of the fritzbox are required for this.
The container image also includes a web server to show the current statistic.

## Run the container

As the container requires the password to be able to login to your Fritzbo

With default configuration:

```
  podman run -d -p 80:80 -e FRITZBOX_PASSWORD_FILE=/etc/secrets/fritzbox-password registry.opensuse.org/home/kukuk/container/fritzbox-monitor:latest
```

The data get's fetched from the Fritzbox with the IP `192.168.178.1`, the
statistic can be seen on `http://localhost/munin/`.

For persistent data and HTML output:
```
  mkdir -p /srv/fritzbox
  podman run -d -v /srv/fritzbox:/srv/www/htdocs -p 80:80 -e FRITZBOX_PASSWORD_FILE=/etc/secrets/fritzbox-password registry.opensuse.org/home/kukuk/container/fritzbox-monitor:latest
```

## Environment Variables

- DEBUG=[0|1]		- Run entrypoint script in debug mode
- TZ                    - Set a timezone the container should use
- RUN_INTERVAL 		- Poll interval in seconds, default `300`
- FRITZBOX              - IP address of fritzbox, default `192.168.178.1`
- FRITZBOX_PASSWORD     - Login password for Fritzbox
- FRITZBOX_PASSWORD_FILE - File containing the login password for the Fritzbox
- RUN_WEBSERVER=[0|1]   - If a webserver should be started, default `1`

## Screenshot

![Screenshot](Screenshot.png)
