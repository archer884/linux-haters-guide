# Chapter 2: Networking

## ssh

The best guide for setting up key pairs is straight from [github.com](https://docs.github.com/en/authentication/connecting-to-github-with-ssh). I'll provide you with the basic gist of it.

To generate a key:
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Do whatever you want with your passphrase. Passphrases are NOT annoying on Linux, but they may be on other platforms. At least with Gnome, the shell will ask if you'd like to automagically unlock your key any time you log on. (Which is awesome, obviously.) In order to enable that behavior, you need to continue on to the next step.

```shell
❯ eval "$(ssh-agent -s)"
> Agent pid 59566
❯ ssh-add ~/.ssh/id_ed25519
```

The final step is optional; you would only do this if you've got some server for which you're just tired of entering the password over and over and *over* and ***over....***

```shell
❯ ssh-copy-id <username>@<host>
# ...ssh-copy-id will ask for your password, etc...
```

### Adding a key to a server

This is way easier than using secure copy or whatever. I mean, for one thing, it automagically figures out which key to copy and where to put it.

```shell
❯ ssh-copy-id -i ~/.ssh/id_ed25519 archer@carbon.local
```

## Nordvpn

Installing nordvpn is annoying and I forget how. For right now, let's talk about how to configure it. You can basically just open up your terminal and punch in the following:

```shell
nordvpn connect
```

The program will ask for your credentials, so that's not a part of the configuration I'm talking about. The real annoyance is that you won't be able to connect to your local network once you're connected UNLESS you do this next part, which is important for a guy who keeps most of his files on a server connected by a few runs of CAT6.

```shell
sudo nordvpn whitelist add subnet 192.168.<somenumber>.0/24
```

That number in the middle is going to depend on how your home network is configured. It could be zero, one, two, blue, whatever. The final digit is gonna be zero and I have no idea what the 24 is for, but the point is that UNLESS YOU DO THIS, your computer will only be able to connect to things through the VPN itself. Some other guy mentioned needing to whitelist his DNS server (via `nordvpn set dns`), but I have not experienced this.

From: [https://askubuntu.com/questions/1276829/nordvpn-local-network-addresses-not-reachable-when-connected](https://askubuntu.com/questions/1276829/nordvpn-local-network-addresses-not-reachable-when-connected)

## That local file server I mentioned...

Let's talk about SMB shares. Most Linux file explorers work fine for this stuff: they'll detect your network and automagically connect you to whatever they can, and they'll happily retain your passwords until the cows come home. That's all great. The problem is that, unlike macOS, your Gnome shell or what have you is liable to mount your shared drive at some absolutely insane location on your file system. You thought `/Volumes/foo/bar` was annoying? ...Ha! Just wait.

Here's where mine gets mounted today:

```
/run/user/1000/gvfs/smb-share:server=<local_address>,share=<share_name>
```

Why? Because man is fallen, that's why. This is your punishment! Now, obviously no one in his right mind wants to actually type that nonsense in when using a terminal and changing directories, so let's talk about how you want to fix that. You are probably thinking, "Well, obviously the right way is to just edit the fstab to automatically mount the drive to the right location when I boot."

WRONG. That never works.

It's something about the order in which the network loads up vs. when the disks are loaded up and to be real honest I just don't freaking care, but we're not doing it that way. INSTEAD, we're doing this:

```shell
ln -s /some/ridiculous/path /less/ridiculous/path
```

Links are infinitely superior to mount points because this link will start working at whatever time you choose to actually mount this thing. I can hear you already: "But John, you suave, sexy software savant, what if it gets mounted in a different place next time?" Well, it won't. Your computer isn't that creative: the mount point is deterministic and based on your user id, the thing doing the mounting (I'm assuming that's what gvfs is?), and the thing being mounted.

Anyway, even if you change GUIs and it changes the path, just change the damn link. It ain't rocket surgery, and unlike the whole fstab thing it will at least WORK.

<!-- FIXME: see if this works https://help.ubuntu.com/community/MountCifsFstab -->
