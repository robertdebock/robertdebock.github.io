An overview of articles
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

{% for post in site.posts | sort post.date %}
<article>
{{ post.content }}
</article>
{% endfor %}

