# Problemas con el reloj

En ocasiones cuando compilas el código o actualizas ficheros desde Internet se pueden obtener errores como:

```
error: newly created file is older than distributed files!   
```

Para solucionar este problema tienes que utilizar el siguiente comando en Ubuntu:
```
sudo ntpdate pool.ntp.org
```

*(En Angstrom puedes usar `/usr/bin/ntpdate -b -s -u pool.ntp.org`)*