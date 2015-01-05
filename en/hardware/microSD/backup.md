#Backup 

The following steps describe how to **backup the content of your microSD card** copying the image to a compressed file named *erleimage.img.gz*:

   * Connect the microSD card to your computer using a USB adapter or a SD one (if your computer supports it).
   * Open a terminal and figure out the what is the interface under `/dev` (Linux systems, in Mac OS it's usually mounted under `/Volumes`) where your microSD has been mounted. Let's suppose `/dev/sdc`.
   * Backup the content:

```shell
sudo dd if=/dev/sdc bs=8M | gzip -c > erleimage.img.gz
```

---

*(Note: in Mac OS the `8M` should be substituted by `8m`. For example: `sudo dd if=/dev/disk1 bs=8m | gzip -c > wheezy-rt-3.8.13_9-2-14.img.gz`)*

---
