# Chapter 7: Filesystems

The Linux filesystem is, to Windows adherents, one of the most confusing aspects of switching operating systems. I'm not going to help with that here. Figure that out on your own. This is for my notes on specific filesystem gotchas and commands, etc.

## Btrfs

Btrfs ("Butterface") is a powerful, modern filesystem format with a few sharp edges and footguns. For instance, you should never snapshot your /home directory.

### Never snapshot /home

Enough said.

### Exempt high-write directories

Directories containing lots of files that are or may be continually modified will collect pieces of older versions of files and take up a lot of room for no reason. To avoid that, you can deactivate Btrfs's copy-on-write characteristics for that directory.

```shell
chattr +C /path/to/dir
```

Any file created in such a directory will inherit the "NOCOW" attribute. Existing files will not, so this is better applied to empty directories, but there's no *harm* in putting it on a non-empty one.
