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
    /* --- 核心超参数 --- */
    --wall-bg: #f4f6f8;
    --empty-cell: #ffffff;
    --text-gray: #888; /* 统一文本颜色 */
    
    /* 绿墙参数 */
    --cell-w: 12px;
    --cell-h: 9px;
    --gap: 3px;

    /* 坐标系参数 */
    --scatter-w: 480px;      
    --scatter-h: 320px;     
    --dot-size: 14px;            
    --dot-border-w: 2px;         
    --dot-border-col: #ffffff;   /* 纯白边界更清透 */
  }

  .academic-dashboard-wrapper {
    background: var(--wall-bg);
    padding: 35px;
    border-radius: 12px;
    margin: 40px auto;
    width: fit-content;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #e1e4e8;
    font-family: -apple-system, system-ui, sans-serif;
  }

  /* --- Wall 部分 --- */
  .matrix-main-container {
    display: grid;
    grid-template-columns: 35px 1fr;
    grid-template-rows: 25px 1fr;
    gap: 5px;
    margin-bottom: 40px;
  }

  .month-axis {
    grid-column: 2;
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    color: var(--text-gray);
  }

  .day-axis {
    grid-row: 2;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    font-size: 10px;
    color: var(--text-gray);
    height: calc(7 * var(--cell-h) + 6 * var(--gap));
  }

  .matrix-grid-final {
    display: grid;
    grid-template-rows: repeat(7, var(--cell-h));
    grid-auto-flow: column;
    grid-auto-columns: var(--cell-w);
    gap: var(--gap);
  }

  .cell {
    width: var(--cell-w);
    height: var(--cell-h);
    border-radius: 1px;
    background-color: var(--bg-color);
    border: 1px solid rgba(0,0,0,0.02);
  }

  /* --- Scatter 坐标系部分 --- */
  .scatter-plots-container {
    width: var(--scatter-w);
    position: relative;
    padding-bottom: 30px; /* 为横轴文字留位 */
  }

  .scatter-frame {
    width: var(--scatter-w);
    height: var(--scatter-h);
    border-left: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
    position: relative;
    background: linear-gradient(to top right, #334455, #88aabb, #ffffff);
    overflow: visible; /* 允许圆点悬停放大 */
  }

  /* 坐标轴文本修复 */
  .axis-label {
    position: absolute;
    font-size: 10px;
    color: var(--text-gray);
    white-space: nowrap;
  }

  .label-y { 
    left: -40px; 
    top: 50%; 
    transform: translateY(-50%) rotate(-90deg); 
  }
  .label-x { 
    bottom: -25px; 
    left: 50%; 
    transform: translateX(-50%); 
  }

  /* 样本点修复 */
  .data-dot {
    width: var(--dot-size);
    height: var(--dot-size);
    border-radius: 50%;
    border: var(--dot-border-w) solid var(--dot-border-col);
    background-color: var(--dot-color);
    position: absolute;
    transform: translate(-50%, -50%); /* 改为中心对齐 */
    box-shadow: 0 2px 5px rgba(0,0,0,0.15);
    z-index: 5;
    transition: all 0.2s;
  }

  .data-dot:hover {
    transform: translate(-50%, -50%) scale(1.5);
    z-index: 100;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  }

  .data-dot:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: rgba(40, 44, 52, 0.95);
    color: #fff;
    padding: 8px 12px;
    border-radius: 4px;
    font-size: 10px;
    bottom: 150%;
    left: 50%;
    transform: translateX(-50%);
    white-space: pre-wrap;
    width: max-content;
    max-width: 200px;
    box-shadow: 0 5px 20px rgba(0,0,0,0.3);
  }
</style>

<div class="academic-dashboard-wrapper">
  
  <div class="matrix-main-container">
    <div class="month-axis">
      <span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span><span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span>
    </div>
    <div class="day-axis">
      <span>M</span><span>W</span><span>F</span><span>S</span>
    </div>
    <div class="matrix-grid-final">
      {% assign year_start = "now" | date: "%Y-01-01" %}
      {% for i in (0..364) %}
        {% assign day_ts = year_start | date: "%s" | plus: 86400 | times: i | plus: 1735689600 %} {% assign day_ts = i | times: 86400 | plus: 1735689600 %}
        {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 反转映射：M越高越亮(L+), A越高越深(L-) {% endcomment %}
          {% assign h = entry.a | minus: 1 | times: 20 | plus: 200 %}
          {% assign l = entry.m | times: 10 | minus: entry.a | times: 5 | plus: 50 %}
          {% assign color = "hsl(" | append: h | append: ", 40%, " | append: l | append: "%)" %}
          {% assign tip = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
        {% else %}
          {% assign color = "var(--empty-cell)" %}
          {% assign tip = date_str %}
        {% endif %}
        <div class="cell" style="--bg-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ tip }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="axis-label label-y">Mood (Psychological)</div>
    <div class="axis-label label-x">Alcohol Intake (Physiological)</div>
    
    <div class="scatter-frame">
      {% for entry in site.data.moods %}
        {% comment %} 归一化修复：使用 5% 到 95% 避免边缘切断 {% endcomment %}
        {% assign x_pos = entry.a | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_val = entry.m | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_pos = 100 | minus: y_val %}

        {% assign h_dot = entry.a | minus: 1 | times: 20 | plus: 200 %}
        {% assign l_dot = entry.m | times: 10 | minus: entry.a | times: 5 | plus: 50 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 45%, " | append: l_dot | append: "%)" %}

        {% capture tip_scat %}{{ entry.date }}&#10;M: {{ entry.m }} A: {{ entry.a }}{% if entry.note %}&#10;Note: {{ entry.note }}{% endif %}{% endcapture %}

        <div class="data-dot" 
             style="--dot-color: {{ color_dot }}; left: {{ x_pos }}%; top: {{ y_pos }}%;" 
             data-tip="{{ tip_scat }}">
        </div>
      {% endfor %}
    </div>
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

