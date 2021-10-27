# Chapter 4: Broken things

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

## Open

This works fine on macOS, but `open` on a Linux machine points to God-only-knows-what. You should just add the following line to your shell profile:

```shell
alias open=xdg-open
```

Problem solved.

Well, not *solved,* really. Any Windows shell solves this by allowing you to simply prefix the path itself with an ampersand, but WE CAN'T HAVE NICE THINGS CAN WE.
