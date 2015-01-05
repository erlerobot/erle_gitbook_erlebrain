# Clock problems
Sometimes when compiling or fetching files from the Internet you get errors such as:
```
error: newly created file is older than distributed files!   
```

In order to fix this issue you have to use the following command in Ubuntu:
```
sudo ntpdate pool.ntp.org
```

*(In Angstrom you can use `/usr/bin/ntpdate -b -s -u pool.ntp.org`)*