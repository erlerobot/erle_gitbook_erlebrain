# Software

### Installing the tools:
````bash
sudo apt-get install gawk gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
```

### Downloading the code
```bash
git clone http://github.com/erlerobot/ardupilot
```

### Compiling the code
````bash
cd ardupilot/ArduCopter
make configure
make pxf
```

The code binaries should be deployed under `/tmp``.

