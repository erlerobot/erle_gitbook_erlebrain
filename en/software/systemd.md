# Systemd

### First `systemd` Steps

>systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic.
>
> https://wiki.archlinux.org/index.php/Systemd

With `systemd` we have a `/etc/systemd/system/` directory chock-full of symlinks to files in `/lib/systemd/system/`. `/lib/systemd/system/` contains init scripts; to start a service at boot it must be linked to `/etc/systemd/system/`. The systemctl command does this for you when you enable a new service, like this example:

