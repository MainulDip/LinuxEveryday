### Overviews

### Linux kernel /boot directory:
Kernel lives inside `/lib/modules` directory.

Linux kernel will not load all the `Modules` at a time, rather it will load only necessary modules. Modules are hardware drivers.

To force kernel what module to load and what not, rather than relying auto feature, edit `/etc/modules` file for force loading and `/etc/modprobe.d/blacklist.conf` for force blacklisting can be used

* use modprobe instead of insmod command for tweaking/troubleshoot kernel modules

### Kernel Panic:
Can happen because of faulty hardware or software. 
A system upgrade can trigger a kernel panic. In this case, revert back to previous kernel.
 
 - Reboot system holding `shift` to open grub menu, and choose previous kernel. linux alway keeps some previous kernels to go back incase current/updated kernel trigger error.
 - After resolving by choosing previous kernel, remove the faulty kernel and try to upgrade fresh again.