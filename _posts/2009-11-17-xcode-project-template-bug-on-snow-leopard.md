---
id: 24
title: 'XCode Project Template bug on Snow Leopard'
date: '2009-11-17T07:33:42-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=24'
permalink: /xcode-project-template-bug-on-snow-leopard
categories:
    - Ruby
    - Web
---

After a gap of almost six years I’ve been grabbed by the sudden urge to start doing some Cocoa development again.

Rather than get my old battered copy of Hillegass’ seminal [Cocoa Programming for Mac OS X](http://www.amazon.co.uk/gp/product/0321503619?ie=UTF8&tag=mwood-21&linkCode=as2&camp=1634&creative=19450&creativeASIN=0321503619)![](http://www.assoc-amazon.co.uk/e/ir?t=mwood-21&l=as2&o=2&a=0321503619) out of some long forgotten cupboard I’ve been refreshing my fading memory by reading the new [Cocoa Programming](http://www.pragprog.com/titles/dscpq/cocoa-programming) title from the Pragmatic bookshelf, which, although still in Beta, is so far a nice breezy ride (back) into the world of Mac development, and more importantly for me these days, is available as a downloadable PDF – unlike the Hillegass title (sort it out, man!)

Everything was going well until I hit a chapter that claimed that I should have an application delegate class automatically created when starting a new vanilla ‘Cocoa Application’ project.

I didn’t.

Fighting my ‘this is obviously a Beta book problem’ mentality, I slowly discovered that my XCode installation is doing very strange things, even after totally wiping out my current installation and replacing it with the latest download from the Apple site (v3.2.1) creating a new basic ‘Cocoa Application’ would not give me the delegate files (and instead of the newer Xib Interface Builder format, I had the older style Nib format).

Very strange indeed.

Digging deeper into the mysterious world of XCode templates (i.e. poking randomly around the contents of `/Developer/Library/Xcode/Project Templates`) I discovered that yes, my Cocoa Application template folder, which is used as the basis of new projects, looked a bit suspect :

\[sourcecode language=’bash’\]  
drwxrwxr-x 9 root admin 306 May 18 2009 Cocoa Application  
drwxrwxr-x 9 root admin 306 May 18 2009 Cocoa Document-based Application  
drwxrwxr-x 4 root admin 136 Sep 24 2007 CocoaApp.xcodeproj  
-rw-rw-r– 1 root admin 157 Sep 24 2007 CocoaApp\_Prefix.pch  
drwxrwxr-x 10 root admin 340 May 18 2009 Core Data Application  
drwxrwxr-x 11 root admin 374 May 18 2009 Core Data Application with Spotlight Importer  
drwxrwxr-x 10 root admin 340 May 18 2009 Core Data Document-based Application  
drwxrwxr-x 11 root admin 374 May 18 2009 Core Data Document-based Application with Spotlight Importer  
drwxrwxr-x 4 root admin 136 Sep 24 2007 English.lproj  
-rw-rw-r– 1 root admin 849 Sep 24 2007 Info.plist  
-rw-rw-r– 1 root admin 596 May 18 2009 TemplateChooser.plist  
-rw-rw-r– 1 root admin 263 Sep 24 2007 main.m  
\[/sourcecode\]

i.e. what the blazes are those “Core Data…” folders doing there? They are Project Templates in their own right and shouldn’t be within this folder.

Throwing caution to the wind (well, backing up the folder first) I pruned out these folders, plus the “Cocoa Document-based Application one” and the “TemplateChoose.plist” for good measure. Leaving it looking like :

\[sourcecode language=’bash’\]  
drwxrwxr-x 9 root admin 306 May 18 2009 Cocoa Application  
drwxrwxr-x 4 root admin 136 Sep 24 2007 CocoaApp.xcodeproj  
-rw-rw-r– 1 root admin 157 Sep 24 2007 CocoaApp\_Prefix.pch  
drwxrwxr-x 4 root admin 136 Sep 24 2007 English.lproj  
-rw-rw-r– 1 root admin 849 Sep 24 2007 Info.plist  
-rw-rw-r– 1 root admin 263 Sep 24 2007 main.m  
\[/sourcecode\]

and all seems well within the XCode world again.

I’m afraid I have no idea what could have caused this, and the only other reference I can find to the problem is [here](http://telliott99.blogspot.com/2009/10/xcode-templates-messed-up.html) which ties up exactly to what I was seeing, and proves it’s not an isolated incident.

Interested to learn if anybody has had a similar problem, and what the possible cause(s) are as I’m sure if it affected all Snow Leopard / XCode developers it would be more widely known about.