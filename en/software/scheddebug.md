# `SCHED_DEBUG` APM parameter

Within the [APM parameters](http://copter.ardupilot.com/wiki/arducopter-parameters/#Scheduler_debug_level_SCHED_DEBUG), `SCHED_DEBUG` allows you to see the debugging messages of the scheduler. The options of the parameter are:


|VALUE |	MEANING |
|------|---------|
|0	| Disabled |
| 2	| ShowSlips |
| 3	| ShowOverruns |

Setting the parameter to `2` prints out the following:
```
PERF: 3/1000 23319
PERF: 5/1000 21594
PERF: 5/1000 22235
PERF: 4/1000 20590
PERF: 6/1000 22863
PERF: 5/1000 23568
PERF: 5/1000 23275
PERF: 4/1000 21487
PERF: 3/1000 22118
PERF: 3/1000 23658
PERF: 4/1000 23123
PERF: 4/1000 23441
PERF: 3/1000 18775

```

The first number within these "PERF" printouts show the number of loops that took longer than expected (e.g. if the autopilot is running at 100 Hz then we expect tasks to be completed within 10 ms). 

The second number represents the maximum time a cycle took in milliseconds.

