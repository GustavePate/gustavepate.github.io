
    <!--<li><a class="extra" href="/">menu2</a></li>
    {% for category in site.categories %}
        <li><a href="{{ site.url }}/{{category}}">{{category.title }}</a></li>
    {% endfor %}-->
{% for category in site.categories %}
    + {{category.title}}
{% endfor %}
