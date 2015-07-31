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
    <article id="imageDisplay">
    	<script>
        	var img = location.search.split('id=')[1];
            if(imgs[img][1] == "video"){
            	document.write("<video autoplay='autoplay' loop='loop' poster='"+imgs[img][]1+".jpg' preload='auto'><source src='"+imgs[img][]1+".webm' type='video/webm'></video>");
            }else{
    			document.write("<img src='"+imgs[img][]1+".png' alt=''>");
            }
        </script>
    </article>
</div>
