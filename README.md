personal-website
================

A website for myself, made with [Jekyll](http://jekyllrb.com/), theme by [Inc](https://sendtoinc.com).

Published to the repo [swilson96.github.io](https://github.com/swilson96/swilson96.github.io) and hosted at [swilson.co.uk](http://swilson.co.uk)

### Install

(For Windows)

Install Ruby, devkit, python and bundler (if you don't already have them). First, install Chocolaty: https://chocolatey.org/
```
$ choco install ruby
$ choco install ruby2.devkit
$ cd c:\tools\DevKit2
$ ruby dk.rb init
# Now add c:\tools\ruby22 to the config.yml file
$ ruby dk.rb install
$ choco install python
$ gem install bundler -v 1.9    # At the moment I'm having a checksum error with the latest version.
                                # http://stackoverflow.com/questions/35315712/rails-initialization-checksum-error
```

Once you have ruby and bundler:
```
$ bundle install
```

To compile and run locally (on default port 4000):
```
$ bundle exec jekyll server
```

To just build (and watch)
```
$ bundle exec jekyll build --watch
```

To publish, make sure Rakefile has the correct github repo (it will force-push the built site there):
```
$ bundle exec rake site:publish
```
