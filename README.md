# eos-bgrt
EndeavourOS BGRT Plymouth bootscreen animation 
(supports dracut/mkinitcpio and systemd-boot/Grub)

If using package it will be fully automatic adding all needed settings, and remove them on package removal.

# manually:

1. install plymouth and KDE/plasma settings snippet: `yay -S plymouth plymouth-kcm`
2. create systemd boot delay service:
   `sudo nano /etc/systemd/system/plymouth-wait-for-animation.service`
   insert this:

```
[Unit]
Description=Waits for Plymouth animation to finish
Before=plymouth-quit.service display-manager.service

[Service]
Type=oneshot
ExecStart=/usr/bin/sleep 15
[Install]
WantedBy=plymouth-start.service
```
   enable the service: `sudo systemctl enable plymouth-wait-for-animation.service`

3. get the theme:

   `git clone https://github.com/killajoe/eos-bgrt.git`

   copy it in place:
   `sudo cp -a eos-bgrt/eos-bgrt /usr/share/plymouth/themes/`

4. use KDE settings (Plymouth) to set the theme.

      Or alternative use commandline: `sudo plymouth-set-default-theme eos-bgrt`
   
5. add dropin for dracut to add it to the initram image (if using dracut)

   `sudo nano /etc/dracut.conf.d/plymouth.conf`  and add this:

   `add_dracutmodules+=" plymouth "`

   For mkinitcpio add `plymouth` to the HOOKS line in `/etc/mkinitcpio.conf`

6. add needed Kernel parameter for silent boot (not showing boot messages) for systemd-boot:
   
   `sudo nano /etc/kernel/cmdline` and add this 3 options:

   `splash quiet plymouth.nolog ...`

   For Grub usage in the grub CMD line at `/etc/default/grub`

   GRUB_CMDLINE_LINUX_DEFAULT=` ... splash quiet plymouth.nolog`

7. regenerate images: `sudo reinstall-kernels` (systemd-boot)

      `sudo dracut-rebuild` for grub.

