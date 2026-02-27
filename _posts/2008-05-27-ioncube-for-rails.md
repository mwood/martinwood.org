---
id: 17
title: 'Ioncube for Rails?'
date: '2008-05-27T12:49:21-07:00'
author: 'Martin Wood'
layout: post
guid: 'https://martinwood.org/2008/05/27/ioncube-for-rails/'
permalink: /ioncube-for-rails
categories:
    - Business
    - mISV
    - Ruby
---

Having just completed my [first mISV product](http://datafeedstudio.com), I’m already starting to think about potential new projects.

Despite Datafeed Studio being written in PHP, it is fair to say it is not my programming language of choice.

Datafeed Studio is a web application that is installed by the end user on their server, thus it made sense to go for for the language that has the biggest support amongst web hosting services.

Of course, now that [mod\_rails / Passenger](http://modrails.com/) has been released, hopefully it wont be too long before Ruby does some catching up in the widespread availability and ease of deployment stakes. I understand that major web hosting providers such as [Dreamhost](http://dreamhost.com) are already offering support.

The second reason I chose PHP is to do with script protection. There are several PHP solutions out there to encode scripts to prevent piracy such as [Zend Guard](http://www.zend.com/en/products/guard/), [Code Lock](http://www.codelock.co.nz/) and [IonCube](http://ioncube.com) (I opted for the latter) but seemingly none for the Ruby world?

My previous Rails projects have been SAAS based so I’ve never had to worry about this – but I have a mISV idea that like Datafeed Studio, would require the customer to install the software on their server, but the lack of script encoding / protection does unfortunately put me off using Rails in this instance.

Has anybody else been in this position? What did you do? Am I worrying too much about piracy concerns?