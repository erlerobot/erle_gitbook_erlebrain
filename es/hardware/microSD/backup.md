# Copia de seguridad

Los siguientes pasos describen como **hacer una copia de seguridad de tu tarjeta SD** copiando la imagen a archivo comprimido denominado *erleimage.img.gz*:

	* Conecta la tarjeta microSD a tu ordenador utilizando un adaptador USB o SD (si tu ordenador lo soporta).
    * Abre un terminal y comprueba cual es la interfaz bajo `/dev` (en sistema linux, en Mac OS usualmente se encuentra montado bajo `/Volumnes`) donde tu tarjeta microSD ha sido montada. Supongamos `/dev/sdc`.
    * Haz la copia de seguridad del contenido:

```shell
sudo dd if=/dev/sdc bs=8M | gzip -c > erleimage.img.gz
```
---

*(Nota: En Mac Os el `8M` debe ser sustituido por `8m`. Por ejemplo: `sudo dd if=/dev/disk1 bs=8m | gzip -c > wheezy-rt-3.8.13_9-2-14.img.gz`)*

---
