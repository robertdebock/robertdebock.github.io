# Blog

Here is a set of blog posts on Ansible. Have fun reading them, to contribute (AKA fix silly typos) please [edit these pages](https://github.com/robertdebock/robertdebock.github.io/tree/master/_posts).

{% for post in site.posts %}
<article>
  <h2>
    <a href="{{ post.url }}">
      {{ post.title }}
    </a>
  </h2>
  <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_long_string }}</time>
  {{ post.content }}
</article>
{% endfor %}

