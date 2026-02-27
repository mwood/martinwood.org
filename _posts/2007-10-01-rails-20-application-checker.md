---
id: 15
title: 'Rails 2.0 application checker'
date: '2007-10-01T15:37:09-07:00'
author: 'Martin Wood'
layout: post
guid: 'https://martinwood.org/2007/10/01/rails-20-application-checker/'
permalink: /rails-20-application-checker
categories:
    - Programming
    - Ruby
---

Now that the first release candidate of Rails 2.0 has been [announced](http://weblog.rubyonrails.com/2007/9/30/rails-2-0-0-preview-release), what better time to check if your existing Rails app might need some TLC before the upgrade?

Enter [r2check](http://pastie.caboo.se/private/krcevozww61drdeza13e3a), a small tool which does some regular expression searches against your codebase for things that we know are changing.