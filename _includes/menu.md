
+ [home]({{site.url}})
+ [menu1][linkmenu]
+ [menu2][linksite]

[linkmenu]: http://www.google.fr 
[linksite]: http://test

{% for category in site.categories %}
    <span>{{category.title}}</span>
{% endfor %}
