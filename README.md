---
title: Watch Markdown
---

Displays the given markdown file rendered as HTML, and auto-update that display
if the markdown is modified.

## Usage

### Command-line usage

```bash
watch-markdown OPTIONS FILENAME
```

Where `FILENAME` is the name of a Markdown input file, and `OPTIONS` are:

Option           | Description
-----------------|----------------------
`-h` \| `--help` | Display this help.

### Desktop GUI usage

If you registered watch-markdown as the application to open markdown files,
then invoke it by double-clicking a markdown file.

Alternatively, use things like Gnome's context menu item "Open with other
application" on an individual markdown file.

## Description

When run on a markdown file, convert the markdown to an HTML file in /tmp,
using Pandoc. Launch a background process that watches the markdown file, using
entr, and regenerates the HTML if the markdown is modified. Open the
HTML, using Falkon (aka Gnome Web). This is a lightweight HTML browser which
has the nice property that it automatically updates if the HTML file is
modified.

This means you can double-click the markdown file to view it as HTML, while you
edit it in your favorite editor. Whenever you hit 'save', the HTML viewer
updates to show the changes.

When the HTML viewer app is closed, delete the HTML file, kill our background
process, and exit.

## Dependencies

Probably Linux-only. I'm developing and testing on Pop!OS, an
Ubuntu-derivative. Your luck elsewhere will depend on:

'entr' works on Linux or Macs.

Availability of Bash. I'm using v5.1.16.

We provide a .desktop file to register the application, which is a standard
that works on Gnome, KDE, and others.

We rely on other existing apt packages, such as pandoc and falkon, which are
currently installed by a hacky 'install' script.

## Install

For now, you can only install with a hacky 'install' script. Clone this repo
and run it. It `sudo apt install`s dependencies, and copies files into
`~/.local` and `~/.config`. There is an accompanying `uninstall` script that
undoes everything except the apt install (in case you already had those
packages installed).

I suggest making 'watch-markdown' the default application to open markdown
files in your desktop GUI. (On Gnome, right click a markdown file, Properties,
the "Open With" tab, and select "Watch Markdown".)

## Suggestions

When Falkon first opens, it has toolbars visible for its use as a web browser.
Personally, I don't use these, so I hide them all from the View menu, and this
setting persists for future invocations.

## Future plans

watch-markdown is an example of a pattern. It quickly wires together existing
applications to provide a live preview while users edit a file in their
favorite editor. This pattern is powerful and generic. It stands in opposition
to the more prevalent practice getting live previews by building dedicated
applications for each possible file type. Instead of requiring a whole new GUI
application, the core of watch-markdown remains the same, and a couple of lines
of config should suffice to handle any new filetype we want. (one to express
how to convert the filetype into a viewable format like HTML or SVG, and one to
express how we want to view that format, e.g. in a browser window.)

My intention is to tidy up the wrinkles in this implementation, and then make
it generic so that it allows users to configure how to convert markdown to
HTML, and what HTML viewer to use. This should make it possible for it to
handle more than just Markdown. Live previews should be possible for diverse
formats, from RST, graphviz files, or running unit tests.

