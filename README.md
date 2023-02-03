# Tuxedo Linux Kernel Self-Signed Certificate

We currently don't officially support Secure Boot as we don't (yet) have a Microsoft signed shim with an embedded TUXEDO certificate.

However we do still sign our Kernel with a self-signed certificate. So after manually importing and approving this certificate, TUXEDO OS can be run with Secure Boot enabled.

## Install:

- Secure boot needs to be deactivated
- Make sure shim is installed and you have booted from it
- Download TUXEDO Computers GmbH Secure Boot Signing.crt
- Make sure the download is correct and not corrupted
```
$ sha512sum TUXEDO\ Computers\ GmbH\ Secure\ Boot\ Signing.crt 
13887887159754640c69b4e270c7ccd89f6e06bae9f8addc91708cf93ce3fd565f712b8c7acaf0444c5da30aae2dfe0941b71c228a6bdd20facd28529c0aaa62  TUXEDO Computers GmbH Secure Boot Signing.crt
```
- Import certificate as a Machine Owner Key (MOK)
```
$ sudo mokutil --import TUXEDO\ Computers\ GmbH\ Secure\ Boot\ Signing.crt --timeout -1
```
(`--timeout -1` makes sure that you are not skipping the mok manager by accident next boot)
- Provide sudo password
- Provide one time password for mokutil
- Repead one time password to rule out typing errors
- Reboot
- MOKManager should pop up before Grub -> Use the menu to verify the imported key
- Reboot to UEFI menu and activate Secure Boot
- If you close the MOKManager by accident without having the key verified, you must run the mokutil command again before reboot
