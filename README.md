personal-website
================

A website for myself, made with [Jekyll](http://jekyllrb.com/), theme by [Inc](https://sendtoinc.com).

Published to the repo [swilson96.github.io](https://github.com/swilson96/swilson96.github.io) and hosted at [swilson.co.uk](http://swilson.co.uk)

To compile and run locally (on default port 4000):
```
$ jekyll server
```

To publish, make sure Rakefile has the correct github repo (it will force-push the built site there):
```
$ bundle exec rake site:publish
```
