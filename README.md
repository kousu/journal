# Shell-friendly `journal`ling

`journal` in the spirit of Unix.

This defines a folder structure to keep your thoughts in, `$JOURNAL/$YEAR/$MONTH/$DAY.md`,
titling and opening each file for you at a keystroke.

It's meant to encourage introspection by keeping it close at hand.

Works with (and through) any basic text editor, and is compatible with [Diary.apk](https://f-droid.org/en/packages/org.billthefarmer.diary/) (if you can figure out a way to sync your locked-down Android files with your gnu/looniks freesoftware laptop),
or any text editor with Markdown support like [vscode](https://github.com/Microsoft/vscode) or [Ghostwriter](https://github.com/wereturtle/ghostwriter/)).

## Requirements:

* GNU date (so, probably basically Linux-only, please forgive)
* sh

## Installation

[Download](https://github.com/kousu/journal/raw/master/journal) and copy `journal` onto your PATH.

You can do this with

```
mkdir -p ~/.local/bin
echo "export PATH=$PATH:~/.local/bin" >> ~/.profile
ln journal ~/.local/bin/
```

## Usage

( for now: see the comments in the shell script [itself](./journal) )

------------------------------------------

# `rjournal`

This is a wrapper script that I use to share my journal across all of my computers.

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

Try adding this to your ~/.bashrc:

```
if [ $(($RANDOM % 100)) -lt 30 ]; then
  echo "Your Hypertext Memory:"
  echo
  EDITOR="cat" journal $(($(($RANDOM % 12)) + 2)) days ago | shuf -n 5
fi
```

Make it even more accessible with

```
alias j=journal
```

# TODO

* [ ] write a markdown-reader that picks out a random paragraph to print; use that instead of `EDITOR=cat | shuf -n 5`, which just picks a random set of lines
