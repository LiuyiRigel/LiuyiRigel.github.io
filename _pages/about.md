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
    --wall-bg: #f4f6f8;
    --empty-cell: #ffffff;
    --cell-w: 12px;
    --cell-h: 9px;
    --gap: 3px;
    --radius: 2px;
    --scatter-width: 450px;
    --scatter-height: 300px;
    --dot-size: 14px; /* 稍微缩小一点更精致 */
    --dot-border-w: 2px;
    --dot-border-col: rgba(255,255,255,0.8);
    --axis-text-color: #888; /* 统一颜色 */
  }

  /* --- 整体容器保持一致 --- */
  .academic-dashboard-wrapper {
    background: var(--wall-bg);
    padding: 30px 40px;
    border-radius: 8px;
    border: 1px solid #e1e4e8;
    font-family: -apple-system, system-ui, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* --- 绿墙部分 (颜色映射同步更新) --- */
  .matrix-grid-final {
    display: grid;
    grid-template-rows: repeat(7, var(--cell-h));
    grid-auto-flow: column;
    grid-auto-columns: var(--cell-w);
    gap: var(--gap);
  }

  /* --- 坐标系部分 (Bug Fixes) --- */
  .scatter-frame {
    width: var(--scatter-width);
    height: var(--scatter-height);
    border: 1px solid #ddd;
    border-radius: 4px;
    position: relative;
    box-sizing: border-box;
    overflow: visible; /* 允许点在边缘完整显示 */
    /* 背景渐变反转：映射逻辑更新 */
    background: linear-gradient(to top right, 
      hsl(220, 30%, 20%) 0%,   /* 左下：低M (深暗) */
      hsl(280, 40%, 60%) 50%, 
      hsl(200, 60%, 95%) 100%  /* 右上：高M (明亮) */
    );
  }

  /* 坐标轴标签颜色修正 & 位置微调防止重叠 */
  .axis-label {
    position: absolute;
    font-size: 10px;
    color: var(--axis-text-color);
    white-space: nowrap;
  }

  /* Y轴：垂直排列在左侧外部 */
  .label-y-high { top: 0; left: -65px; text-align: right; width: 60px; }
  .label-y-low  { bottom: 0; left: -65px; text-align: right; width: 60px; }
  
  /* X轴：水平排列在下方外部 */
  .label-x-high { bottom: -25px; right: 0; }
  .label-x-low  { bottom: -25px; left: 0; }

  /* 样本圆形归一化修正 */
  .data-dot {
    width: var(--dot-size);
    height: var(--dot-size);
    border-radius: 50%;
    border: var(--dot-border-w) solid var(--dot-border-col);
    background-color: var(--dot-color);
    position: absolute;
    /* 修正核心：translate(-50%, -50%) 确保圆心对齐坐标 */
    transform: translate(-50%, -50%); 
    transition: all 0.2s ease;
    cursor: pointer;
    box-shadow: 0 1px 3px rgba(0,0,0,0.15);
    z-index: 10;
  }

  .data-dot:hover {
    transform: translate(-50%, -50%) scale(1.4);
    z-index: 100;
  }
</style>

<div class="academic-dashboard-wrapper">
  <div class="matrix-main-container">
    <div class="matrix-grid-final">
      {% for i in (0..364) %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}
        {% if entry %}
          {% comment %} 
            反转逻辑：
            Lightness (L): Mood 越好越亮 (1->40%, 5->90%)
            Hue (H): 酒精越多，颜色越往冷/深色偏移
          {% endcomment %}
          {% assign l = entry.m | times: 12 | plus: 30 %}
          {% assign h = 240 | minus: entry.a | times: 10 %}
          {% assign color = "hsl(" | append: h | append: ", 50%, " | append: l | append: "%)" %}
        {% else %}
          {% assign color = "var(--empty-cell)" %}
        {% endif %}
        <div class="cell" style="--bg-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ date_str }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="scatter-title">M-A Correlation Space</div>
    <div class="scatter-frame">
      <div class="axis-label label-y-high">High Mood</div>
      <div class="axis-label label-y-low">Low Mood</div>
      <div class="axis-label label-x-high">Heavy Alcohol</div>
      <div class="axis-label label-x-low">Clear State</div>

      {% for entry in site.data.moods %}
        {% comment %} 归一化计算：确保 1.0-5.0 映射到 0%-100% {% endcomment %}
        {% assign x_percent = entry.a | minus: 1.0 | divided_by: 4.0 | times: 100 %}
        {% assign y_raw = entry.m | minus: 1.0 | divided_by: 4.0 | times: 100 %}
        {% assign y_percent = 100 | minus: y_raw %}

        {% comment %} 颜色映射同步反转 {% endcomment %}
        {% assign l_dot = entry.m | times: 12 | plus: 30 %}
        {% assign h_dot = 240 | minus: entry.a | times: 10 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 50%, " | append: l_dot | append: "%)" %}

        <div class="data-dot" 
             style="--dot-color: {{ color_dot }}; left: {{ x_percent }}%; top: {{ y_percent }}%;" 
             data-tip="{{ entry.date }} | M{{ entry.m }} A{{ entry.a }}{% if entry.note %} &#10;{{ entry.note }}{% endif %}">
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

