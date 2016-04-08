---
layout: post
comments: true
title:  "Moving to Jekyll"
date:   2016-03-15 20:00:00 +0530
category: general
tags: jekyll
---

I initially did set up this blog on this domain using [Wordpress](https://wordpress.com/
), as I have heard of its praise from many online blogs and websites.   
It felt good because it has so many plugins, themes, and a really good CMS. You need to just login and type in their editor. But after a week or so, I decided to ditch it and go for Jekyll.

Here are some of the good reasons that did move me to Jekyll

* No good markdown support in Wordpress.
* Wordpress is not git friendly. Though many tutorials and services do exist like [this](http://stevegrunwell.github.io/wordpress-git), [this](https://revisr.io/) and much more but in the end, it just fell too much to do just for version controlling it.
* It is mainly optimized for dynamic content and therefore slow.
* It is resource heavy. I used [``t2.micro``](https://aws.amazon.com/ec2/instance-types/) AWS EC2 instance to host it, and it was constantly crashing even with a meagre load of 50 clients per second on stress testing.

So I decided to give Jekyll a shot. To host it I used newly released [Gitlab Pages](https://pages.gitlab.io/) as it allows multiple CNAME records and custom SSL certificates which aren't available on GitHub pages (yet).

Setting up Jekyll was easy (though a bit difficult than  Wordpress which just needed to click buttons). There are numerous online guides available, I followed [http://joshualande.com/jekyll-github-pages-poole/](http://joshualande.com/jekyll-github-pages-poole/) with some of my own customizations like setting up tags, categories etc.   
It took around 2-3 hours to set up a working blog. You can check the code here [https://gitlab.com/anchitjain1234/anchitjain1234.gitlab.io](https://gitlab.com/anchitjain1234/anchitjain1234.gitlab.io).

To stress test the Jekyll,  I hosted the blog on a 512 MB RAM [Digital Ocean](https://www.digitalocean.com) droplet and tested it with [``ab``](https://httpd.apache.org/docs/2.4/programs/ab.html) for 20000 requests at 1000 concurrency level and it worked flawlessly (didn't crash ever) even on 512 MB RAM.

Finally, I am pretty happy with Jekyll and Gitlab. I also like this workflow very much, use Sublime Text or Atom to write the post or edit the site and just ``git push`` it to deploy.
