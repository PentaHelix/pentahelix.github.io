---
layout: page
permalink: "/image/:num/"
published: true
---

<script>
	var imgs = [];
	{% for img in site.data.images %}
    	imgs.push(['{{img.link}}', '{{img.type}}']);
    {% endfor %}
</script>


<div class="posts">
    <article class="post">
    	{% assign img = site.data.images[page.url | remove: 'image/'] %}
		{% if img.type == "image" %}
    		<img src="{{img.link}}.png" alt="{{img.title}}">
    	{% else %}
    		<video autoplay="autoplay" loop="loop" poster="{{img.link}}.jpg" preload="auto"><source src="{{img.link}}.webm" type="video/webm"></video>
    	{% endif %}
    </article>
</div>
