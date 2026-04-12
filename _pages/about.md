---
permalink: /
title: "Welcome to my Island"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<style>
  .mood-matrix-container {
    border: 1px solid #e1e4e8;
    border-radius: 6px;
    padding: 15px;
    background: #ffffff;
    max-width: fit-content;
    font-family: -apple-system, sans-serif;
  }

  .matrix-grid {
    display: grid;
    grid-template-rows: repeat(7, 12px); /* 7行代表一周 */
    grid-auto-flow: column;
    grid-auto-columns: 12px;
    gap: 3px;
  }

  .cell {
    width: 12px;
    height: 12px;
    border-radius: 2px;
    background-color: var(--bg-color);
    border: 1px solid rgba(0,0,0,0.05);
    transition: transform 0.2s;
    cursor: help;
  }

  .cell:hover {
    transform: scale(1.4);
    z-index: 10;
    filter: brightness(1.1);
  }

  /* 提示框逻辑 */
  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: #24292e;
    color: white;
    padding: 5px 10px;
    border-radius: 4px;
    font-size: 11px;
    white-space: nowrap;
    top: -30px;
    left: 50%;
    transform: translateX(-50%);
  }
</style>

 一点互联网角落的碎碎念/ This is the front page of my website blog.


* 一点随笔/ Random Thoughts
* 学术笔记/ Paper Notes
* 一点电影/ 书籍的观后感 / Movie and Book Reviews
* 以及一些项目/ And some projects


## 📝 最近博文 / Recent Posts


{% for post in site.posts limit:3 %}
  {% include archive-single.html  %}

  {% unless forloop.last %}
<div style="height: 2px; background: linear-gradient(to right, rgba(0,0,0,0), rgba(138, 43, 201, 0.45), rgba(0,0,0,0)); margin: 20px 0; clear: both;"></div>
  {% endunless %}

{% endfor %}

[查看更多博文/ More Posts]({{ site.baseurl }}/year-archive/)


---
## 📝 项目 / Recent Projects

* Aerodynamic Effects of Stretched Formations During Long-Distance Running (Marathons)  
<div align="center">
    <img src="../images/project_images/FFF.png" 
         style="width: 95%; 
                border-radius: 5px; 
                box-shadow: 0 10px 15px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>

<br>
<a href="../files/paper-file/Aerodynamic Effects of Stretched Formations During Long-Distance Running (Marathons)_Ver_0.02.1.pdf" class="btn btn--info">Download PDF</a>



* Dummy-Robot: Super compact smart robotic-arm  
<div align="center">
    <img src="../images/project_images/Frame000168.png
" 
         style="width: 95%; 
                border-radius: 5px; 
                box-shadow: 0 10px 15px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>

<br>
<a href="https://github.com/LiuyiRigel/Dummy-Robot" class="btn btn--info">View on GitHub</a>

<div style="height: 2px; background: linear-gradient(to right, rgba(0,0,0,0), rgba(138, 43, 201, 0.45), rgba(0,0,0,0)); margin: 20px 0; clear: both;"></div>

[//]: (Mood & Alcohol Matrix Visualization - A personal state monitor that maps mood and alcohol levels to a color-coded grid, providing an intuitive visual representation of daily fluctuations. Each cell's color intensity reflects the combined effect of mood and alcohol consumption, allowing for quick insights into patterns and correlations over time.)

<style>
  .mood-matrix-wrapper {
    background: #0d1117; /* 极简深色背景 */
    padding: 20px;
    border-radius: 10px;
    display: inline-block;
  }

  .matrix-grid {
    display: grid;
    /* 7行代表一周 */
    grid-template-rows: repeat(7, 12px); 
    grid-auto-flow: column;
    grid-auto-columns: 12px;
    gap: 4px;
  }

  .cell {
    width: 12px;
    height: 12px;
    border-radius: 2px;
    background-color: var(--bg-color);
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  }

  /* 悬停时仅微亮，不显示文字 */
  .cell:hover {
    filter: brightness(1.5);
    transform: scale(1.1);
  }
</style>

<div class="mood-matrix-wrapper">
  <div class="matrix-grid">
    {% comment %}
      设置显示的总天数（例如 84 天代表 12 周）
    {% endcomment %}
    {% for i in (1..84) %}
      {% comment %}
        这里可以放置更复杂的 Liquid 逻辑来匹配日期。
        目前为了确保显示，我们先渲染基础色块。
      {% endcomment %}
      
      {% assign entry = site.data.moods[forloop.index0] %}
      
      {% if entry %}
        {% comment %} 有数据：根据 m 和 a 计算沉静色调 {% endcomment %}
        {% assign hue_shift = entry.a | minus: 1 | times: 25 %}
        {% assign h = 200 | plus: hue_shift %}
        {% assign l = entry.m | times: 6 | plus: 15 %}
        {% assign color = "hsl(" | append: h | append: ", 30%, " | append: l | append: "%)" %}
      {% else %}
        {% comment %} 无数据：浅灰色/深灰（克制感） {% endcomment %}
        {% assign color = "#161b22" %}
      {% endif %}

      <div class="cell" style="--bg-color: {{ color }};"></div>
    {% endfor %}
  </div>
</div>


About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

