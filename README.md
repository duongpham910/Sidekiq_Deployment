# Sidekiq_Deployment
For deployment for rails 6.0
## Installation

    gem 'capistrano-sidekiq', group: :development

And then execute:

    $ bundle
## Usage
```ruby
# Capfile

require 'capistrano/sidekiq'
install_plugin Capistrano::Sidekiq  # Default sidekiq tasks
install_plugin Capistrano::Sidekiq::Systemd 
```

## Integration with systemd
Set init system to systemd in the cap deploy config:

```ruby
set :init_system, :systemd
```

Install systemd.service template file and enable the service with:

```ruby
bundle exec cap sidekiq:install
```

Default name for the service file is sidekiq-stage.service. This can be changed as needed, for example:

```ruby
set :service_unit_name, "sidekiq-#{fetch(:application)}-#{fetch(:stage)}.service"
```

If check sidekiq.service status have error on server `Neither a valid executable name nor an absolute path: $HOME/.rbenv/bin/rbenv`. Fixing likes below:

```
$ vi /home/lbapp/.config/systemd/user/sidekiq.service
$ Change $HOME -> /home/lbapp
```

After that reload sidekiq.service

```
$ systemctl --user daemon-reload
$ systemctl --user enable sidekiq
$ systemctl --user restart sidekiq.service
$ systemctl --user status sidekiq.service
```

Reference
https://www.rubydoc.info/gems/capistrano-sidekiq/1.0.2#integration-with-systemd
