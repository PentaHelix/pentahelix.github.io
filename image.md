---
layout: page
permalink: /image/
published: true
---

{% if img.type == "image" %}
    	<img src="{{img.link}}.png" alt="{{img.title}}">
    {% else %}
    <video autoplay="autoplay" loop="loop" poster="{{img.link}}.jpg" preload="auto"><source src="{{img.link}}.webm" type="video/webm"></video>
    {% endif %}
    	<div class="imageOverlay"></div>
  </article>
{% endfor %}
<div class="posts">
    <article class="post">
		{% if site.data.images[page.url | remove: 'image/'].type == "image" %}
    		<img src="{{img.link}}.png" alt="{{img.title}}">
    	{% else %}
    		<video autoplay="autoplay" loop="loop" poster="{{img.link}}.jpg" preload="auto"><source src="{{img.link}}.webm" type="video/webm"></video>
    	{% endif %}
    </article>
</div>

