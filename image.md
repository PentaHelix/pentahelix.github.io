---
layout: page
permalink: /image/
published: true
---

<script>
	var imgs = [];
	{% for img in site.data.img %}
    	imgs.push(['{{img.link}}', '{{img.type}}']);
    {% endfor %}
</script>


<div class="posts">
    <article class="post">
    	<script>
        	var myParam = location.search.split('myParam=')[1];
            if(){
            }
    		//<img src="{{img.link}}.png" alt="{{img.title}}">
    		//<video autoplay="autoplay" loop="loop" poster="{{img.link}}.jpg" preload="auto"><source src="{{img.link}}.webm" type="video/webm"></video>
        </script>
    </article>
</div>
