---
layout: archive
permalink: /numerical-analysis/
title: "This page will contain all the posts"
author_profile: true
header:
#  image:
---
# Post Post Post Heading

## These are subheadings

list of links:
* [bisection method]({{ site.baseurl }}{% post_url _2018-01-29-bisection %})
* [false position]({{ site.baseurl }}{% post_url 2018-01-30-false-position %})
body text!

Here's a [link to the google page](https://google.com)

bulleted list use mathematical operatsors:
* Item 1
+ Item 2
- Item 3

Similarly with numbered lists:
1. First
2. Second
3. Third


Python code!
```python
    def add(a, b):
        return a + b
    print (add(2+3))
```
Jekyll uses Mathjax to render mathematical equations

$$E=mc^2$$

you can also put it inline $$E=mc^2$$


MORE TEXT!

Here's some inline equations `x+y`

{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
