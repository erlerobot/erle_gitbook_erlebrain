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
root       126 -20  39      - [kworker/0:1H]
root       175   0  19      - [jbd2/mmcblk0p2-]
root       176 -20  39      - [ext4-dio-unwrit]
root       207   0  19      - [kworker/0:2]
root       209   0  19      - /lib/systemd/systemd-journald
root       221   0  19      - /sbin/udevd
root       305   0  19      - /sbin/udevd
root       306   0  19      - /sbin/udevd
avahi      570   0  19      - avahi-daemon: running [beaglebone.local]
root       577   0  19      - /usr/sbin/acpid
101        587   0  19      - /usr/bin/dbus-daemon --system --address=systemd:
root       590   0  19      - /bin/bash /etc/init.d/apm4-startup.sh
root       594   0  19      - /sbin/wpa_supplicant -u -s -O /var/run/wpa_suppl
root       595   0  19      - /usr/bin/node autorun.js
root       599   0  19      - /lib/systemd/systemd-logind
root       603   0  19      - /usr/sbin/rsyslogd -n -c5
root       629   0  19      - /sbin/agetty tty1 38400
root       631   0  19      - /sbin/agetty -s ttyO0 115200 38400 9600
avahi      653   0  19      - avahi-daemon: chroot helper
root       661   0  19      - [spi1]
root       674   0  19      - [spi2]
root       686 -20  39      - [OMAP UART5]
xrdp       691   0  19      - /usr/sbin/xrdp
root       697 -20  39      - [OMAP UART4]
root       774   0  19      - /usr/sbin/xrdp-sesman
root       795 -20  39      - [OMAP UART2]
root       835   0  19      - /usr/sbin/cron
root       838   0  19      - [file-storage]
root       851   0  19      - /usr/sbin/sshd
root       910   0  19      - /sbin/agetty -s ttyGS0 115200 38400 9600
root      1020   0  19      - /bin/bash /etc/init.d/apm4-startup.sh
root      1025   -  52     12 ./ArduCopter.elf -A udp:192.168.7.1:6000 -B /dev
root      1047   0  19      - sshd: root@pts/0 
root      1049   0  19      - -bash
root      1076   0  19      - sshd: root@pts/1 
root      1078   0  19      - -bash
root      1109   0  19      - [kworker/0:0]
root      1110   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1111   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1112   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1113   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1114   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1115   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1116   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1117   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1118   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1119   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1120   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1121   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1122   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1123   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1124   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1125   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1126   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1127   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1128   0  19      - stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
root      1129   0  19      - [flush-179:0]
root      1132   0  19      - ps -eo user,pid,nice,pri,rtprio,command

```

