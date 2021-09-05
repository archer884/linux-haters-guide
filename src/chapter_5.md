# Chapter 5: Stupid Human Tricks

## Github with multiple SSH keys

This is actually pretty easy, although [this guide](https://medium.com/@trionkidnapper/ssh-keys-with-multiple-github-accounts-c67db56f191e) makes it seem a little more complicated than is absolutely necessary (it includes instructions on how to make the SSH key, which you should already have figured out by looking at [GitHub's own documentation](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)). What you do is you perform your initial clone operation using a subdomain, and you configure your ssh-agent to use a given ssh key when accessing that particular subdomain.

The config file in question is found at `~/.ssh/config` (and won't exist, probably, until you create it yourself). The configuration element you'll need to add will be similar to the following:

```
Host foo.github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_foo
```

There. Now you can log into multiple separate GitHub accounts automagically and without huge degrees of annoyance. On a related note....

## Alternative git authorship

Wanna commit something but don't want your normal email address on it? I got you, fam. (Or, actually, [this guy does](https://stackoverflow.com/questions/8337071/different-gitconfig-for-a-given-subdirectory) I like his strategy better than the one I was going to tell you about.)

What you can do is create a subdirectory (like, if your code is usually under `~/src`, just use `~/src/foo` instead) and configure git to use a little extra configuration in the event that you're editing something found in that subdirectory. Cool, right? It looks like this:

```
[includeIf "gitdir:~/src/foo/"]
    path = ~/src/foo/.gitconfig
```

Your conditional git config can contain whatever you like, but I'd recommend just throwing in an alternate name and email. Hell, what else do you need?
