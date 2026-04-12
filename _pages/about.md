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
    /* --- 超参数调节 --- */
    --wall-bg: #f7f9fb;          /* 更清透的背景 */
    --empty-cell: #ffffff;       /* 纯白空位 */
    --text-gray: #78828e;        /* 标准学术灰文本 */
    
    /* 绿墙参数 */
    --cell-w: 13px;
    --cell-h: 10px;
    --gap: 3px;

    /* 坐标系参数 */
    --scatter-w: 500px;      
    --scatter-h: 300px;     
    --dot-size: 14px;            
    --dot-border-w: 2px;         
    --dot-border-col: #ffffff;
  }

  .academic-dashboard-wrapper {
    background: var(--wall-bg);
    padding: 40px;
    border-radius: 12px;
    margin: 40px auto;
    width: fit-content;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #e1e4e8;
    box-shadow: 0 4px 20px rgba(0,0,0,0.03);
    font-family: "Inter", -apple-system, system-ui, sans-serif;
  }

  /* --- Wall 部分 --- */
  .matrix-main-container {
    display: grid;
    grid-template-columns: 35px 1fr;
    grid-template-rows: 25px 1fr;
    gap: 6px;
    margin-bottom: 50px;
  }

  .month-axis {
    grid-column: 2;
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    color: var(--text-gray);
    letter-spacing: 0.05em;
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
    position: relative;
    transition: filter 0.2s;
  }

  /* --- Scatter 坐标系部分 --- */
  .scatter-plots-container {
    width: var(--scatter-w);
    position: relative;
  }

  .scatter-frame {
    width: var(--scatter-w);
    height: var(--scatter-h);
    border-left: 1px solid #d1d5da;
    border-bottom: 1px solid #d1d5da;
    position: relative;
    /* 梦幻渐变：从左下的暗青紫到右上的亮薄荷绿 */
    background: linear-gradient(135deg, #3d4a5e 0%, #7da8a6 50%, #e0f2f1 100%);
    overflow: visible;
  }

  .axis-label {
    position: absolute;
    font-size: 11px;
    color: var(--text-gray);
    white-space: nowrap;
    font-weight: 500;
  }

  .label-y { 
    left: -50px; 
    top: 50%; 
    transform: translateY(-50%) rotate(-90deg); 
  }
  .label-x { 
    bottom: -30px; 
    left: 50%; 
    transform: translateX(-50%); 
  }

  .data-dot {
    width: var(--dot-size);
    height: var(--dot-size);
    border-radius: 50%;
    border: var(--dot-border-w) solid var(--dot-border-col);
    background-color: var(--dot-color);
    position: absolute;
    transform: translate(-50%, 50%);
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    z-index: 10;
    transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }

  .data-dot:hover {
    transform: translate(-50%, 50%) scale(1.6);
    z-index: 100;
    box-shadow: 0 8px 24px rgba(0,0,0,0.15);
  }

  /* --- Tooltip (Wall & Scatter 共享样式) --- */
  .cell:hover::after, .data-dot:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: rgba(45, 55, 72, 0.95);
    color: #fff;
    padding: 8px 12px;
    border-radius: 4px;
    font-size: 10px;
    bottom: 180%;
    left: 50%;
    transform: translateX(-50%);
    white-space: pre-wrap;
    width: max-content;
    max-width: 200px;
    z-index: 200;
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
      {% assign current_year = "now" | date: "%Y" %}
      {% assign year_start = current_year | append: "-01-01" %}
      {% for i in (0..364) %}
        {% assign day_ts = i | times: 86400 | plus: 1735689600 %} {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 
            重构颜色映射逻辑：
            H (Hue): 心情决定绿/青程度 (150-190)
            S (Saturation): 固定低饱和 (35%)
            L (Lightness): M 越高 L 越高(清醒)，A 越高 L 越低(梦幻/沉重)
          {% endcomment %}
          {% assign h = 150 | plus: entry.a | times: 8 %}
          {% assign l = entry.m | times: 8 | minus: entry.a | times: 4 | plus: 40 %}
          {% assign color = "hsl(" | append: h | append: ", 35%, " | append: l | append: "%)" %}
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
    <div class="axis-label label-y">Positive Mood</div>
    <div class="axis-label label-x">Alcohol Consumption</div>
    
    <div class="scatter-frame">
      {% for entry in site.data.moods %}
        {% comment %} 归一化修复 5%-95% 保护圆点不溢出 {% endcomment %}
        {% assign x_pos = entry.a | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_raw = entry.m | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_pos = 100 | minus: y_raw %}

        {% assign h_dot = 150 | plus: entry.a | times: 8 %}
        {% assign l_dot = entry.m | times: 8 | minus: entry.a | times: 4 | plus: 40 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 38%, " | append: l_dot | append: "%)" %}

        {% capture tip_scat %}{{ entry.date }}&#10;M: {{ entry.m }} A: {{ entry.a }}{% if entry.note %}&#10;Note: {{ entry.note }}{% endif %}{% endcapture %}

        <div class="data-dot" 
             style="--dot-color: {{ color_dot }}; left: {{ x_pos }}%; top: {{ y_pos }}%;" 
             data-tip="{{ tip_scat }}">
        </div>
      {% endfor %}
    </div>
  </div>

  <div class="matrix-caption-v3" style="margin-top: 30px; font-size: 11px; color: #999; text-align: left; width: 100%; border-top: 1px solid #eee; padding-top: 10px;">
    <b>Fig 1.</b> Bivariate manifold of psychological state. <b>Lighter mint-green</b> indicates high-lucidity mood; <b>Deeper oceanic-teal</b> represents alcohol-induced dreaminess.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

