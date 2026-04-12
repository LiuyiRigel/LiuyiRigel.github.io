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
    --wall-width: 100%;
    --cell-aspect-ratio: 1.5 / 1; /* 季度分区建议比例 */
    --cell-gap: 3px;
    --quarter-gap: 15px;         /* 季度大区之间的间距 */
    --bg-gray: #f2f2f2;
    --q-label-color: #999;
  }

  .year-matrix-wrapper {
    background: var(--bg-gray);
    padding: 35px 25px;
    border-radius: 8px;
    margin: 30px auto;
    width: var(--wall-width);
    box-sizing: border-box;
    border: 1px solid #e5e5e5;
  }

  /* 季度分区大容器 */
  .quarter-container {
    display: flex;
    gap: var(--quarter-gap);
    width: 100%;
    align-items: flex-start;
  }

  .quarter-section {
    flex: 1; /* 每个季度等宽 */
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .q-label {
    font-size: 10px;
    color: var(--q-label-color);
    text-transform: uppercase;
    font-weight: 600;
    letter-spacing: 1px;
    border-bottom: 1px solid #ddd;
    padding-bottom: 4px;
  }

  .matrix-grid-q {
    display: grid;
    grid-template-rows: repeat(7, 1fr);
    grid-auto-flow: column;
    grid-auto-columns: 1fr;
    gap: var(--cell-gap);
    width: 100%;
  }

  .cell {
    aspect-ratio: var(--cell-aspect-ratio);
    border-radius: 1px;
    transition: all 0.2s ease;
    position: relative;
  }

  .cell:hover {
    filter: brightness(0.9);
    transform: scale(1.2);
    z-index: 10;
  }

  /* Tooltip */
  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: #333;
    color: #fff;
    padding: 4px 8px;
    border-radius: 2px;
    font-size: 9px;
    bottom: 160%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    z-index: 100;
  }

  .matrix-caption {
    margin-top: 30px;
    font-size: 11px;
    color: #888;
    border-top: 1px solid #e0e0e0;
    padding-top: 12px;
  }
</style>

<div class="year-matrix-wrapper">
  <div class="quarter-container">
    {% assign seconds_per_day = 86400 %}
    {% comment %} 计算当前年度第一天对应的周一（第一个有效周） {% endcomment %}
    {% assign year_start_str = "now" | date: "%Y-01-01" %}
    {% assign year_start_seconds = year_start_str | date: "%s" | plus: 0 %}
    {% assign day_of_week_start = year_start_str | date: "%u" | minus: 1 %}
    {% assign first_monday_offset = day_of_week_start | times: seconds_per_day %}
    {% assign first_monday_seconds = year_start_seconds | minus: first_monday_offset %}

    {% for q in (0..3) %}
      <div class="quarter-section">
        <div class="q-label">Quarter 0{{ q | plus: 1 }}</div>
        <div class="matrix-grid-q">
          {% assign days_per_quarter = 13 | times: 7 %}
          {% assign q_offset_days = q | times: days_per_quarter %}
          {% assign q_offset_seconds = q_offset_days | times: seconds_per_day %}
          
          {% for day_idx in (0..90) %} {% comment %} 每个季度 13 周共 91 天 {% endcomment %}
            {% assign current_offset = day_idx | times: seconds_per_day | plus: q_offset_seconds %}
            {% assign current_day_seconds = first_monday_seconds | plus: current_offset %}
            {% assign current_date_str = current_day_seconds | date: "%Y-%m-%d" %}
            {% assign current_day_of_week = current_day_seconds | date: "%u" %}

            {% assign entry = site.data.moods | where: "date", current_date_str | first %}

            {% if entry %}
              {% assign h = entry.a | minus: 1 | times: 25 | plus: 210 %}
              {% assign l = entry.m | times: 5 | plus: 20 %}
              {% assign color = "hsl(" | append: h | append: ", 25%, " | append: l | append: "%)" %}
              {% assign tip = current_date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
            {% else %}
              {% assign color = "#e8e8e8" %}
              {% assign tip = current_date_str | append: " | N/A" %}
            {% endif %}

            <div class="cell" 
                 style="background-color: {{ color }}; grid-row: {{ current_day_of_week }};" 
                 data-tip="{{ tip }}">
            </div>
          {% endfor %}
        </div>
      </div>
    {% endfor %}
  </div>

  <div class="matrix-caption">
    <b>Fig 1.</b> Annual manifold partitioned by fiscal quarters. 
    Horizontal axis represents 52-week longitudinal observation. 
    Grid alignment initiated from the first valid Monday of {{ "now" | date: "%Y" }}.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

