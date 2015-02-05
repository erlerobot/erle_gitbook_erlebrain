# Systemd

### First `systemd` Steps

>systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic.
>
> https://wiki.archlinux.org/index.php/Systemd

With `systemd` we have a `/etc/systemd/system/` directory chock-full of symlinks to files in `/lib/systemd/system/`. `/lib/systemd/system/` contains init scripts; to start a service at boot it must be linked to `/etc/systemd/system/`. The systemctl command does this for you when you enable a new service, like this example:

File at `/lib/systemd/system/mountfs-rw.service`:
```
[Unit]
Description=Mount the system-a partition for rw
After=network.target auditd.service
#ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
ExecStart=mount -o remount,rw  /
ExecReload=mount -o remount,rw  /
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target
Alias=mountfs-rw.service

```

Enable the service:
```
systemctl enable mountfs-rw.service
Created symlink from /etc/systemd/system/mountfs-rw.service to /lib/systemd/system/mountfs-rw.service.
Created symlink from /etc/systemd/system/multi-user.target.wants/mountfs-rw.service to /lib/systemd/system/mountfs-rw.service.

```