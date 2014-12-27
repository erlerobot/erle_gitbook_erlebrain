# Linux kernel

The Linux kernel has not been designed to meet real-time constraints however there are some techniques that can help tune the kernel to meeting certain deadlines. Four kernels have been evaluated (some available [here](https://github.com/erlerobot/kernels/tree/master/bbb/3.8.13.x)):

The following tests will be formed with a vanilla kernel compiled with the PREEMPT option. We will make use of [cylictests](https://rt.wiki.kernel.org/index.php/Cyclictest) code and we will also torture the kernel with `stress`to obtain the maximum latencies.

### Torturing the kernel

For this purpose we use the [stress](http://linux.die.net/man/1/stress) Linux command line tool.

While running the autopilot and using:
``` bash
stress --cpu 8 --io 8 --vm 2 --vm-bytes 128M
```
We can see that the load of the system raises up to `17` percent approximately:
```bash
root@beaglebone:~# uptime
 03:20:20 up 14 min,  3 users,  load average: 17.07, 7.86, 3.21
```

Let's push a bit more launching more threads and io operations:
```bash
stress --cpu 64 --io 64 --vm 2 --vm-bytes 128M
```
With this, the `uptime` command returns:
```bash
root@beaglebone:~# uptime
 03:27:10 up 21 min,  3 users,  load average: 129.08, 81.93, 36.63
```

### Running cyclictests
Now that we have the processor stressed we use `cyclictests` to measure the latencies:
```bash
root@beaglebone:~# cyclictest -t1 -p 80 -n -i 10000 -l 10000
# /dev/cpu_dma_latency set to 0us
policy: fifo: loadavg: 0.47 0.65 0.44 1/98 1312           

T: 0 ( 1307) P:80 I:10000 C:   3892 Min:     19 Act:   26 Avg:   67 Max:   13743
```

