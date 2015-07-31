---
layout: page
permalink: /image
published: true
---

<script>
	var imgs = [];
	{% for img in site.data.img %}
    	imgs.push(['{{img.link}}', '{{img.type}}', '{{img.title}}']);
    {% endfor %}
</script>


<div class="posts">
    <article id="imageDisplay">
    	<script>
        	var img = location.search.split('id=')[1];
            document.write("<h1>"+imgs[img][2]+"<h1/>");
            if(imgs[img][1] == "video"){
            	document.write("<video autoplay='autoplay' loop='loop' poster='"+imgs[img][0]+".jpg' preload='auto'><source src='"+imgs[img][0]+".webm' type='video/webm'></video>");
            }else{
    			document.write("<img src='"+imgs[img][0]+".png' alt=''>");
            }
        </script>
    </article>
</div>
