---
layout: post
title:  "Todo/check list for a new jekyll blog"
date:   2013-07-18 19:55:05
description: "A check list for a new jekyll blog, decently responsive and well integrated with google, facebook and twitter."
categories: [web]
image: false
article: yes
tags: [jekyll, blog, responsive, twitter, facebook, google, github, pages, howto, check list,
todo list, best practices]
---

* my toc
{:toc}

#Context 

In order to refresh my web standart knowledge, I decided to open this blog a few weeks ago.
I won't write another howto create a jekyll blog, nor I will enter in the details of adding a
facebook +1 button.

I will just list you all the tasks I had to complete in order to show you the current article,
it was quite time consuming for me (I usually work on backends and I'm really not a front-end
design person).

Anyway, I learn a lot constructing this and wanted to share with you this todo list.
Most of the tasks are not jekyll-centric and can be done for another blogging plateform. 

Feel free to leave me a comment if you like it or if you think I miss something.

I hope this will help you !

# Things to (eventually) do

## Google recommandations

A quick summary of [google search engine optimization guide](http://www.google.com/webmasters/docs/search-engine-optimization-starter-guide.pdf)

* Use [schema keywords](http://schema.org/)
* Use [add rel author links](http://www.vervesearch.com/blog/seo/how-to-implement-the-relauthor-tag-a-step-by-step-guide/)
* Have a clear description of your thing on the front page
* Check page title relevance compare to the article content
* Use accurate meta description tag
* Use accurate page url (according the page content)
* Have a cutom [404 page with a link to the main page + latest 10 articles](http://yizeng.me/2013/05/26/create-a-custom-jekyll-404-page/)
* Use alt attributes for images
* A tags text should be relevant (no [link](/))

## Things I have done

### Going social
* [integrate twitter button](https://dev.twitter.com/docs/tweet-button)
* [twitter cards](https://github.com/jpoehls/hulk-example/blob/master/_posts/2013/2013-02-02-jekyll-recipes-for-blog-meta-tags.md)
* [attach site to facebook account](https://developers.facebook.com/docs/insights/)
* [facebook open graph](http://davidensinger.com/2013/04/adding-open-graph-tags-to-jekyll/)
* [validate opengraph](https://developers.facebook.com/tools/debug)
* [integrate flatter](http://developers.flattr.net/button/)
* [integrate google+](https://developers.google.com/+/web/+1button/)
* [use a gravatar profile](http://fr.gravatar.com/)


### Obey the rules
* [css validation](http://jigsaw.w3.org/css-validator/validator?uri=http%3A%2F%2Fgustavepate.github.io)
* [xhtml validation](http://validator.w3.org/check?uri=http%3A%2F%2Fgustavepate.github.io)
* [rss/atom validation](http://validator.w3.org/feed/check.cgi?url=http%3A%2F%2Fgustavepate.github.io%2Fatom.xml)
* [test / validate twitter cards](https://dev.twitter.com/docs/cards/validation/validator)
* [test social buttons](http://yourlittlehands.com/)

### A bit of seo
* [meta description](http://paradigmatic.streum.org/2011/02/generating-html-meta-data-with-jekyll/)
* [add structred data](http://schema.org/)
* [create a robots.txt](http://www.robotstxt.org/)
* [create a sitemaps.xml](http://www.sitemaps.org/fr/)
* [use permalink](http://jekyllrb.com/docs/permalinks/)
* create an rss/atom feed

### Test site responsivness

* [test responsivness: matt](http://mattkersley.com/responsive/)
* [test responsivness: responsinator](http://www.responsinator.com/?url=http%3A%2F%2Fgustavepate.github.io%2F)

### Other things

* [optimize css](https://github.com/geuis/helium-css)
* [add basic analytics](http://www.google.com/analytics/)
* [use icons](http://zurb.com/playground/social-webicons)
* [use a favicon](http://www.favicon.cc/)
* [add a comment system](http://disqus.com/)


## Things I haven't done

* [implement fallback for old browser](http://modernizr.com/)
* [custom jekyll excerpt](https://coderwall.com/p/eazb7w)
* [show the whole n last post on first page](/ressources/liquid.txt)
* consider creating a rss feed per category
* image specific sitemap.xml

# When everything is ok

Todo when you are ready to show the world your hard work.

* recheck xhtml and css validity
* recheck rich snippets, open graph and twitter card validity
* [submit content to google](http://www.google.com/submityourcontent/)
* [pass the sitemap.xml to google](https://www.google.com/webmasters/tools/home)
* Have a beer !
