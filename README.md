# Shell-friendly `journal`ling

`journal` in the spirit of Unix.

This defines a folder structure to keep your thoughts in, `$JOURNAL/$YEAR/$MONTH/$DAY.md`,
titling and opening each file for you at a keystroke.

It's meant to encourage introspection by keeping it close at hand.

Works with (and through) any basic text editor, and is compatible with [Diary.apk](https://f-droid.org/en/packages/org.billthefarmer.diary/) (if you can figure out a way to sync your locked-down Android files with your gnu/looniks freesoftware laptop),
or any text editor with Markdown support like [Zettlr](https://zettlr.com/), [Obsidian](https://obsidian.md), [vscode](https://github.com/Microsoft/vscode) or [Ghostwriter](https://github.com/wereturtle/ghostwriter/)).

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

Set up your chosen path and editor in your ~/.profile or ~/.bash_profile:

```
export JOURNAL=~/Notes/Journal
export JOURNAL_EDITOR=vis
```

`JOURNAL` defaults to `~/Journal`.

`$JOURNAL_EDITOR` defaults to `$EDITOR`, `nano` then `vi`, in that order.


```
$ journal  # -> open today's entry: $JOURNAL/$yyyy/$mm/$dd.md
$ journal yesterday # -> open yesterday's entry
```


## Tips and Tricks

### Quick

Make it even more accessible with this in your ~/.bashrc

```
alias j=journal
```

### Memories

Try adding this to your ~/.bashrc:

```
if [ $(($RANDOM % 100)) -lt 30 ]; then
  echo "Your Hypertext Memory:"
  echo
  EDITOR="cat" journal $(($(($RANDOM % 12)) + 2)) days ago | shuf -n 5
fi
```


### Syncing

Use [gitwatch](https://github.com/gitwatch/gitwatch):

```
systemctl --user --now enable gitwatch@$(systemd-escape -- "-s 600 -r origin -R ${JOURNAL:-~/Journal}").service
```

See also my [cobbled-together version of gitwatch for Android](https://gist.github.com/kousu/623b25dbad2d084e4ffea540c65d3ff1).

## Related Work

* [Diarian for Obsidian](https://github.com/Erallie/diarian) is pretty good!
* There is [not yet](https://www.reddit.com/r/Zettlr/comments/16zu02z/calendar_view_for_journaldiary/) any way to do diaries in Zettlr, but I think this works well with it!

# TODO

* [ ] calendar view, à là Diarian, but as TUI.
* [ ] write a markdown-reader that picks out a random paragraph to print; use that instead of `EDITOR=cat | shuf -n 5`, which just picks a random set of lines
* [ ] import useful features from https://journalcli.me/docs/getting-started and org-mode
    * [ ] calendar integration
    * [ ] reminders
    * [ ] automatically copy unfinished TODOs forward
