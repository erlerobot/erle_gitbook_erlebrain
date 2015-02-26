# Creating a drone app

This section covers how to develop a simple drone application (snap) with Snappy Ubuntu Core using [Erle-Brain](erlerobotics.com/blog/product/erle-brain).

### `erle-date` app

The following code will help us understand how to create a simple drone application that will print in the standart output the date in the drone.

#### Files

``` bash
.
├── meta
│   ├── erle-small.png
│   ├── package.yaml
│   └── readme.md
└── src
    └── script.sh

```

A snap needs at the very least:
 - a `meta` folder
 - a `meta/package.yaml` describing the app
 - a `meta/readme.md` with at least two lines describing the app.
 - source code (we put this into the `src` folder)


Let's take a look at each one of these files:

##### `meta/package.yaml`

 ```yaml
name: erle-date.erle
vendor: Erle Robotics <contact@erlerobot.com>
icon: meta/erle-small.png
version: 1.0
architecture: armhf
binaries:
 - name: src/script.sh
maintainer: Víctor Mayoral <victor@erlerobot.com>

 ```

##### `meta/readme.md`
```
Erle Robotics date snap example

This snap outputs the date.
(contact@erlerobotics.com)
```

##### `src/script.sh`
```bash
#!/bin/bash

#date >> /home/ubuntu/date.txt
echo "date: $(date)"

```

#### Building the app
From the root directory of the app we proceed using the following command:
```bash
snappy build .
```
This will create a file called `erle-date.erle_1.0_armhf.snap` that can be installed or uploaded to the [app store](https://myapps.developer.ubuntu.com/dev/click-apps/?format=snap).

#### Installing the app
In order to install the app, run:
```bash
sudo snappy install erle-date.erle_1.0_armhf.snap
```

#### Running the application
After having installed the app, you should be able to run it through:
```bash
erle-date.erle.script.sh
```

which will produce:

```bash
date: Mon Feb 23 15:22:20 UTC 2015
```

### Sources
 - [Source code](https://github.com/vmayoral/erle-date.erle) of `erle-date` app