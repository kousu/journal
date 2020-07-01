# Shell-friendly journalling

In the spirit of Unix, provides the `journal` command.
It defines a folder shape to keep your thoughts in, $JOURNAL/$YEAR/$MONTH/$DAY.md,
titles and opens them for you. It's meant to encourage diaries and introspection
by keeping them
(this is compatible with [Diary.apk](https://f-droid.org/en/packages/org.billthefarmer.diary/), and any basic text editor, or text editor with Markdown support like VSCode or [Ghostwriter](https://github.com/wereturtle/ghostwriter/)).

## Requirements:

* GNU date (so, probably basically Linux-only, please forgive)
* sh

## Installation

Copy `journal` onto your PATH.

You can do this with

```
mkdir -p ~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> ~/.profile
ln journal ~/.local/bin/
```

## Usage

( for now: see the comments in the shell script [itself](./journal) )

# `rjournal`

This is a script that I use to access my journal from all my devices by mounting it from a remote filesystem.


## Requirements

* The same as above plus
* `mountpoint` (from `util-linux`)

## Installation

Install it the same way:

```
ln rjournal ~/.local/bin/
```

**Get a remote disk somewhere**, e.g. https://rsync.net, your own server, a VPS, a NextCloud server like https://cloudamo.com/ or https://thegood.cloud/ or https://www.owncube.com/, and setup a `user`-mountable entry in `/etc/fstab` for it:

```
# /etc/fstab
# ...
user@example.com:~/Notebook /home/user/Notebook fuse.sshfs idmap=user,reconnect,intr,noauto,user    0 0
```

(you could also use cifs/smb/afp/WebDAV/[any of the other wild filesystem options out there](https://aur.archlinux.org/packages/?O=0&K=fuse), which would allow you to use Google Drive, Dropbox, etc)

```
echo "alias journal=rjournal" >> ~/.profile
```
(and reload your shell)

Then just

```
journal [DATE]
```

and you'll be journalling to the cloud. Hooray! (pick your server wisely, yah?)


## Tips and Tricks

Try adding this to your ~/.profile or ~/.bash_profile:

```
if [ $(($RANDOM % 100)) -lt 30 ]; then
  echo "Your Hypertext Memory:"
  echo
  EDITOR="head" journal $(($(($RANDOM % 12)) + 2)) days ago
fi
```

# TODO

* [ ] write a markdown-reader that picks out a random paragraph to print; use that instead of `EDITOR=head`
