---
id: 139
title: 'Using the &#8216;after_party&#8217; gem with Capistrano 3'
date: '2015-06-03T22:28:27-07:00'
author: Martin
layout: post
guid: 'https://martinwood.org/?p=139'
permalink: /using-the-after_party-gem-with-capistrano-3
categories:
    - Programming
    - Ruby
---

One of my long-term client projects involves frequent data migrations (as opposed to the classical schema migrations one typically runs).

Up until recently I’ve been creating standalone rake tasks to perform the data migrations and executing them manually when deploying to Staging and Production.

Of course, the dangerous word in the above paragraph is ‘manually’.

This isn’t too much of an issue for Staging as I’m coming fresh from developing the rake task and it at the forefront of my mind. No, the real danger is that by the time Staging gets shunted over to ProductionÂ I’ll invariablyÂ Â forget to run the data migration, regardless of however lurid the colour I mark the task with Â in a Trello label.

Enter [the ‘after\_party’ gem](https://github.com/theSteveMitchell/after_party)Â by Steve Mitchell.

With this fantastic gem I still get to write the migrations as tried and tested rake tasks, but these tasks are registered in a ‘task\_records’ table, similar to schema migrations when executed, so they are only launched once.

The only issue I’ve had is integrating it with Capistrano 3. For those with similar struggles below is how my config/deploy.rb file is setup.

```
<pre class="ruby" name="code">
namespace :deploy do

  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      # Your restart mechanism here, for example:
      execute :touch, release_path.join('tmp/restart.txt')
    end
  end

  after :publishing, :restart

  after :restart, :clear_cache do
    on roles(:web), in: :groups, limit: 3, wait: 10 do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :rake, 'after_party:run'
        end
      end
    end
  end
end
```