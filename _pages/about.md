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
    /* --- 年度墙超参数 --- */
    --wall-bg: #f0f2f5;          /* 深一点的背景灰 */
    --empty-cell: #ffffff;       /* 纯白空色块，与背景拉开层次 */
    --cell-w: 12px;
    --cell-h: 9px;
    --gap: 3px;
    --radius: 2px;
  }

  .year-matrix-wrapper-v3 {
    background: var(--wall-bg);
    padding: 30px 40px;
    border-radius: 8px;
    margin: 40px auto;
    width: fit-content; /* 紧凑包裹，确保居中 */
    display: flex;
    flex-direction: column;
    border: 1px solid #e1e4e8;
    font-family: -apple-system, system-ui, sans-serif;
  }

  .matrix-main-container {
    display: grid;
    grid-template-columns: 30px 1fr; /* 纵轴空间 */
    grid-template-rows: 20px 1fr;    /* 横轴(月份)空间 */
    gap: 4px;
  }

  /* 月份横轴 */
  .month-axis {
    grid-column: 2;
    display: flex;
    justify-content: space-between;
    font-size: 9px;
    color: #888;
    padding-bottom: 5px;
  }

  /* 星期纵轴 */
  .day-axis {
    grid-row: 2;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    font-size: 9px;
    color: #aaa;
    padding: 2px 0;
    height: calc(7 * var(--cell-h) + 6 * var(--gap));
  }

  .matrix-grid-v3 {
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
    border: 1px solid rgba(0,0,0,0.03); /* 极淡边框增强定义感 */
  }

  .cell:hover {
    transform: scale(1.3);
    z-index: 10;
    filter: saturate(1.2);
  }

  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: #333;
    color: #fff;
    padding: 4px 8px;
    border-radius: 3px;
    font-size: 9px;
    bottom: 250%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
  }

  .matrix-caption-v3 {
    margin-top: 20px;
    font-size: 11px;
    color: #777;
    border-top: 1px solid #ddd;
    padding-top: 10px;
  }
</style>

<div class="year-matrix-wrapper-v3">
  <div class="matrix-main-container">
    <div class="month-axis">
      <span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span><span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span>
    </div>

    <div class="day-axis">
      <span>M</span><span>W</span><span>F</span><span>S</span>
    </div>

    <div class="matrix-grid-v3">
      {% assign seconds_per_day = 86400 %}
      {% assign total_days = 364 %} {% comment %} 全年约 52 周 {% endcomment %}
      
      {% comment %} 寻找到今年第一个周一 {% endcomment %}
      {% assign year_start = "now" | date: "%Y-01-01" %}
      {% assign start_dow = year_start | date: "%u" | minus: 1 %}
      {% assign start_seconds = year_start | date: "%s" | minus: first_monday_offset %}
      {% assign first_monday = start_seconds | minus: start_dow | times: seconds_per_day %}

      {% for i in (0..total_days) %}
        {% assign current_seconds = year_start | date: "%s" | plus: 0 %}
        {% comment %} 这里简化逻辑：展示从 1月1日开始的 52 周 {% endcomment %}
        {% assign offset = i | times: seconds_per_day %}
        {% assign day_ts = year_start | date: "%s" | plus: offset %}
        {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}

        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 提升明度：L 从 45% 起跳 {% endcomment %}
          {% assign h = entry.a | minus: 1 | times: 25 | plus: 200 %}
          {% assign l = entry.m | times: 7 | plus: 40 %}
          {% assign color = "hsl(" | append: h | append: ", 40%, " | append: l | append: "%)" %}
          {% assign tip = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
        {% else %}
          {% assign color = "var(--empty-cell)" %}
          {% assign tip = date_str %}
        {% endif %}

        <div class="cell" 
             style="--bg-color: {{ color }}; grid-row: {{ dow }};" 
             data-tip="{{ tip }}">
        </div>
      {% endfor %}
    </div>
  </div>

  <div class="matrix-caption-v3">
    <b>Fig 1.</b> Annual State Manifest ({{ "now" | date: "%Y" }}). 
    HSL-mapped grid showing physiological (Hue) and psychological (Lightness) variance.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

