# Thread priorities

```
root@beaglebone:~# ps -eo user,pid,nice,pri,rtprio,command
USER       PID  NI PRI RTPRIO COMMAND
root         1   0  19      - /lib/systemd/systemd
root         2   0  19      - [kthreadd]
root         3   0  19      - [ksoftirqd/0]
root         5 -20  39      - [kworker/0:0H]
root         7 -20  39      - [kworker/u:0H]
root         8   - 139     99 [migration/0]
root         9   0  19      - [rcu_preempt]
root        10   0  19      - [rcu_bh]
root        11   0  19      - [rcu_sched]
root        12   - 139     99 [watchdog/0]
root        13 -20  39      - [khelper]
root        14   0  19      - [kdevtmpfs]
root        15 -20  39      - [netns]
root        17   0  19      - [bdi-default]
root        18 -20  39      - [kintegrityd]
root        19 -20  39      - [kblockd]
root        20   0  19      - [khubd]
root        21   -  90     50 [irq/70-44e0b000]
root        22   0  19      - [kworker/u:1]
root        25   -  90     50 [irq/7-tps65217]
root        28   -  90     50 [irq/30-4819c000]
root        37 -20  39      - [rpciod]
root        39   0  19      - [khungtaskd]
root        40   0  19      - [kswapd0]
root        41   0  19      - [fsnotify_mark]
root        42 -20  39      - [nfsiod]
root        43 -20  39      - [crypto]
root        46 -20  39      - [pencrypt]
root        47 -20  39      - [pdecrypt]
root        54 -20  39      - [OMAP UART0]
root        56 -20  39      - [kpsmoused]
root        57   -  90     50 [irq/134-mmc0]
root        69 -20  39      - [binder]
root        70 -20  39      - [deferwq]
root        71   0  19      - [kworker/u:2]
root        74   0  19      - [mmcqd/1]
root        75   0  19      - [mmcqd/1boot0]
root        76   0  19      - [mmcqd/1boot1]
root       131 -20  39      - [kworker/0:1H]
root       179   0  19      - [jbd2/mmcblk0p2-]
root       180 -20  39      - [ext4-dio-unwrit]
root       212   0  19      - /lib/systemd/systemd-journald
root       224   0  19      - /sbin/udevd
root       308   0  19      - /sbin/udevd
root       309   0  19      - /sbin/udevd
avahi      589   0  19      - avahi-daemon: running [beaglebone.local]
101        593   0  19      - /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --
root       596   0  19      - /usr/sbin/acpid
root       600   0  19      - /usr/bin/node autorun.js
root       601   0  19      - /sbin/wpa_supplicant -u -s -O /var/run/wpa_supplicant
root       604   0  19      - /lib/systemd/systemd-logind
root       610   0  19      - /usr/sbin/rsyslogd -n -c5
root       619   0  19      - /bin/bash /etc/init.d/apm4-startup.sh
root       641   0  19      - /sbin/agetty tty1 38400
root       642   0  19      - /sbin/agetty -s ttyO0 115200 38400 9600
avahi      661   0  19      - avahi-daemon: chroot helper
root       699   0  19      - [spi1]
xrdp       700   0  19      - /usr/sbin/xrdp
root       708   0  19      - [spi2]
root       724 -20  39      - [OMAP UART5]
root       750 -20  39      - [OMAP UART4]
root       801   0  19      - /usr/sbin/xrdp-sesman
root       818 -20  39      - [OMAP UART2]
root       845   0  19      - /usr/sbin/cron
root       852   0  19      - [RTW_CMD_THREAD]
root       859   0  19      - /usr/sbin/sshd
root       871   0  19      - [file-storage]
root       896   0  19      - /usr/local/bin/hostapd -B -P /var/run/hostapd.pid /etc/hostapd/hostapd.c
root       941   0  19      - /bin/bash /etc/init.d/apm4-startup.sh
root       992   -  52     12 ./ArduCopter.elf -A udp:192.168.7.1:6000 -B /dev/ttyO5
root       993   0  19      - /usr/sbin/udhcpd -S
root      1001   0  19      - /usr/sbin/udhcpd -S /etc/udhcpd.conf
root      1026   0  19      - sshd: root@pts/0
root      1028   0  19      - -bash
root      1058   0  19      - /sbin/agetty -s ttyGS0 115200 38400 9600
root      1076   0  19      - [flush-179:0]
root      1080   0  19      - [kworker/0:2]
root      1083   0  19      - [kworker/0:0]
root      1143   0  19      - [kworker/0:1]
root      1185   0  19      - ps -eo user,pid,nice,pri,rtprio,command

```

