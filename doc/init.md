# Sample init scripts and service configuration for cloudenoded

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/cloudenoded.service:    systemd service unit configuration
    contrib/init/cloudenoded.openrc:     OpenRC compatible SysV style init script
    contrib/init/cloudenoded.openrcconf: OpenRC conf.d file
    contrib/init/cloudenoded.conf:       Upstart service configuration file
    contrib/init/cloudenoded.init:       CentOS compatible SysV style init script

# Service User

All three startup configurations assume the existence of a "cloudenode" user
and group.  They must be created before attempting to use these scripts.

# Configuration

At a bare minimum, cloudenoded requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, cloudenoded will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that cloudenoded and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If cloudenoded is run with `-daemon` flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

```bash
bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'
```

Once you have a password in hand, set `rpcpassword=` in `/etc/cloudenode/cloudenode.conf`

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/cloudenode.conf`.

# Paths

All three configurations assume several paths that might need to be adjusted.
```
Binary:              /usr/bin/cloudenoded
Configuration file:  /etc/cloudenode/cloudenode.conf
Data directory:      /var/lib/cloudenoded
PID file:            /var/run/cloudenoded/cloudenoded.pid (OpenRC and Upstart)
                     /var/lib/cloudenoded/cloudenoded.pid (systemd)
```
The configuration file, PID directory (if applicable) and data directory
should all be owned by the cloudenode user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
cloudenode user and group.  Access to cloudenode-cli and other cloudenoded rpc clients
can then be controlled by group membership.

# Installing Service Configuration

## systemd

Installing this .service file consists on just copying it to
`/usr/lib/systemd/system` directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run "systemctl start cloudenoded" and to enable for system startup run
`systemctl enable cloudenoded`

## OpenRC

Rename cloudenoded.openrc to cloudenoded and drop it in `/etc/init.d`.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/cloudenoded start` and configure it to run on startup with
`rc-update add cloudenoded`

## Upstart (for Debian/Ubuntu based distributions)

Drop cloudenoded.conf in `/etc/init`.  Test by running "service cloudenoded start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

## CentOS

Copy cloudenoded.init to `/etc/init.d/cloudenoded`. Test by running "service cloudenoded start".

Using this script, you can adjust the path and flags to the cloudenoded program by
setting the CLOUDENODED and FLAGS environment variables in the file
`/etc/sysconfig/cloudenoded`. You can also use the DAEMONOPTS environment variable here.

# Auto-respawn

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
