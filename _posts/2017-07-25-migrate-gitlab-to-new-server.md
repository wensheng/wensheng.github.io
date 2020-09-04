---
layout: post
title: Migrate Gitlab to New Server
ghissueid: 8
---

I recently migrated my [Gitlab](https://about.gitlab.com/) to a new server, here are the steps and gotchas that I took notes for future references.

The source and target server need to run the same version of Gitlab.

back-up

restore

change /etc/gitlab/gitlab.rb,

change 8080, to 8082

change
