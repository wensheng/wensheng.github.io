# Steps

## Install dependencies

Ubuntu >16.04 come with ruby 2.3):

    sudo apt install ruby ruby-dev libffi-dev libz-dev libxml2-dev
    gem install bundler --user-install
    export PATH=/home/wensheng/.local/share/gem/ruby/3.0.0/bin:$PATH
    export GEM_HOME=/home/wensheng/.local/share/gem/ruby/3.0.0/gems

## Clone repo and install gh-pages

    git clone git@github.com:wensheng/wensheng.github.io.git myblog
    cd myblog
    bundle install

## Create post and publish

    vi _posts/date-title.md

## Serve locally

    bundle exec jekyll serve
    # check blog at localhost:4000, if all good, proceed to next step

## Create post and publish

    git add _posts/date-title.md
    git commit -a -m 'comment'
    git push
