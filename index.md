---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

{% for post in paginator.posts %} 
{{ post.title }}
{{ post.content | strip_html | truncatewords:40 }}
{% endfor %}