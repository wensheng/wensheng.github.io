# Steps

## Install dependencies

    sudo apt-get install ruby2.0 ruby2.0-dev
    sudo gem2.0 sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
    sudo gem2.0 install bundler

## Clone repo and install gh-pages

    git clone git@github.com:wensheng/wensheng.github.io.git myblog
    cd myblog
    bundle install

## Create post and publish

    vi _posts/date-title.md

## Serve locally

    bundle exec jekyll serve
    # check blog at localhost:3000, if all good, proceed to next step

## Create post and publish

    git add _posts/date-title.md
    git commit -a -m 'comment'
    git push
