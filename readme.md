# Dotfiles

What I do have are a very particular set of dotfiles, dotfiles I have acquired over a very long career. Dotfiles that make unix less of a nightmare for people like me.

## Features

* colourful prompt, directory listing, `man` pages and `git` output
* 2-line prompt with `git` status, clock and command syntax highlighting
* works on OS X, Linux and Cygwin - and with both GNU and BSD core utilities
* shell history from `↑`/`↓` keys is from the current shell, `^r` is from all (`zsh` only)
* stops `play` and `ant` from being unruly citizens on OS X

## Requirements

* `git`
* **Python 2.6/2.7 and 3.x**
* **Homebrew**  
  _OS X Only - This means you need Xcode installed first_
* `reattach-to-user-namespace`  
  _OS X Only - Allows lots of things to work better inside `tmux`_
* `zsh-syntax-highlighting`
  _Only under `zsh` - provides syntax highlighting for the `zsh` prompt_
* `ncurses`  
  _If this isn't installed, the `clear` command is missing_
* `Regexp::Common` version `2013031301` or newer  
  _Perl module used for `ipgrep` and `ipls` commands. Older versions do not suport IPv6 and will break._
* **DejaVu Sans Mono**  
  _Cygwin Only - I use this font in MinTTY, and it'll fall back to Courier if it's not installed_

### Supported Shells
* **zsh 4.3** or newer  
  _This is not a hard requirement, however, this version is the one supplied with OS X 10.8_
* **Bash 3.2** or newer  
  _Sorry, MinGW users_

The focus of future updates to these dotfiles will be `zsh`. At some point, `bash` support may be removed or reduced, but only once the environments I use consistently have a compatible `zsh` version available.

## Setup

Setup involves a few steps specific to each platform, followed by the "all platforms" steps below.

### OS X

