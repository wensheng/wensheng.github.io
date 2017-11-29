# Steps

## Install dependencies

activerecord 5 requires ruby >= 2.2.2

(for Ubuntu <16.04):

    sudo apt-get install python-software-properties
    sudo apt-add-repository ppa:brightbox/ruby-ng
    sudo apt-get update
    sudo apt-get purge ruby1.9
    sudo apt-get install ruby2.2 ruby2.2-dev
    ruby2.2 -v  # shows 2.2.5
    #sudo gem2.2 sources --add https://ruby.taobao.org/ --remove https://rubygems.org/  # only if inside china
    sudo gem2.2 install bundler

(Ubuntu >16.04 come with ruby 2.3):

    sudo apt install ruby ruby-dev libffi-dev libz-dev libxml2-dev
    sudo gem install bundler

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
