---
id: 28
title: 'git svn clone/fetch yet no files? This might help&#8230;'
date: '2010-06-07T06:36:29-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=28'
permalink: /git-svn-clonefetch-yet-no-files-this-might-help
categories:
    - Programming
---

I’ve just spent some time wondering why a “git svn clone &lt;repo&gt;” followed by a “git svn fetch” was producing no files in the target folder.

This was puzzling, as the Subversion repository was upto version 423, and poking through the .git metadata I could see that the version information and URL of the SVN repo was all present and correct, but yet it would stubbornly refuse to pull down any files, merely presenting the following message :

\[sourcecode language=’bash’\]  
Initialized empty Git repository in /Users/martin/src/projectname/.git/  
\[/sourcecode\]

Long story short – this particular repository had a non-standard layout where there was no trunk subfolder, so adding the -T option to specify the trunk location as the root like so :

\[sourcecode language=’bash’\]  
git svn clone svn+ssh://user@example.com/path/to/svn/project -T  
\[/sourcecode\]

did the trick and the files started flowing again.