* install [Xcode](https://itunes.apple.com/app/xcode/id497799835)
* install [Homebrew](https://github.com/Homebrew/homebrew)
* `brew install reattach-to-user-namespace`
* (if using `zsh`) `brew install zsh-syntax-highlighting`
* (if using `bash`) `brew install bash-completion`

### Cygwin

Cygwin has limited package management built-in. I recommend downloading [apt-cyg](http://code.google.com/p/apt-cyg/) which gives you a command-line interface for Cygwin's package manager.

* `apt-cyg install openssh ncurses`
* (if using `bash`) `apt-cyg install bash-completion`

### All Platforms

After completing the relevant platform-specific section,

* (if using `zsh`) install [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
* (if using `bash`) install [git-bash-completion](https://github.com/markgandolfo/git-bash-completion)
* `cpan Regexp::Common` (frustratingly, will require some basic interaction if you haven't set up CPAN before)
* `git clone https://gitlab.com/geoffstokes/dotfiles.git ~/dotfiles`
* `~/dotfiles/install.sh`  
  _**NOTE**: If `install.sh` detects that dependencies are missing, it will notify you and abort._
* Restart your shell

## Updating

`profile update` will fetch the latest changes and install them.

## Platform-specific directories

### Single-Platform

This supports three kinds of platform-specific functionality;

1. Platform-specific `bin` directories
2. Platform-specific `~` includes
3. Platform-specific `zshrc` includes

The install script will manage this automatically. You can find these under `dotfiles/platforms`.

* Files in `dotfiles/platforms/{platform}/bin` are automatically available in your `$PATH`.

* Files in `dotfiles/platforms/{platform}/home` are either symlinked (editing `~/{file}` updates `dotfiles/platforms/{platform}/file` - platform-specific includes are only possible if the file is executable) or concatenated (appended; this allows platform-specific stuff to be added to files which aren't executed).

* Files in `dotfiles/platforms/{platform}/zsh` are loaded by `.zshrc` (and thus, only when running `zsh`), in the following order;
    1. `path.zsh` - settings for `$PATH` go here
    2. All files except `path.zsh` and `completion.zsh`, in alphabetical order - general settings and custom   categorisation goes here
    3. `completion.zsh` - completion settings go here

This setup includes a helper function for this called `platformbindir`. Running this will;

* create the platform-specific `bin` directory if it doesn't already exist
* output the full location of the platform-specific `bin` directory

So it's easy to add stuff; you can do stuff like ``cd `platformbindir` ``, which navigates straight to the current platform's `bin` directory.

_**NOTE**: Due to the way this works, it's not possible to update a concatenated file in your home directory and commit it immediately. You will need to make the edit twice for those files._

### All-But-Platform

Additionally, there is provision for such directories usable for all but a certain platform. For example, `tmux` works anywhere except on Cygwin, so `tmux.conf.concat` is placed in `dotfiles/platforms/all-but-cygwin/home`. It then applies on `linux` and `darwin`, but not `cygwin`.

There are no shortcuts for configuring this, however, it relies on the same `$UNAME` variable as the above, so if you had a `linux` directory, the all-but directory is `all-but-linux`.

## Shortcuts and Variables

### Variables

* `$UNAME`: current platform name, in lower case. (In the case of Cygwin, this will always be `cygwin`)

### Aliases

* `cip`: outputs a single IP address for the current system (last line of `ipls` output), usually the one you want - useful for scripts
* `cl`: shortcut to `clear`
* `fl`: shortcut to show the Adobe Flash debug log (OS X Only)
* `ipls`: outputs a list of local IP addresses for the current system (works for both Linux and OS X)
* `mkcd`: make a directory and move into it immediately
* `platformbindir`: outputs the current platform's platform-specific `bin` directory
* `profile`, `p`: utility for updating, editing, installing and loading dotfiles
* `rm!`: shortcut to `rm -rf`
* `title`: set the window title
* `x`, `:q`: shortcut to `exit`

### Utilities

* `chcase`: Utility to change the case of text input. Uses `tr` internally to support multiple character sets.
* `colourtest`: prints out a table of the main colour codes (borrowed from [iTerm 2](https://code.google.com/p/iterm2/source/browse/trunk/tests/colors.sh))
* `fn`: find files under the current directory by name (uses Spotlight's cache on OS X and `find` on other systems)
* `gravatar`: output gravatar URLs for email addresses passed in
* `gz`: output the current size and gzipped size of a file
* `ipgrep`: finds valid IP addresses within input which are not link-local
* `imessage`: (OS X only) sends an iMessage to the specified address/number (`imessage "+61491570156" "This is an iMessage to a fictitious Australian telephone number"`)
* `internet-sharing-ip`: (OS X only) sets the IP range of the Internet Sharing service. Restart sharing after running. (`internet-sharing-ip 172.27.2.0`)
* `keystroke`: (OS X only) sends text to whichever window is in focus (best used with a delay)
* `learn-spelling`: (OS X only) merges a backed up list of spelling words into the OS X user spell checking dictionary
* `mansi`: Colourful `man` output.
* `nuname`: Normalised `uname` command; used to derive the `$UNAME` variable. Lower case for all platforms.
* `osxdefaults`: (OS X only) configure behaviours for the OS X desktop and applications
* `replacements2hotstrings`: (OS X only) export the current OS X Text Replacements as AutoHotKey HotStrings for use on Windows
* `returnOneOf`: pass in a bunch of parameters, it'll randomly echo one of them.
* `shttp`: starts an HTTP server for the current working directory using either Ruby's `httpd` or Python's `SimpleHTTPServer`.
* `simplify`: simplifies fractions when you're too lazy to.
* `sshmux`: `ssh` with automatic remote `tmux` (re)attachment, and fallback to the remote user's default shell.
* `tminus`: prints the time until a unix date passed in.
* `untar`: you can `unzip`, why can't you `untar`?
* `wed`: (Cygwin only) open a file or directory in Sublime Text 2 (if available), otherwise `vim`
* `zdate`: date output which leverages `zsh` date utilities to provide consistent date output with nicer syntax than plain `strftime(3)`. custom formats are supported. built-in templates include;
    * `beats` or `b`: Swatch Internet Time - `@778`
    * `short` or `s`: time only - `9:41 am`
    * `medium` or `m`: time and day - `Tue 9:41 am`
    * `long` or `l`: date, month, day of month and time - `Tue Jan 9 9:41 am`
    * `full` or `f`: date, month, day of month and time - `Tuesday, January 9 2007 9:41:00 am`
    * `8601`, `iso` or `i`: ISO 8601-style format `2007-01-09T09:41:00-0800`

## History

These were originally forked from [seaofclouds](http://github.com/seaofclouds)' [dotfiles repo](https://github.com/seaofclouds/dotfiles), and subsequently heavily modified.  
Lots of inspiration (and some shortcuts and functions) taken from these fine people;

* [Eevee](https://github.com/eevee/rc)
* [Ben Alman](https://github.com/cowboy/dotfiles)
* [Mathias Bynens](http://mths.be/dotfiles)
* [Ryan Tomayko](http://github.com/rtomayko/dotfiles)
* [GitHub does dotfiles](http://dotfiles.github.com/)
