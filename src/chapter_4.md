# Chapter 4: Broken things

## Bluetooth

Taken from a random [github issue](https://github.com/pop-os/pop/issues/1623):

> ALRIGHT GUYS ! I HOPE THAT I HAVE FOUND A SOLUTION FOR US!
>
> Execute following commands as written below :
>
> ```shell
> sudo hciconfig hci0 down
> sudo rmmod btusb
> sudo modprobe btusb
> sudo hciconfig hci0 up
> ```
>
> Idk how this magic happened eventually on next reboots the bluetooth started working ! It might take you to click the on and off buttons few times (3-4) but it will work without a need of separate workout to be done on each reboot !
>
> Congrats everyone ! and thanks to this amazing person ! I'll drop his post's link too.
>
> CREDIT GOES TO :
>
> https://bbs.archlinux.org/viewtopic.php?pid=1953936#p1953936

This didn't actually work on my machine. The last step whines about "Can't init device hci0: Connection timed out (110)."
