# Tuxedo Linux Kernel Self-Signed Certificate

We currently don't officially support Secure Boot as we don't (yet) have a Microsoft signed shim with an embedded TUXEDO certificate.

However we do still sign our Kernel with a self-signed certificate. So after manually importing and approving this certificate, TUXEDO OS can be run with Secure Boot enabled.

## Install

1. Make sure shim is installed and you have booted from it (on TUXEDO OS and *buntus the package is called `shim-signed`)
2. Download `TUXEDO_Computers_GmbH_Secure_Boot_Signing.crt`
3. Make sure the download is correct and not corrupted
```
$ sha512sum TUXEDO\ Computers\ GmbH\ Secure\ Boot\ Signing.crt 
13887887159754640c69b4e270c7ccd89f6e06bae9f8addc91708cf93ce3fd565f712b8c7acaf0444c5da30aae2dfe0941b71c228a6bdd20facd28529c0aaa62  TUXEDO Computers GmbH Secure Boot Signing.crt
```
4. Import certificate as a Machine Owner Key (MOK)
```
$ sudo mokutil --import TUXEDO_Computers_GmbH_Secure_Boot_Signing.crt
```
5. Provide sudo password
6. Provide one time password for mokutil
7. Repead one time password to rule out typing errors
8. For tuxedo-drivers to work you also need to import the DKMS MOK
```
sudo mokutil --import /var/lib/shim-signed/mok/MOK.der
```
9. You might be asked for passwords again, see 5.-7.
10. Make sure that you do not miss MOKManager on next boot by disabling the timeout for it (temporary setting that gets automatically reset after next boot)
```
sudo mokutil --timeout -1
```
11. Reboot
12. MOKManager should pop up before Grub -> Use the menu to verify the imported keys (if you close the MOKManager by accident without having the key verified, you must run the mokutil commands again before reboot, see 4.-11.)
13. Reboot to UEFI menu and activate Secure Boot
14. If you close the MOKManager by accident without having the key verified, you must run the mokutil commands again before reboot

## Troubleshooting

On TUXEDO OS and *buntus, if `/var/lib/shim-signed/mok/MOK.der` does not exist on your system you can generate it with `sudo update-secureboot-policy --new-key` (you still need to enroll it afterwards as described in 9.). You might need to reinstall all DKMS packages after that for the respective modules to be signed with the new key.
