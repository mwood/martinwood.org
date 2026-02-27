---
id: 182
title: 'Managing multiple projects with tmuxinator'
date: '2020-02-26T11:12:48-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=182'
permalink: /managing-multiple-projects-with-tmuxinator
spacious_page_layout:
    - default_layout
categories:
    - Freelance
    - Productivity
    - Programming
    - Ruby
---

Being a freelancer I get to work on several projects at a time.

When I started out this sometimes mean’t juggling several projects in a single day, but for my sanity’s sake I moved over a single project per day (and daily billing) soon after.

This still means I’m usually juggling four to five, usually Ruby on Rails, projects per week. Naturally client’s wouldn’t be pleased if I spent a significant proportion of each day (re)setting my environment up in the mornings.

Enter **tmuxinator**.

What is it? [tmuxinator](https://github.com/tmuxinator/tmuxinator) is great little tool that handles tmux session with multiple panes/windows with ease.

Now [tmux](https://en.wikipedia.org/wiki/Tmux) on it’s own is great – being able to suspend and resume terminal sessions, like the screen command before it, is already a massive boost for a CLI guy like me, but being able to configure those terminal sessions, with the exact windows/panes I want, and to be able to start them with a single command is *life-changing!*

With tmuxinator installed I start my working day by simply typing :

$ mux &lt;project-name&gt;

and it will create (or resume) a tmux session with my preferred shell workspace settings.

The thing I love most about tmuxinator is that it so easy to setup – each project is simply a YAML file stored in ~/.tmuxinator.

For my Rails projects they all follow the same pattern as shown below :

[![](https://martinwood.org/wp-content/uploads/2020/02/rbda6c35vs2DGL0ZfEqCqWU1-580x309.png)](https://martinwood.org/wp-content/uploads/2020/02/rbda6c35vs2DGL0ZfEqCqWU1.png)

A brief summaryÂ of the fields:

**project\_name** – the name of the tmux session, I usually like to include the port number of my local web server for the project here

**project\_root** – what directory to start the session in

**windows** – here’s where the magic begins, this begins the definitions on what tmux windows the session will launch with. Under this field are a list of the eight windows I have for all my Rails projects, and the commands they will be invoked with :

**ed** – launches with vim (what else?)

**sh** – the main shell window I use with the project, which does a ‘git status’ command on startup so know the state of my working folder, e.g. anything I didn’t check-in or complete during the last session.

**db** – my interactive database session, used to be simply ‘rails db’ but I’ve switched in the last year or so to using ‘pgcli’ for some more advanced features.

**srv** – where I start my Rails server

**logs** – a tail of everything under the log folder

**c** – always good to have an interactive Rails console at hand

**zzdebug** – a long time ago I, at the recommendation of a colleague, I got in the habit of adding any temporary log/trace lines with the prefix ‘ZZDEBUG’ meaning I can quickly grep for them. I still use this habit to this day and this window tails my main development log and scans for any lines that match the ‘ZZDEBUG’ keyword.

**remote** – here’s where I have an open SSH session to the server that the code is typically deployed to

…and that’s it. Note that I keep myÂ window names, e.g. ‘ed’, ‘db’ and ‘c’ typically short to preserve screen estate, as all these windows are displayed at the bottom of the tmux session horizontally :

[![](https://martinwood.org/wp-content/uploads/2020/02/w4wMORn7Bm9b2kisLg89HUA2-580x36.png)](https://martinwood.org/wp-content/uploads/2020/02/w4wMORn7Bm9b2kisLg89HUA2.png)

You’ll also see that each window has it’s own numeric prefix, which I can use to change to using tmux’s switch window commands/shortcuts, however as these typically involve typing Ctrl-B followed by the number, it can be a game of finger Twister due to the frequency that I swap between windows, so I’ve therefore setup [Keyboard Maestro](https://www.keyboardmaestro.com/main/) commands to I can just press Apple-&lt;number) instead, slightly delaying my date with RSI.

[![](https://martinwood.org/wp-content/uploads/2020/02/po1JVGxsEBodZVi9LE2kzrmv-580x339.png)](https://martinwood.org/wp-content/uploads/2020/02/po1JVGxsEBodZVi9LE2kzrmv.png)

Additionally I’ve also created Keyboard Maestro commands to do a tmux find command and quickly toggle between the last used window.

(I don’t tend to use the tmux panes feature, where you can split a window into multiple independent panes, I’m more of a one-window one-focus kind of guy, but this is easily possible with tmuxinator if required).

With this setup I get several benefits:

- instant setup of environment
- consistent windows/shortcuts between projects
- easy setup of additional projects (just copy an existing YAML file, and edit as appropriate, taking less than a minute)

What’s missing? Obviously support for any GUI tools, web browsers, etc. Technically I could start these as part of a window’s startup but as I sometimes SSH into my development box from a laptop, starting these applications remotely is probably not what I want.

So there you have it – tmuxinator (and tmux) are somewhat hidden gems and a definite boost to my productivity, starting the day with a single command instead of corralling everything into place manually must save me (and my clients) around an hour a week all told, so well worth looking into if you frequently switch projects like me.