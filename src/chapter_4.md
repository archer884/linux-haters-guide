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

## Hotel Wi-Fi

You would think that something as basic as hotel wifi would be absolutely no big deal at all, but you'd be wrong. In all likelihood, when you connect to the hotel's wifi, you'll just get a browser window that spins for a while and gives up. I suppose this is probably some kind of a security feature. In any case, you need a workaround, and here it is.

**When you connect to the hotel's network, find out your default gateway.**

There are probably a dozen ways to do this on Linux. The best one is to run the following command:

```shell
ip route
```

Then follow these simple steps:

1. Grab the offending IP address
2. Slip that bad boy into your browser's address bar
3. Profit

Seriously.
