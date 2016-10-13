mina-sidekiq
============

[![Build Status](https://travis-ci.org/Mic92/mina-sidekiq.png?branch=master)](https://travis-ci.org/Mic92/mina-sidekiq)

mina-sidekiq is a gem that adds tasks to aid in the deployment of [Sidekiq] (http://mperham.github.com/sidekiq/)
using [Mina] (http://nadarei.co/mina).

Starting with 1.0.0 this gem requires Mina 1.0! (thanks [@devvmh](https://github.com/devvmh))

# Getting Start

## Installation

```console
gem install mina-sidekiq
```

## Example

```ruby
require 'mina_sidekiq/tasks'
#...

task :setup do
  # sidekiq needs a place to store its pid file and log file
  command %(mkdir -p "#{fetch(:deploy_to)}/shared/pids/")
  command %(mkdir -p "#{fetch(:deploy_to)}/shared/log/")
end

task :deploy do
  deploy do
    # stop accepting new workers
    invoke :'sidekiq:quiet'
    invoke :'git:clone'
    invoke :'deploy:link_shared_paths'
    ...

    on :launch do
      ...
      invoke :'sidekiq:restart'
    end
  end
end
```


## Available Tasks

* sidekiq:stop
* sidekiq:start
* sidekiq:restart
* sidekiq:quiet
* sidekiq:log

## Available Options

| Option              | Description                                                                    |
| ------------------- | ------------------------------------------------------------------------------ |
| *sidekiq*           | Sets the path to sidekiq.                                                      |
| *sidekiqctl*        | Sets the path to sidekiqctl.                                                   |
| *sidekiq\_timeout*  | Sets a upper limit of time a worker is allowed to finish, before it is killed. |
| *sidekiq\_log*      | Sets the path to the log file of sidekiq.                                      |
| *sidekiq\_pid*      | Sets the path to the pid file of a sidekiq worker.                             |
| *sidekiq_processes* | Sets the number of sidekiq processes launched.                                 |

## Testing

The test requires a local running ssh server with the ssh keys of the current
user added to its `~/.ssh/authorized_keys`. In OS X, this is "Remote Login"
under the Sharing pref pane. You will also need a working rvm installation.

To run the full blown test suite use:

```console
bundle exec rake test
```

For faster release cycle use

```console
cd test_env
bundle exec mina deploy --verbose
```

## Copyright

Copyright (c) 2016 Jörg Thalheim <joerg@higgsboson.tk>

See LICENSE for further details.
