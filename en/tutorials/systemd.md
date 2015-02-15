# Systemd

>systemd is a system and service manager for Linux, compatible with SysV and LSB init scripts. systemd provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, supports snapshotting and restoring of the system state, maintains mount and automount points and implements an elaborate transactional dependency-based service control logic.
>
> https://wiki.archlinux.org/index.php/Systemd


### A crash course

![](http://www.linux.com/images/stories/41373/Systemd-components.png)

With `systemd` we have a `/etc/systemd/system/` directory chock-full of symlinks to files in `/lib/systemd/system/`. `/lib/systemd/system/` contains init scripts; to start a service at boot it must be linked to `/etc/systemd/system/`. The `systemctl` command does this for you when you enable a new service.


### Cheatsheet
```bash
# systemctl start [name.service]
# systemctl stop [name.service]
# systemctl restart [name.service]
# systemctl reload [name.service]
# systemctl status [name.service]
# systemctl is-active [name.service]
# systemctl list-units --type service --all
# systemctl enable ntpd.service
# systemctl disable ntpd.service
```

### Useful stuff:
Check the services
```bash
systemctl
```

Check the services that failed
```bash
systemctl --failed
```

Check how much time each service takes:
```bash
systemd-analyze blame
```

### A simple example
First create the script that will be launched by the service:
``` bash
touch test-script.sh
touch test
```
```bash
cat << _EOF_ > test-script.sh
#!/bin/bash
echo "test: $(date)" >> /home/ubuntu/test
_EOF_
```

Now we create the service file:
```bash
cat << _EOF_ > /lib/systemd/system/test.service
[Unit]
Description=A test service for demo purposes

[Service]
Type=oneshot
ExecStart=/home/ubuntu/test-script.sh

[Install]
WantedBy=multi-user.target
_EOF_
```

We test that the service works:
```bash
systemctl start test.service
```
Finally we enable the service so that it starts at boot time
```bash
systemctl enable test.service
```

### Sources
- [Docs & info](http://www.freedesktop.org/wiki/Software/systemd/)
- [Understanding and Using Systemd](http://www.linux.com/learn/tutorials/788613-understanding-and-using-systemd)