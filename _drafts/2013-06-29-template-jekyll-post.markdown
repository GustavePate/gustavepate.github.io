---
layout: post
title:  Template Post
published: true
tags: [template, meaningless]
description: This should be move to draf and is only an exemple post
categories: [python, linux]
image:  false
article: yes
---

* my toc
{:toc}

## original post


You'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes!
To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.

Jekyll also offers powerful support for code snippets:


{% highlight ruby linenos %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}


Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com

## reminders of markdown syntax

blockquote
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet, consectetuer adipiscing
> id sem consectetuer libero luctus adipiscing.

* page.id : {{page.id}}
* page.url : {{page.url}}
* site url: {{ site.url }}

[Link_to_my_post]({% post_url 2013-07-22-vim-cheat-sheet %})

a python:

![a beautiful python][python_logo_id]
[python_logo_id]: /ressources/python-logo.png "Optional title attribute"

a link to the python [link][python_logo_id]

a list:
* one
* two

an ordered list:
1. one
2. two

a big list with paragraphs:

* ooko ko kok ok okokok okoko kok okoko kokoko kokok kokokok
  iikoikoiko  ikoikoiko  ikoikoiko  ikoikoiko  koikoiko 

* three lines
  three lines
  three lines


This is a rule (hr):

------


_generates em tag_

__generates strong tag__

`backquote produce code tag`

__slow webrick__:
`sudo service avahi-daemon stop`

