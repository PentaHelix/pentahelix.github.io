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
        	var img = location.search.split('id=')[1];
            if(imgs[img][1] == "video"){
            	<video autoplay="autoplay" loop="loop" poster="{{img.link}}.jpg" preload="auto"><source src="{{img.link}}.webm" type="video/webm"></video>
            }else{
    			<img src="{{img.link}}.png" alt="{{img.title}}">
            }
        </script>
    </article>
</div>
