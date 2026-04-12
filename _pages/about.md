---
permalink: /
title: "Welcome to my Island"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

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
  :root {
    /* --- 超参数调节面板 (Hyper-parameters) --- */
    --wall-width: 95%;          /* 容器占据页面的宽度 */
    --cell-size: 11px;          /* 色块宽度：控制它变小的核心 */
    --cell-aspect-ratio: 1.8 / 1; /* 压扁比例：宽/高 */
    --cell-gap: 4px;            /* 色块间距 */
    --cell-radius: 3px;         /* 色块圆角 */
    --container-radius: 10px;    /* 背景容器圆角 */
    --bg-gray: #f2f2f2;         /* 灰度背景 */
  }

  .mood-matrix-wrapper {
    background: var(--bg-gray);
    padding: 25px 0; /* 上下留白 */
    border-radius: var(--container-radius);
    margin: 30px auto;
    width: var(--wall-width);
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #e5e5e5;
  }

  /* 核心对齐逻辑：使用 fit-content 确保左右间距完全由 margin: auto 分配 */
  .matrix-container-with-axes {
    display: grid;
    grid-template-columns: auto auto; /* 纵轴和网格各占其位 */
    gap: 12px;
    width: fit-content; 
    margin: 0 auto;
    padding: 0 40px; /* 这里的 padding 决定了左右两端的呼吸感 */
  }

  .y-axis {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    font-size: 9px;
    color: #bbb;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    text-align: right;
    padding: 2px 0;
  }

  .matrix-grid {
    display: grid;
    grid-template-rows: repeat(7, 1fr); 
    grid-auto-flow: column;
    grid-auto-columns: var(--cell-size); /* 固定宽度 */
    gap: var(--cell-gap);
  }

  .cell {
    width: var(--cell-size);
    aspect-ratio: var(--cell-aspect-ratio);
    border-radius: var(--cell-radius);
    background-color: var(--bg-color);
    transition: all 0.2s ease;
    position: relative;
  }

  .cell:hover {
    filter: brightness(0.9);
    transform: scale(1.2);
    z-index: 10;
  }

  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: #333;
    color: #fff;
    padding: 4px 8px;
    border-radius: 2px;
    font-size: 9px;
    bottom: 180%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    z-index: 100;
  }

  /* Academic Caption */
  .matrix-caption {
    margin-top: 20px;
    font-size: 11px;
    color: #999;
    width: fit-content;
    padding: 10px 40px 0 40px;
    border-top: 1px solid #e8e8e8;
    text-align: left;
  }
</style>

<div class="mood-matrix-wrapper">
  <div class="matrix-container-with-axes">
    <div class="y-axis">
      <span>M</span><span>W</span><span>F</span><span>S</span>
    </div>

    <div class="matrix-grid">
      {% for i in (1..91) %} 
        {% assign entry = site.data.moods[forloop.index0] %}
        {% if entry %}
          {% assign h = entry.a | minus: 1 | times: 25 | plus: 210 %}
          {% assign l = entry.m | times: 5 | plus: 20 %}
          {% assign color = "hsl(" | append: h | append: ", 25%, " | append: l | append: "%)" %}
          {% assign tip = "M" | append: entry.m | append: " / A" | append: entry.a %}
        {% else %}
          {% assign color = "#e0e0e0" %}
          {% assign tip = "N/A" %}
        {% endif %}
        <div class="cell" style="--bg-color: {{ color }};" data-tip="{{ tip }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="matrix-caption">
    <b>Fig 1.</b> State manifold. Mood-Alcohol bivariate HSL mapping. 
    Longitudinal observation of daily biological and psychological status.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

