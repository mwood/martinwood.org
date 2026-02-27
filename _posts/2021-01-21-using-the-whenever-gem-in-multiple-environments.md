---
id: 248
title: 'Using the whenever Gem in multiple environments'
date: '2021-01-21T10:42:05-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=248'
permalink: /using-the-whenever-gem-in-multiple-environments
spacious_page_layout:
    - default_layout
categories:
    - Uncategorized
tags:
    - Programming
    - Ruby
---

In my Rails projects I invariably end up using the fantastic [whenever](https://github.com/javan/whenever) gem to schedule cron tasks a project may require.

With the **whenever** gem one can configure scheduled tasks using its DSL, e.g. :

```ruby
every 1.day, at: '4pm' do
  rake "membership:expire", output: 'log/membership_expire.log'
end
```

which gets converted into a standard UNIX cron entry line such as :

```bash
0 16 * * * /bin/bash -l -c 'cd /home/deploy/production/releases/20200519122408 && RAILS_ENV=production bundle exec rake membership:expire --silent >> log/membership_expire.log 2>&1'
```

upon deployment to the target environment when using the supplied Capistrano task.

One not so obvious feature of the gem is how to restrict certain tasks to only happen in certain environments, e.g. just Production not Staging *and* Production.

For example if you have nightly task on your database which relies on 3rd-party API look-ups (e.g. geocoding users based on their last IP address) or sending out membership expiry emails you probably don’t want to be running those in your Staging environment to save API costs, prevent duplicate or misleading emails being sent to customers if your Staging environment is a subset of your Production data, etc.

Turns out this is easier than you think, you can just use the **@environment** variable inside the config/schedule.rb definition files, so you can have contain a block like :

```ruby
if @environment == 'production'
  every 1.day, at: '4pm' do
    rake "membership:expire", output: 'log/membership_expire.log'
  end
end
```

to only run that task in the production environment – a cron entry will not be created to run for any other environment.

Alternatively you could run things at a different pace on the two environments, e.g.

```ruby
if @environment == 'staging' 
  every :monday, at: '7am' do 
    rake "users:geocode", output: 'log/users_geocode'
  end
elsif @environent == 'production'
  every 1.day, at: '4pm' do
    rake "users:geocode", output: 'log/users_geocode' 
  end
end
``````ruby
<br></br>
```

so that the expensive action still gets done on Staging but at a less frequent interval.

If both of your environments are on the same physical server/VPS then staggering tasks like this could improve performance, especially if you have a task executing say every 15 minutes or less – firing up two heavyweight Rails frameworks at the same time at a regular interval could make less beefy servers chug, so it’s worth taking the extra time to consider (a) do I really need this task executing on Staging and (b) does it have to been as regular as the Production task.