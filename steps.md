# Steps

## Install dependencies

    sudo apt-get install ruby2.0 ruby2.0-dev
    sudo gem2.0 sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
    sudo gem2.0 install bundler

## Clone repo and install gh-pages

    git clone git@github.com:wensheng/wensheng.github.io.git myblog
    cd myblog
    bundle install

## Serve locally

    bundle exec jekyll serve
