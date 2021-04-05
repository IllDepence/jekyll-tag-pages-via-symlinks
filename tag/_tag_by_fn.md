---
layout: none
---

{% assign output_tag = page.name | remove:'.md' %}

[back]({{ site.url }})

# All posts tagged \#{{ output_tag }}

{% for post in site.posts %}
  {% if post.tags contains output_tag %}
### [{{ post.title }}]({{ site.url }}{{ post.url }})

{{ post.content }}
  {% endif %}
{% endfor %}
