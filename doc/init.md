# Sample init scripts and service configuration for bitcoinlegendd

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/bitcoinlegendd.service:    systemd service unit configuration
    contrib/init/bitcoinlegendd.openrc:     OpenRC compatible SysV style init script
    contrib/init/bitcoinlegendd.openrcconf: OpenRC conf.d file
    contrib/init/bitcoinlegendd.conf:       Upstart service configuration file
    contrib/init/bitcoinlegendd.init:       CentOS compatible SysV style init script

# Service User

All three startup configurations assume the existence of a "bitcoinlegend" user
and group.  They must be created before attempting to use these scripts.

# Configuration

At a bare minimum, bitcoinlegendd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, bitcoinlegendd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that bitcoinlegendd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If bitcoinlegendd is run with `-daemon` flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

```bash
bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'
```

Once you have a password in hand, set `rpcpassword=` in `/etc/bitcoinlegend/bitcoinlegend.conf`

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/bitcoinlegend.conf`.

# Paths

All three configurations assume several paths that might need to be adjusted.
```
Binary:              /usr/bin/bitcoinlegendd
Configuration file:  /etc/bitcoinlegend/bitcoinlegend.conf
Data directory:      /var/lib/bitcoinlegendd
PID file:            /var/run/bitcoinlegendd/bitcoinlegendd.pid (OpenRC and Upstart)
                     /var/lib/bitcoinlegendd/bitcoinlegendd.pid (systemd)
```
The configuration file, PID directory (if applicable) and data directory
should all be owned by the bitcoinlegend user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
bitcoinlegend user and group.  Access to bitcoinlegend-cli and other bitcoinlegendd rpc clients
can then be controlled by group membership.

# Installing Service Configuration

## systemd

Installing this .service file consists on just copying it to
`/usr/lib/systemd/system` directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run "systemctl start bitcoinlegendd" and to enable for system startup run
`systemctl enable bitcoinlegendd`

## OpenRC

Rename bitcoinlegendd.openrc to bitcoinlegendd and drop it in `/etc/init.d`.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/bitcoinlegendd start` and configure it to run on startup with
`rc-update add bitcoinlegendd`

## Upstart (for Debian/Ubuntu based distributions)

Drop bitcoinlegendd.conf in `/etc/init`.  Test by running "service bitcoinlegendd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

## CentOS

Copy bitcoinlegendd.init to `/etc/init.d/bitcoinlegendd`. Test by running "service bitcoinlegendd start".

Using this script, you can adjust the path and flags to the bitcoinlegendd program by
setting the BITCOINLEGENDD and FLAGS environment variables in the file
`/etc/sysconfig/bitcoinlegendd`. You can also use the DAEMONOPTS environment variable here.

# Auto-respawn

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
