---
layout: page-forkme
title: Sublime Text
---
<p class="message">This guide was originally written as a presentation given internally at Etsy. Hopefully I've scrubbed all the Etsy-specific bits.</p>
<p class="message">I welcome any edits and additions to this guide. Click the "Fork me on GitHub" ribbon on the right to get to the source.</p>

## Table of Contents

* [Introduction](#intro)
* [Installation](#installation)
* [Basic Navigation](#basic-navigation)
* [Essential Commands](#essential-commands)
* [Preferences and Customization](#preferences)
* [Miscellaneous Features](#misc-features)
* [Using Packages (Plugins)](#plugins)
* [The Console](#console)
* [Working with Remote Files](#remote-files)
* [Writing Plugins](#writing-plugins)
* [Resources](#resources)

## <a name="intro"></a> Introduction

[Sublime Text](http://www.sublimetext.com/) is a popular cross-platform (Mac, Windows, Linux) text editor. The core editor is the work of a single developer, Jon Skinner, who left his job at Google to pursue his dream of building a better text editor. At the time of its first release in 2008, [TextMate](http://macromates.com/) was an extremely popular editor for OS X, but development on it had stagnated and developers were eager to jump to something with similar power and minimalist aesthetic, but was being more actively developed.

The core framework is written in C++ and uses a custom UI toolkit, but a large part of Sublime Text's success is due to its extensibility via python plugins, of which [thousands have been written](https://sublime.wbond.net/).

The current stable version, 2.02, was released in July 2013. Version 3 was introduced in January 2013 and is still in beta, but many people (including myself) have been using it exclusively for some time now. The big change, at least internally, between versions 2 and 3 was the move from python 2.6 to 3.3 for the plugin framework. This meant that a lot of plugins had to be updated before they would work in Sublime Text 3, which slowed adoption considerably. But now almost all of the most popular plugins work on version 3. I recommend using version 3, especially if you're just getting started. Even if you're currently a happy version 2 user, you should give version 3 a shot, as it introduces some great new features such as Goto Definition and Goto Symbol in Project, as well as performance improvements.

### Notable Features

**Speed** - Super-fast startup, and handles large repositories without breaking a sweat.

**Hot Exit** - You can quit at any time and all your windows and files–whether they've been saved or not–will be magically restored. This particularly nice if you like to have "scratch pads" that you don't want to bother saving.

**Multi-Selection** - You can have multiple selections and cursors, which makes block selection and mass editing a breeze. See *Essential Commands* below

**Autocompletion** - Autocompletes based on other strings in your document.

**Extensible** - Comprehensive Python API makes adding functionality through plugins (packages) easy. See *Using Packages* and *Writing Plugins* below.

## <a name="installation"></a> Installation

1. Download and install Sublime Text 3 for your platform from <http://www.sublimetext.com/3>. If you're already a registered user and want to live on the [not all that sharp] edge, you can [grab the dev build](http://www.sublimetext.com/3dev).

2. [Install Package Control](https://sublime.wbond.net/installation). This is the de facto package manager for Sublime.

3. [Install the command line tool](https://www.sublimetext.com/docs/3/osx_command_line.html). This will allow you to open files or directories directly from the command line, as well as use Sublime Text as your default `EDITOR` for git, etc.

First, create a symlink from a directory in your `PATH` to the subl executable (NOTE: the path below is specific to OS X. Adjust as appropriate for Linux.)

    ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" ~/bin/subl

If you don't already have ~/bin in your `PATH`, you can add it by adding this to your `.bashrc` file in your home directory:

    export PATH=~/bin:$PATH

If you'd like to use Sublime Text as the default unix editor, add this to your `~/.bashrc` file as well:

    export EDITOR='subl -w'

To load these changes in your current terminal session, run:

    source ~/.bashrc

Now you should be able to run `subl <filename>` or `subl <directory>` to open a file or entire directory in Sublime Text.

## <a name="basic-navigation"></a> Basic Navigation

The standard Mac navigation works in Sublime:

* Go to beginning of line (⌘←, ⌃A)
* Go to end of line (⌘→, ⌃E)
* Go to beginning of document (⌘↑)
* Go to end of document (⌘↓)
* Go to next word (⌥→)
* Go to previous word (⌥←)

And a few Sublime-specific:

* Go to next/previous tab (⌥⌘→, ⌥⌘←)
* Go to tab #n (⌘1..⌘9)

## <a name="essential-commands"></a> Essential Commands

**Command Palette... (⇧⌘P)** - Allows you to execute commands by name. Includes both default commands and those added by plugins.

**Goto Anything... (⌘T or ⌘P)** - Fuzzy file search. Just start typing part of the name of the file you're looking for. The contents of the currently selected file will appear in the view.

**Goto Symbol... (⌘R)** - Jump to a "symbol" in the current file. What constitutes a symbol varies by filetype. In a PHP file, it will show classes and functions. In a Markdown document, headers.

**Goto Symbol in Project... (⇧⌘R)** - Jump to a symbol anywhere in your project. This is the fastest way to jump to a particular class or function anywhere in your project.

**Go to Definition... (⌥⌘↓)** - Hitting this while your cursor is on a function will jump to the place in your project where that function is defined. Sublime isn't doing anything too fancy here, just looking for places in your project where a function of that name is defined. If the function is unique in your project, you'll jump right to the definition. If it has a more common name, you'll get a list of possible files that you can scroll through to look for the one you want.

**Jump Back/Forward (⌃-, ⇧⌃-)** - After moving to a different location in your file or project, you can jump back to where you were last with ⌃- (ctrl-dash) and forward again with ⇧⌃- (shift-ctrl-dash).

**Expand Selection to Word (⌘D)** - This is a very quick way to select identical blocks of text in your document and edit them in bulk. For example, let's change all instances of the variable `frozzle` to `frizzle`:

    def frobinate(foo):
        frozzle = Frobinator(foo).frozzle
        frozzle.bar = 42
        for baz in frozzle.doodads:
            print baz

Double-click the first 'frozzle', then hit ⌘D three times, then type 'frizzle'.

**Quick Find All (⌃⌘G)** - An even faster way to select and edit in bulk. Select the term you want to change, and then ⌃⌘G to select all matching terms in the doc.

**Split Into Lines (⌘⇧L)** - Select and edit multiple lines at once. Let's fix the block below by getting rid of the line numbers and adding commas to make it valid JSON:

    1  {
    2    'this': 'json'
    3    'has': 'line numbers'
    4    'but': 'is'
    5    'missing': 'commas'
    6  }

Select all the lines, then hit ⌘⇧L. Hit ⌃A to move the cursor(s) to the beginning of the line(s). Now hold down shift and hit right-arrow until you're flush with the brackets, and hit delete. Hit escape to get out of column mode (or click somewhere with your mouse). Now select lines 2-4 `'this': 'json'` through `'but': 'is'`), hit ⌘⇧L again, then ⌃E to jump to the end of the line, and type a comma. Hit escape.

You can also use ⌃⇧↑ and ⌃⇧↓ to select lines one at a time in column mode, or hold down the option key and drag with your mouse.

Bonus tip: hold down ⌘ and click with your mouse to create multiple cursors.

**Finding & Replacing (⌘F, ⌥⌘F, ⇧⌘F)** - Simple searching within your current file is done with ⌘F. To step through matches, use ⌘G. For searching and replacing within your entire project, use ⇧⌘F.

You can search and replace within the current file with ⌥⌘F, but even faster is to do a regular find with ⌘F, then hit ⌥↩ (alt-return) to select all instances of your search term. You can then edit them all at once.

Even faster still is to select the term you want and then hit ⌃⌘G to select all matching terms in your text.

On the left side of the find pane are buttons that allow you to enable regular expressions, case sensitivity, and whole word matching. One of the nice things about the regular expression search is that it shows you what your regex is currently matching as you type.

The last two buttons, Show Context and Use Buffer, you'll probably want to leave enabled.

## <a name="preferences"></a> Preferences and Customization

Sublime is very customizable, but not through the usual point-and-click dialog that you might be used to. Sublime preferences are stored as JSON files. Look at *Preferences > Settings - Default* to see some of the options that are available to you. If you find something you'd like to change, don't do it in that file, but rather in *Preferences > Settings - User*. (The default settings are stored within the application itself, so any changes there will be lost when you upgrade; user settings are stored in your home directory.)

Some of my favorite preference changes:

```javascript
"highlight_line": true,
"rulers": [80, 100],
"translate_tabs_to_spaces": true,
"scroll_past_end": true,  // not actually a change from the default, but necessary on OSX
```

Plugin-specific preferences can be found under *Preferences > Package Settings*.

### Customizing Key Bindings

Key bindings can be configured in the same way that preferences are. Default key bindings can be found in *Preferences > Key Bindings - Default*. Put any changes in *Preferences > Key Bindings - User*. My favorite changes:

```javascript
 { "keys": ["ctrl+shift+r"], "command": "reveal_in_side_bar"},
 { "keys": ["ctrl+super+s"], "command": "delete_trailing_spaces" }
```

### Fonts and Color Schemes

Although both font and color schemes settings can be found in the Preferences JSON file, you can change those via the menu as well: *Preferences > Font* and Color Scheme.

If none of the included color schemes float your boat, there is no shortage of others to be found. [Color Sublime](http://colorsublime.com/) is a good place to look.

### Syntax-Specific Settings

Sublime has the ability to apply different settings based on the type of file you have open. Just open up a file of the type you want, and go to *Preferences > Settings - More > Syntax Specific - User*. This will open up a .sublime-settings file for that file type. Any preferences you put in here will now apply to any files of that type.

One nice use of this is to have different color schemes for different file types, which, among other things, makes them easier to pick out in your tabs when you have a lot of files open. As an exercise, let's change the color scheme for our sublime-settings files (which are just JSON files):

1. Open a .sublime-settings file (say *Preferences > Settings - Default*)
2. Go to *Preferences > Settings - More > Syntax Specific - User* and it will open up a blank JSON.sublime-settings file
3. Add the following:

```javascript
{
    "color_scheme": "Packages/User/Dawn (SL).tmTheme"
}
```

4. Save and the color scheme of your file should change.

You can also change the font, tab settings...anything that you find in the normal preferences file.

### Vintage

Sublime Text 3 comes with a vi mode editing package called Vintage that is disabled by default. You can enable it by going to *Preferences > Settings - Default* and editing the `ignored_packages` setting to remove "Vintage".

There's also a plugin called [Vintageous](http://guillermooo.bitbucket.org/Vintageous/) that purports to be a more faithful emulation of Vim.

[More information in Vintage can be found in the docs](http://www.sublimetext.com/docs/3/vintage.html).

## <a name="misc-features"></a> Miscellaneous Features

### Distraction Free Mode

Full screen and no UI chrome. Ahhhh. ⌃⇧⌘F

### Projects and Workspaces

A project allows you to group sets of files or directories together. Just add the files/folders you need and then go to *Project > Save Project As...*. You can switch between projects using ⌃⌘P.

A workspace is a particular view—i.e. set of open files, or pane layout—of a project. One use case for having multiple workspaces in a project would be if you were working on different features. You switch between workspaces the same way you switch between projects.

### Layouts and Groups

You can split your window into multiple panes, either horizonal or vertical, via *View > Layout*. You can also do this via *View > Group*. A group is just a set of files in a particular pane. You can move files over to different panes either by dragging their tabs, or via *View > Move File to Group*

### Snippets

*Tools > New Snippet*

Uncomment the `tabTrigger` line and save into `Packages/User` as `hello.sublime-snippet`

Open any document and type `hello<TAB>`

### Build Systems

Build systems (under the Tools menu) allow you to run external programs and view the results within Sublime. See
<http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/build_systems.html> for documentation.

### Bookmarks

Bookmarks allow you to bookmark individual lines in a file and then jump between them. The default key bindings are around the F2 key, which is a pain on Macs, so I've remapped them:

```javascript
{ "keys": ["super+b"], "command": "next_bookmark" },
{ "keys": ["shift+super+b"], "command": "prev_bookmark" },
{ "keys": ["super+alt+b"], "command": "toggle_bookmark" },
{ "keys": ["super+alt+shift+b"], "command": "clear_bookmarks" },
{ "keys": ["alt+b"], "command": "select_all_bookmarks" },
```

That said, I never actually use bookmarks. They might be more useful if they worked across views, but they don't.

## <a name="plugins"></a> Using Packages (Plugins)

One of Sublime's strongest suits is its plugin system. Thousands of plugins have been written, and finding and installing them is painless thanks to another plugin, [Package Control](https://sublime.wbond.net/). If you haven't already installed it, [do so now](https://sublime.wbond.net/installation).

The [Package Control site](https://sublime.wbond.net/) is the easiest way to discover new plugins. Installing a package is just a matter of bringing up the Command Palette (⇧⌘P) and start typing "Install Package", then start typing the name of the package you want.

Which packages you install is obviously going to depend on your needs. Some of my more commonly-used ones are:

**[Sublime SFTP](http://wbond.net/sublime_packages/sftp)**
- Essential for working on remote repositories (see "Working with Remote Files" below)

**[sublime-github](https://github.com/bgreenlee/sublime-github)**
- Shameless plug. Adds commands for interacting with GitHub. Most useful is the ability to create gists easily. Works with GitHub Enterprise.

**[SublimeLinter](https://sublime.wbond.net/packages/SublimeLinter)**
- This is a base plugin for a bunch of different linters for different languages. I currently have the jshint, ruby, and pylint linters installed. (I tried phplint, but it finds way too many issues with the Etsy codebase.)

**[JSFormat](https://sublime.wbond.net/packages/JsFormat)**
- This is super helpful if you have a blob of JSON (say from an AJAX response) and you want to make it readable.

**[Emmet LiveStyle](http://livestyle.emmet.io/)** - Live editing of CSS in Sublime. Pretty much magic.

**[DocBlockr](https://github.com/spadgos/sublime-jsdocs)** - Documentation block creation for JS, PHP, and more.

**[FileDiffs](https://github.com/colinta/SublimeFileDiffs)** - Shows diffs between the current file, or selection(s) in the current file, and clipboard, another file, or unsaved changes. Can be configured to show diffs in an external diff tool.

**[SublimeCodeIntel](https://github.com/SublimeCodeIntel/SublimeCodeIntel)** - Adds intelligent code completion, among other things.

## <a name="console"></a> The Console

Hit Ctrl-\` to bring up the console. This not only acts as a log file for the editor, but is a Python REPL with access to the sublime environment.

Try this in the console:

```python
sublime.message_dialog("Hello, world!")
```

The console is the first place you should look if something funky is going on with Sublime. Any errors will show up there.

### <a name="remote-files"></a> Working with Remote Files

At Etsy, most of our development work is done on virtual machines, so Sublime needs to be configured to work with remote files. There are a few ways to do this. Which one you choose will depends on your needs and how fast the connection to your remote server is.

### NFS (or sshfs/Fuse) Mount

If you already have a NFS or other type of remote filesystem mount set up, this is probably the easiest route, although performance can be terrible, especially with large repositories or over a slower connection.

### rmate/rsub

rsub is a Sublime port of TextMate's rmate command, which allows you to initiate opening a file on a remote machine, and have it open up in Sublime. It accomplishes this through a bit of ssh tunnelling wizardry. Setup is not hard, but contains enough steps that I'll just [link to it instead of reproducing it here](http://log.liminastudio.com/writing/tutorials/sublime-tunnel-of-love-how-to-edit-remote-files-with-sublime-text-via-an-ssh-tunnel).

One major limitation with this method is that it only works for editing individual files, not opening up whole directories/repositories.

### SFTP + rsync

This is my preferred method of working with a large remote repository. The basic idea is you use the rsync command to create a copy of the remote repository on your local machine, and then use the [Sublime SFTP plugin](http://wbond.net/sublime_packages/sftp) to make sure that any changes you make locally get automatically synced over to the remote machine.

The tricky part of using this method is you have to make sure that any time changes are made to your remote repository that didn't originate from your local machine—for example, if you git pull or change branches–you have to run the rsync script to pull the changes over to your local machine. If you don't, you might be editing old versions of files, and if you make changes to them and save, you'll end up overwriting the newer versions on your remote machine, and you'll likely have a mess to unravel.

One way to automate syncing a bit–at least when working with git repos–is to install a post-checkout hook that uses an ssh tunnel to automatically rsync the changes back to your local machine.

#### Setup

1. Install the [Sublime SFTP plugin](http://wbond.net/sublime_packages/sftp).

2. Create your sync script. Mine is called `sync_vm` and looks something like this (Etsy-specific stuff edited out):

```sh
#!/bin/sh
REMOTE_DIR=username@some.server.com:development/
LOCAL_DIR=~/workspace/somewhere
rsync -avz --delete --exclude 'sync_vm' --exclude 'sftp-config.json' \
 --exclude '.git' --exclude '.tags' --exclude '*.jar' \
 --exclude '*.class' --exclude "*.gz" --exclude 'target/' \
 $REMOTE_DIR $LOCAL_DIR
```

**WARNING:** note the `--delete` flag, which will delete anything in your local directory that does not exist in the remote directory, so be careful where you point this.

Put the sync script in the directory you set `LOCAL_DIR` to above, and make it executable: `chmod a+x sync_vm`.

3. For each directory/repository that you'd like to use Sublime SFTP on, you need to have an [sftp-config.json](http://wbond.net/sublime_packages/sftp/settings) file in the root directory of that directory on your local machine.

4. If you're using this with git, you need to make sure that all git operations are done on your remote server (since you don't have a real git repository on your local machine), and that you run your sync script whenever you do a git pull, change branches, cherry-pick, etc.–any changes that are initiated on the remote machine rather than your local machine.

#### Extra Credit

You can install per-repository post-checkout hooks on your remote machine to automatically rsync back to your local machine whenever you pull or change branches.

On your local machine:

* Install [autossh](http://www.harding.motd.ca/autossh/). On OS X, with [Homebrew](http://brew.sh/), just: `brew install autossh`
* `autossh -f -M 0 -N -R 1122:localhost:22 <username>@<remote.host.com>`

On your remote machine:

* Copy the following to `.git/hooks/post-checkout` in any repository you want to auto-sync. Note that you'll need to edit the rsync line:

```sh
#!/bin/bash

# the username on your laptop
REMOTE_USER=brad
# the directory on your laptop where this repository should be synced
REMOTE_DIR=workspace/somewhere/MyRepo

# get directory of this script
MYDIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
rsync -az -e 'ssh -p 1122' --delete <all the excludes from your above sync script> "$MYDIR/../.." $REMOTE_USER@localhost:$REMOTE_DIR &
```

* Make it executable: `chmod a+x post-checkout`

* Copy the contents of the public key from your remote machine (e.g. `~/.ssh/id_dsa.pub` or `id_rsa.pub`) and append it to `~/.ssh/authorized_keys` on your local machine.

* On the remote machine: `ssh -p 1122 <your-local-username>@localhost` (this is just so get the host key of your local machine into your known_hosts file)

Note that you still need to be aware of events that update your repo that don’t trigger the post-checkout hook (e.g. git cherry-pick or git stash pop). In this case, just run the sync script on your laptop and Bob’s your uncle.

## <a name="writing-plugins"></a> Writing Plugins

Four types of plugins:

  - Application commands
    - inherits from sublime_plugin.ApplicationCommand
    - instantiated once
    - example: IncreaseFontSizeCommand
  - Window commands
    - inherits from sublime_plugin.WindowCommand
    - instantiated once per window
    - window is accessible via self.window
    - example: GotoDefinitionCommand
  - Text commands
    - inherits from sublime_plugin.TextCommand
    - instantiated once per view
    - view is accessible via self.view
    - receives an `edit` object, which gives you access to the selection
    - example: UpperCaseCommand
  - Event listeners
    - inherits from sublime_plugin.EventListener
    - see <http://www.sublimetext.com/docs/3/api_reference.html#sublime_plugin.EventListener> for a list of events
    - Don't get too fancy here; you'll probably bog down the whole editor

### Anatomy of a plugin

The standard Sublime Text plugins are in `/Applications/Sublime Text.app/Contents/MacOS/Packages/Default.sublime-package`. Let's peek in there.

A .sublime-package file is just a zip file (with no top-level directory, so be careful where you unzip it!):

```sh
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
mkdir Default
cd Default
unzip /Applications/Sublime\ Text.app/Contents/MacOS/Packages/Default.sublime-package
subl .
```

Let's look at `duplicate_line.py`:

```python
import sublime, sublime_plugin

class DuplicateLineCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        for region in self.view.sel():
            if region.empty():
                line = self.view.line(region)
                line_contents = self.view.substr(line) + '\n'
                self.view.insert(edit, line.begin(), line_contents)
            else:
                self.view.insert(edit, region.begin(), self.view.substr(region))
```

In `Main.sublime-menu` you can see where these commands are called:

```javascript
{
    "caption": "Edit",
    "mnemonic": "E",
    "id": "edit",
    "children":
    [
        ...
        {
            "caption": "Line", "mnemonic": "L",
            "id": "line",
            "children":
            [
                ...
                { "command": "duplicate_line" },
```

In `Default (OSX).sublime-keymap` you can see the keybinding:

```javascript
{ "keys": ["super+shift+d"], "command": "duplicate_line" },
```

This command has no Command Palette entry, but if it did, you'd find it in `Default.sublime-commands`. Here's the entry for the Upper Case command:

```javascript
{
    "caption": "Convert Case: Upper Case",
    "command": "upper_case"
},
```

(Exercise: Add the Duplicate Line command to the Command Palette.)

### Let's Write a Plugin: Say

Note: This will only work on OS X.

1. *Tools > New Plugin...*

2. Make it look like this:

```python
import sublime, sublime_plugin
import subprocess

class SayCommand(sublime_plugin.TextCommand):
    def run(self, edit):
        for region in self.view.sel():
            if not region.empty():
                subprocess.call(["/usr/bin/say", self.view.substr(region)])
```

3. Save as "say.py" in a new folder, "Say", in the Packages directory.

4. Add it to the Command Palette: create a new file:

```javascript
[{
    "caption": "Say: Speak Selection",
    "command": "say"
}]
```

5. Save this as `Say.sublime-commands` in the same directory as `say.py`. Now select some text, hit Cmd-P and type "Say", and our command should appear.

If your command does not appear, chances are there was an error either in your plugin or in the .sublime-commands file. Open the console with Ctrl-\` and look for errors there.

One problem you might have noticed is that Sublime blocks while the Say command is running. Plugins run in the main event loop by default, so when writing plugins, care should be taken to avoid blocking operations. If you do have a blocking operation, you should spawn a new thread for it. Sublime Text 3 introduced a new function that makes this easy: `sublime.set_timeout_async(callback, delay)`. This runs the callback in a separate thread after the given delay. Let's tweak the last line of our plugin:

```python
sublime.set_timeout_async(lambda: subprocess.call(["/usr/bin/say", self.view.substr(region)]), 0)
```

Now if you run Say on a large block of text, you'll find the editor is still responsive while the text is being spoken.

### Saving and Loading Preferences

```python
settings = sublime.load_settings("MyPackage.sublime-settings")
some_settings = settings.get("some_setting", "default value")
settings.set("some_setting", new_value)
settings.erase("some_setting")
sublime.save_settings("MyPackage.sublime-settings")
```

## <a name="resources"></a> Resources

**[Sublime Text 3 Documentation](https://www.sublimetext.com/docs/3/)** - The official documentation. Rather sparse.

**[Sublime Text Unofficial Documentation](http://docs.sublimetext.info/)** - Much more comprehensive documentation. Still lacking in parts, and can be a bit cheeky, but the best place to go in general.

**[Sublime Text Forum](https://www.sublimetext.com/forum/)** - If you're really stuck, head to the forums. There's a good chance someone has already hit your problem, and if not, post something and you'll probably get an answer relatively quickly.

**[@SublimePackages](https://twitter.com/SublimePackages)** - A good way to stay on top of new plugins.
