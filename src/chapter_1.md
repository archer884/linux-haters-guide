# Chapter 1: Development tooling

First, you'll need git. The distribution I've been using comes with that and a few other items (like gcc, a linker, etc) and so I don't know what commands I would use to install those, because I haven't done it lately.

## Git

You're going to need a .gitconfig file. Here's mine, largely inspired by Keith Dahlby's file (still findable as a [gist](https://gist.github.com/dahlbyk/cb062ca2823cce8c4122ce482f6f2678) when last I checked). I believe he also applies periodic updates, so it may be worth your time to take a look. Keith also has a primer on [github](https://gist.github.com/dahlbyk/0b210ed9bb6720bd95fcb785440004c8).

```
[alias]
    co = checkout
    fp = format-patch -C -M --no-binary -o C:/Patches
    pub = !git push origin HEAD:master && git push . HEAD:master && git checkout master
    # New branch
    nb = checkout origin -b
    # Push new branch
    pnb = push -u origin HEAD
    # Push new branch upstream
    pn = !git push --set-upstream origin \"$(git rev-parse --abbrev-ref HEAD)\"
    # Log Graph
    lg = log --oneline --decorate --graph
    # Log (Graph) All
    la = log --oneline --decorate --graph --all
    # Pretty print crap
    ls = log --pretty=format:"%C(green)%h\\ %C(yellow)[%ad]%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=relative
    # New since master
    new = log --oneline --decorate --reverse master..
    # Update from origin; keep master in sync; rebase on latest
    up = !git fetch --all --prune && git rebase origin && git push . origin/HEAD:master 2>/dev/null
    devup = !git fetch origin --prune && git rebase origin/dev && git push . origin/dev:dev 2> /dev/null
    # Update from origin; keep master in sync; don't rebase on latest
    upish = !git fetch --all --prune && git push . origin/HEAD:master 2>/dev/null
    # Reset to upstream (e.g. someone force-pushed)
    rup = reset @{u}
    # Diff what's in index
    di = diff --staged
    mt = mergetool
    dt = difftool
    ci = commit
    cia = commit --amend -C HEAD
    ciar = commit --amend -C HEAD --reset-author
    rbi = rebase --interactive
    rbc = rebase --continue
    rbe = rebase --edit-todo
    rba = rebase --abort
    rwi = !git rebase -i $(git merge-base master HEAD)
    # Rewrite in-place (rather than rebase on latest)
    rwi = !git rebase --interactive $(git merge-base origin HEAD)
    fpush = !git push --force-with-lease
    out = !git add -A && git commit -m \"gtfo $(date)\" && git push
    graph = !git log --color --graph --all --full-history --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    sgraph = !git log --graph --full-history --all --color \
    wipe = clean -xdf

[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true

[pull]
    rebase = true

[init]
    defaultBranch = master
```

## Rust

> **IMPORTANT NOTE:** The reality is you need to install zsh before completing all of this Rust stuff, because you want the rustup script to do some magic in your shell profile, etc. However, you want to come BACK up here and do the REST of this before completing shell setup.

For my personal setup, this is pretty much essential. I'm not that big a fan of downloading things, but building them from source is fine. There are a few important steps to this, starting with installing the Rust dev tools from [rustup.rs](https://rustup.rs/).

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Yeah, I know, you're not supposed to run strange scripts. Don't worry: this one's not strange. It's a *shell* script.

Step two for Rust is to install something called sccache, which you can do just as soon as your shell is configured to run cargo.

```shell
cargo install sccache
```

You'll then need to configure cargo to go ahead and *use* sccache by dropping the following into a toml config file located at ~/.cargo/config:

```toml
[build]
rustc-wrapper = "/home/archer/.cargo/bin/sccache"
```

That said, you *might* want to install sccache using the following command instead:

```shell
RUSTFLAGS="-C target-cpu=native" cargo install sccache
```

And it may also be a good idea to export that flag from your shell profile once you get around to setting that up. Speaking of which, let's talk about the last thing you need before throwing together a shell profile:

```shell
cargo install starship
cargo install exa
```

## Zsh

Yeah, you're gonna use zsh because bash is too annoying when cd'ing around the file system. Just accept it.

```shell
sudo apt install zsh
chsh -s $(which zsh)
```

Now, zsh is going to be an annoying little bitch when you launch your terminal, and it's going to ask for a lot of bullshit setup. Fuck that. Install oh-my-zsh and just use the following for your .zshrc

```shell
# exports
export ZSH="/home/archer/.oh-my-zsh"
export RUSTFLAGS="-C target-cpu=native"

# aliases
alias ls="exa -l --git --group-directories-first --no-permissions --no-user"

# Starship hotness!
eval $(starship init zsh)
```

> **Note:** The RUSTFLAGS export can be replaced with a specific target configuration in your .cargo/config (which is kind of cleaner, since the whole config is found in one place), but you need to know your target triple to configure that. See below for the correct configuration element for whatever computer I'm working on right now.

```toml
[target.x86_64-unknown-linux-gnu]
rustflags = ["-C", "target-cpu=native"]
```
