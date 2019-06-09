# web: bundle install && bundle exec jekyll build && bundle exec thin start -p$PORT -V
web: bundle exec puma -t 8:32 -w 3 -p $PORT
console: echo console
rake: echo rake