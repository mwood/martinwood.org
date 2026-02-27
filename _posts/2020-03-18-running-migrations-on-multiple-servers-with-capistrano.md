---
id: 201
title: 'Running migrations on multiple servers with Capistrano'
date: '2020-03-18T11:30:20-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=201'
permalink: /running-migrations-on-multiple-servers-with-capistrano
spacious_page_layout:
    - default_layout
categories:
    - Programming
    - Ruby
---

Typically the sites I work on have a traditional single Staging server, and a single Production server.

Recently I’ve been working on a client project where we needed the same database schema (and subsequent migrations) deployed to multiple target servers.

No problem I thought – I’ll just add the additional servers to the classic Capistrano :app,:web and :db roles :

```ruby
role :app, %w{deploy@srv1 deploy@srv2 deploy@srv3}
role :web, %w{deploy@srv1 deploy@srv2 deploy@srv3}
role :db, %w{deploy@srv1 deploy@srv2 deploy@srv3}
```

…and things will work without a hitch.

Guess what? There was a hitch.

I was surprised to learn that Capistrano **only runs database migrations on the first of the servers in the :db grou**p, i.e. deploy@srv1 and any subsequent ones are ignored.

From what I can gather this has been the case since Capistrano v3.4.0.

To get the migrations to run on all three servers the following lines needed to be added to config/deploy/production.rb :

```ruby
set :migration_role, :db
set :migration_servers, -> { release_roles(fetch(:migration_role)) }
```

After this all servers got the database updates, and I could go back to concentrating on the code and not pesky deployment issues 🙂