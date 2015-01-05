# Parámetro de APM: `SCHED_DEBUG`

Dentro de los [parámetros de APM](http://copter.ardupilot.com/wiki/arducopter-parameters/#Scheduler_debug_level_SCHED_DEBUG), `SCHED_DEBUG` permite ver los mensajes de depuración del planificador. Las opciones del parámetro son:

|VALOR |	SIGNIFICADO |
|------|---------|
|0	| Disabled |
| 2	| ShowSlips |
| 3	| ShowOverruns |

Configurando el parámetro a `2` imprime lo siguiente:
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
El primer número dentro de las impresiones de "PERF" muestra el número de bucles que tuvieron más de los esperado (por ejemplo, si el autopiloto está funcionando a 100HZ se espera que las tareas se ejecuten dentro de los 10ms).

El segundo número representa el tiempo máximo de necesito un ciclo en milisegundos.
