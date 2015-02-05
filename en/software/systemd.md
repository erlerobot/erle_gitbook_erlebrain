# Systemd

### First `systemd` Steps

>systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic.
>
> https://wiki.archlinux.org/index.php/Systemd

With `systemd` we have a `/etc/systemd/system/` directory chock-full of symlinks to files in /usr/lib/systemd/system/. /usr/lib/systemd/system/ contains init scripts; to start a service at boot it must be linked to /etc/systemd/system/. The systemctl command does this for you when you enable a new service, like this example for ClamAV:

# systemctl enable clamd@scan.service
ln -s '/usr/lib/systemd/system/clamd@scan.service' '/etc/systemd/system/multi-user.target.wants/clamd@scan.service'
How do you know the name of the init script, and where does it come from? On Centos7 they're broken out into separate packages. Many servers (for example Apache) have not caught up tosystemd and do not have systemd init scripts. ClamAV offers both systemd and SysVInit init scripts, so you can install the one you prefer:

$ yum search clamav
clamav-server-sysvinit.noarch
clamav-server-systemd.noarch
So what's inside these init scripts? We can see for ourselves:

$ less /usr/lib/systemd/system/clamd@scan.service
.include /lib/systemd/system/clamd@.service
[Unit]
Description = Generic clamav scanner daemon
[Install]
WantedBy = multi-user.target
Now you can see how systemctl knows where to install the symlink, and this init script also includes a dependency on another service, clamd@.service.

systemctl displays the status of all installed services that have init scripts:

$ systemctl list-unit-files --type=service
UNIT FILE              STATE
[...]
chronyd.service        enabled
clamd@.service         static
clamd@scan.service     disabled
There are three possible states for a service: enabled or disabled, and static. Enabled means it has a symlink in a .wants directory. Disabled means it does not. Static means the service is missing the [Install] section in its init script, so you cannot enable or disable it. Static services are usually dependencies of other services, and are controlled automatically. You can see this in the ClamAV example, as clamd@.service is a dependency of clamd@scan.service, and it runs only when clamd@scan.service runs.

None of these states tell you if a service is running. The ps command will tell you, or use systemctl to get more detailed information:

$ systemctl status bluetooth.service
bluetooth.service - Bluetooth service
   Loaded: loaded (/usr/lib.systemd/system/bluetooth.service; enabled)
   Active: active (running) since Thu 2014-09-14 6:40:11 PDT
  Main PID: 4964 (bluetoothd)
   CGroup: /system.slice/bluetooth.service
           |_4964 /usr/bin/bluetoothd -n 
systemctl tells you everything you want to know, if you know how to ask.