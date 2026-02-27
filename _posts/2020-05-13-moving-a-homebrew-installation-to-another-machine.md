---
id: 242
title: 'Moving a Homebrew installation to another machine'
date: '2020-05-13T10:04:41-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=242'
permalink: /moving-a-homebrew-installation-to-another-machine
spacious_page_layout:
    - default_layout
categories:
    - OSX
---

I’ve had my main Mac desktop for development for over 6 years now and it is still going strong.

More surprising is that I haven’t had to reinstall the operating system from scratch during that time, so it has transitioned from 10.4 Mavericks right up to Mojave today (no, I \_still\_ haven’t been brave enough to upgrade to much maligned Catalina).

Over that time I’ve installed numerous (155 to be exact) [Homebrew](https://brew.sh) packages from the command-line and have often worried about how I would cope with the day when I have to switch systems (or possible backup/recovery failures).

Remembering which packages I’ve installed would be impossible – so either I’d have to start from scratch, and install the missing packages as and when I need them, or make use of a relatively new Homebrew feature – the [Brewfile](https://github.com/Homebrew/homebrew-bundle).

The Brewfile, much like a Gemfile in a Rails projects, is a simple plain-text file that lists all the packages that make up the system – which in this case would be everything I have installed via the Brew package manager.

To generate a Brewfile you simply have to enter :

```bash
$ brew bundle dump
```

and one will be created inside the folder you launched the command from.

You can then copy this file to another machine entirely, install Homebrew again, and then enter :

```bash
$ brew bundle install
```

to get all the listed packages installed again on your new system.

Of course, one thing that wouldn’t get copied over are any config or data files you may have created on the original system, so this in **no way an alternative to the traditional backing up of important files**, but as a quick way to get a new system up and running with your favourite tools and utilities it sure beats having to do reinstall each one manually.

In practise, if I did want to migrate to a new machine, I would probably have a good look through the generated Brewfile as a good percentage of the tools are probably things I no longer use now. Of course, it being a beautiful plain-text file you can just delete the entries you no longer require prior to running the install command.

So I’ve always got a recent Brewfile available in case the worst happens, I’ve created a small shell script :

```bash
BREWFILE_BACKUP_LOCATION=~/az/b/Brewfiles
DATE=`date +%d-%m-%Y`

cd /tmp
brew bundle dump
mv Brewfile "$BREWFILE_BACKUP_LOCATION/Brewfile-$DATE"
```

…that will create a Brewfile with the date stamped on it, e.g. Brewfile-13-05-2020 and save it in a backup location.

I set this to automatically run every Sunday at 6am via cron :

```bash
0 6 * * 0 ~/bin/create_brewfile
```

This means every week I get a fresh Brewfile created and stored in the backup folder along with any older ones, which makes it great for diffing changes between weeks too.

So my advice is, if you are a big Homebrew user like myself, generate that Brewfile \*now\* (and ideally regularly as described above) – you’ll thank yourself one day when it comes to setting up your next working environment.