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
  /* Academic Container */
  .mood-matrix-wrapper {
    background: #f8f9fa; /* Lighter academic gray */
    padding: 25px 35px;
    border-radius: 4px;
    margin: 30px auto;
    width: 92%; /* Adjustable width percentage */
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #efefef;
  }

  .matrix-container-with-axes {
    display: grid;
    grid-template-columns: 30px 1fr;
    grid-template-rows: 1fr 20px;
    gap: 8px;
    width: 100%;
  }

  /* Axis Labels */
  .y-axis, .x-axis {
    font-size: 9px;
    color: #bbb;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .y-axis {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    text-align: right;
    padding: 4px 0;
  }

  .x-axis {
    grid-column: 2;
    display: flex;
    justify-content: space-between;
  }

  /* Grid Layout */
  .matrix-grid {
    display: grid;
    grid-template-rows: repeat(7, 1fr); 
    grid-auto-flow: column;
    grid-auto-columns: 1fr;
    gap: 5px; /* Spacing between cells */
    width: 100%;
  }

  .cell {
    aspect-ratio: 1 / 1;
    border-radius: 1px;
    background-color: var(--bg-color);
    transition: all 0.2s ease-in-out;
    position: relative;
  }

  .cell:hover {
    filter: contrast(1.2) brightness(0.9);
    transform: scale(1.2);
    z-index: 10;
  }

  /* Tooltip */
  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: rgba(50, 50, 50, 0.9);
    color: #fff;
    padding: 4px 8px;
    border-radius: 2px;
    font-size: 9px;
    bottom: 150%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    z-index: 20;
  }

  /* Simplified Academic Caption */
  .matrix-caption {
    margin-top: 20px;
    font-size: 11px;
    color: #888;
    width: 100%;
    text-align: left;
    border-top: 1px solid #eee;
    padding-top: 10px;
  }

  .matrix-caption b { color: #444; }
</style>

<div class="mood-matrix-wrapper">
  <div class="matrix-container-with-axes">
    <div class="y-axis">
      <span>Mon</span><span>Wed</span><span>Fri</span><span>Sun</span>
    </div>

    <div class="matrix-grid">
      {% for i in (1..91) %} 
        {% assign entry = site.data.moods[forloop.index0] %}
        
        {% if entry %}
          {% assign h = entry.a | minus: 1 | times: 25 | plus: 200 %}
          {% assign l = entry.m | times: 6 | plus: 18 %}
          {% assign color = "hsl(" | append: h | append: ", 25%, " | append: l | append: "%)" %}
          {% assign tip = "M" | append: entry.m | append: " / A" | append: entry.a %}
        {% else %}
          {% assign color = "#ececec" %}
          {% assign tip = "N/A" %}
        {% endif %}

        <div class="cell" style="--bg-color: {{ color }};" data-tip="{{ tip }}"></div>
      {% endfor %}
    </div>

    <div class="x-axis">
      <span>W01</span><span>W04</span><span>W08</span><span>W13</span>
    </div>
  </div>

  <div class="matrix-caption">
    <b>Figure 1.</b> Spatiotemporal mapping of daily states. 
    <b>Color cells</b> represent bivariate values of Mood (L) and Alcohol (H). 
    Low-saturation HSL encoding for longitudinal observation.
  </div>
</div>


About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

