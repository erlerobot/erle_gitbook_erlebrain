# Software

### Instalando las herramientas:
````bash
sudo apt-get install gawk gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

### Descargando el código
```bash
git clone http://github.com/erlerobot/ardupilot
```

### Compilando el código
````bash
cd ardupilot/ArduCopter
make configure
make pxf
```
Los binarios deben de estar bajo `tmp`.
