# eos-bgrt
EndeavourOS BGRT Plymouth bootscreen animation

1. install plymouth-git and KDE/plasma settings snippet: `yay -S plymouth-git plymouth-kcm`
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
4. use KDE settings (Plymouth) to set the theme. Or alternative use commandline: `sudo plymouth-set-default-theme eos-bgrt`
5. add dropin for dracut to add it to the initram image:
`sudo nano /etc/dracut.conf.d/plymouth.conf`  and add this:
`add_dracutmodules+=" plymouth "`
6. add needed Kernel parameter for silent boot (not showing boot messages)
`sudo nano /etc/kernel/cmdline` and add this 3 options:
`splash quiet plymouth.nolog ...`
7. regenerate images: `sudo reinstall-kernels`

