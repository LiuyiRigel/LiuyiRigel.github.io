---
permalink: /
title: "Welcome to my island"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

This is the front page of my website blog. 一点互联网角落的碎碎念


* 一点随笔
* 学术笔记
* 一点电影/ 书籍的观后感



---
## 📝 最近博文 / Recent Posts


{% for post in site.posts limit:3 %}
  {% include archive-single.html  %}

  {% unless forloop.last %}
<div style="height: 2px; background: linear-gradient(to right, rgba(0,0,0,0), rgba(60, 10, 100, 0.60), rgba(0,0,0,0)); margin: 20px 0; clear: both;"></div>
  {% endunless %}

{% endfor %}


[查看更多博文]({{ site.baseurl }}/year-archive/)


---
## 📝 项目 / Recent Projects

* Aerodynamic Effects of Stretched Formations During Long-Distance Running (Marathons)
<div align="center">
    <img src="/images/project_images/FFF.png" 
         style="width: 100%; 
                border-radius: 10px; 
                box-shadow: 0 10px 30px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>

<br>


About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

