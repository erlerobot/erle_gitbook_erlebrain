# Linux kernel

The Linux kernel has not been designed to meet real-time constraints however there are some techniques that can help tune the kernel to meeting certain deadlines. Four kernels have been evaluated (some available [here](https://github.com/erlerobot/kernels/tree/master/bbb/3.8.13.x)):

- vanilla kernel (userspace)
- vanilla kernel compiled with the PREEMPT option
(userspace)
- RT PREEMPT patches applied (userspace) 
- Xenomai patches applied (userspace)

Given these kernels and the [cylictests](https://rt.wiki.kernel.org/index.php/Cyclictest) code we will torture the kernel to obtain the maximum latencies with each kernel.

### Torturing the kernel

For this purpose we use the [stress](http://linux.die.net/man/1/stress) Linux command line tool.

Using:
```

```

