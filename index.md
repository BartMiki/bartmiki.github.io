---
layout: default
title: Home
---

If you are looking for information about programming, machine learning, data science, and big data you are in the right place! 
For me, this website is a collection of information and tips.

*Grab a cup of tea and enjoy!*

## Newest posts
{% for post in site.posts %}
{{ post.date | date_to_string }} » [{{ post.title }}]({{ post.url }})
{% endfor %}