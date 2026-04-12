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
    /* --- 通用超参数 --- */
    --wall-bg: #f4f6f8;
    --empty-cell: #ffffff;
    --axis-text-color: #888; /* 统一文字颜色 */
    
    /* --- 绿墙超参数 --- */
    --cell-w: 12px;
    --cell-h: 9px;
    --gap: 3px;
    --radius: 2px;

    /* --- 二维坐标系超参数 --- */
    --scatter-width: 450px;
    --scatter-height: 300px;
    --dot-size: 14px;
    --dot-border-w: 2px;
    --dot-border-col: rgba(255,255,255,0.8);
  }

  /* --- 整体容器 --- */
  .academic-dashboard-wrapper {
    background: var(--wall-bg);
    padding: 30px 40px;
    border-radius: 8px;
    margin: 40px auto;
    width: fit-content;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #e1e4e8;
    font-family: -apple-system, system-ui, sans-serif;
  }

  /* --- 绿墙部分 --- */
  .matrix-main-container {
    display: grid;
    grid-template-columns: 30px 1fr;
    grid-template-rows: 20px 1fr;
    gap: 4px;
    margin-bottom: 30px;
  }

  .month-axis {
    grid-column: 2;
    display: flex;
    justify-content: space-between;
    font-size: 9px;
    color: var(--axis-text-color);
    padding-bottom: 5px;
  }

  .day-axis {
    grid-row: 2;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    font-size: 9px;
    color: var(--axis-text-color);
    padding: 2px 0;
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
    border-radius: var(--radius);
    background-color: var(--bg-color);
    transition: transform 0.2s;
    border: 1px solid rgba(0,0,0,0.03);
    position: relative;
  }

  .cell:hover {
    transform: scale(1.3);
    z-index: 100;
  }

  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: rgba(33, 33, 33, 0.9);
    color: #fff;
    padding: 4px 8px;
    border-radius: 2px;
    font-size: 9px;
    bottom: 250%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    z-index: 1000;
  }

  /* --- 二维坐标系部分 --- */
  .scatter-plots-container {
    width: var(--scatter-width);
    margin-top: 15px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .scatter-title {
    font-size: 11px;
    color: #666;
    margin-bottom: 12px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .scatter-frame {
    width: var(--scatter-width);
    height: var(--scatter-height);
    border: 1px solid #ddd;
    border-radius: 4px;
    position: relative;
    box-sizing: border-box;
    /* 背景保持原有的渐变逻辑感 */
    background: linear-gradient(135deg, hsl(310, 45%, 85%) 0%, hsl(210, 25%, 25%) 100%);
  }

  /* 坐标轴标签修正：移出内部，统一颜色，防止交叉 */
  .axis-label {
    position: absolute;
    font-size: 10px;
    color: var(--axis-text-color);
    font-weight: 500;
  }

  .label-y-high { top: 0; left: -75px; width: 70px; text-align: right; }
  .label-y-low  { bottom: 0; left: -75px; width: 70px; text-align: right; }
  .label-x-high { bottom: -22px; right: 0; }
  .label-x-low  { bottom: -22px; left: 0; }

  /* 样本点归一化修正 */
  .data-dot {
    width: var(--dot-size);
    height: var(--dot-size);
    border-radius: 50%;
    border: var(--dot-border-w) solid var(--dot-border-col);
    background-color: var(--dot-color);
    position: absolute;
    /* 修正：圆心精准定位 */
    transform: translate(-50%, -50%); 
    transition: all 0.2s ease;
    cursor: pointer;
    box-shadow: 0 2px 6px rgba(0,0,0,0.15);
    z-index: 10;
  }

  .data-dot:hover {
    transform: translate(-50%, -50%) scale(1.4);
    z-index: 100;
    border-color: #fff;
  }

  .data-dot:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: rgba(33, 33, 33, 0.95);
    color: #fff;
    padding: 8px 12px;
    border-radius: 4px;
    font-size: 10px;
    line-height: 1.5;
    bottom: 150%;
    left: 50%;
    transform: translateX(-50%);
    white-space: pre-wrap;
    width: max-content;
    max-width: 200px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }

  .matrix-caption-final {
    margin-top: 35px;
    font-size: 11px;
    color: #777;
    border-top: 1px solid #ddd;
    padding-top: 12px;
    width: 100%;
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
      {% assign seconds_per_day = 86400 %}
      {% assign current_year = "now" | date: "%Y" %}
      {% assign year_start = current_year | append: "-01-01" %}
      
      {% for i in (0..364) %}
        {% assign offset = i | times: seconds_per_day %}
        {% assign day_ts = year_start | date: "%s" | plus: offset %}
        {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 保持原有映射逻辑公式，但反转输入数值：使高分变亮 {% endcomment %}
          {% assign m_rev = 6 | minus: entry.m %}
          {% assign a_rev = 6 | minus: entry.a %}
          {% assign h = a_rev | minus: 1 | times: 25 | plus: 200 %}
          {% assign l = m_rev | times: 7 | plus: 45 %}
          {% assign color = "hsl(" | append: h | append: ", 40%, " | append: l | append: "%)" %}
          {% assign tip_wall = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
        {% else %}
          {% assign color = "var(--empty-cell)" %}
          {% assign tip_wall = date_str %}
        {% endif %}

        <div class="cell" style="--bg-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ tip_wall }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="scatter-title">M-A Correlation Space</div>
    <div class="scatter-frame">
      <div class="axis-label label-y-high">High Mood (5.0)</div>
      <div class="axis-label label-y-low">Low Mood (1.0)</div>
      <div class="axis-label label-x-high">High Alcohol (5.0)</div>
      <div class="axis-label label-x-low">Clear (1.0)</div>

      {% for entry in site.data.moods %}
        {% comment %} 坐标映射：1.0-5.0 -> 0%-100% {% endcomment %}
        {% assign x_percent = entry.a | minus: 1.0 | divided_by: 4.0 | times: 100 %}
        {% assign y_raw_percent = entry.m | minus: 1.0 | divided_by: 4.0 | times: 100 %}
        {% assign y_percent = 100 | minus: y_raw_percent %}

        {% comment %} 颜色逻辑同步 Wall 反转 {% endcomment %}
        {% assign m_rev_dot = 6 | minus: entry.m %}
        {% assign a_rev_dot = 6 | minus: entry.a %}
        {% assign h_dot = a_rev_dot | minus: 1 | times: 25 | plus: 200 %}
        {% assign l_dot = m_rev_dot | times: 7 | plus: 45 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 40%, " | append: l_dot | append: "%)" %}

        {% capture tip_scatter %}{{ entry.date }} | M{{ entry.m }} A{{ entry.a }}{% if entry.note %} &#10;Note: {{ entry.note }}{% endif %}{% endcapture %}

        <div class="data-dot" 
             style="--dot-color: {{ color_dot }}; left: {{ x_percent }}%; top: {{ y_percent }}%;" 
             data-tip="{{ tip_scatter }}">
        </div>
      {% endfor %}
    </div>
  </div>

  <div class="matrix-caption-final">
    <b>Fig 1.</b> Bivariate Analysis of Daily States ({{ current_year }}). 
    Colors preserve original HSL mapping with inverted logic: Higher <b>Mood</b> yields increased lightness; 
    Higher <b>Alcohol</b> shifts hue. X-axis: Alcohol intake; Y-axis: Mood level.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

