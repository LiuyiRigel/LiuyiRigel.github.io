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
    /* --- 核心调节参数 --- */
    --wall-max-width: 1000px;    /* 墙的最大宽度限制 */
    --bg-frame: #ebedf0;         /* 工业灰框架背景 */
    --cell-empty: #ffffff;       /* 纯白空色块 (对比鲜明) */
    
    --cell-w: 12px;
    --cell-h: 9px;
    --gap: 3px;
    --radius: 2px;
  }

  .adaptive-wall-wrapper {
    background: var(--bg-frame);
    padding: 30px;
    border-radius: 8px;
    margin: 30px auto;
    width: fit-content;
    max-width: var(--wall-max-width);
    border: 1px solid #d1d5da;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* 季度布局容器：支持换行 */
  .quarter-layout {
    display: flex;
    flex-wrap: wrap;             /* 核心：空间不足时自动换行 */
    justify-content: center;
    gap: 20px;                   /* 季度间的呼吸间距 */
    width: 100%;
  }

  .quarter-box {
    display: flex;
    flex-direction: column;
    gap: 8px;
    min-width: fit-content;
  }

  .q-header {
    font-size: 10px;
    color: #888;
    font-weight: bold;
    text-transform: uppercase;
    border-bottom: 1px solid #ccc;
    padding-bottom: 3px;
  }

  .matrix-sub-grid {
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
    transition: transform 0.1s;
    border: 1px solid rgba(0,0,0,0.05);
  }

  .cell:hover {
    transform: scale(1.4);
    z-index: 50;
    filter: saturate(1.4) brightness(1.1);
  }

  .cell:hover::after {
    content: attr(data-tip);
    position: absolute;
    background: #333;
    color: #fff;
    padding: 4px 8px;
    border-radius: 2px;
    font-size: 9px;
    bottom: 250%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
  }

  .wall-caption {
    margin-top: 25px;
    font-size: 11px;
    color: #666;
    width: 100%;
    text-align: left;
    border-top: 1px solid #ccc;
    padding-top: 10px;
  }
</style>

<div class="adaptive-wall-wrapper">
  <div class="quarter-layout">
    {% assign seconds_per_day = 86400 %}
    {% assign current_year = "now" | date: "%Y" %}
    {% assign year_start = current_year | append: "-01-01" %}
    
    {% comment %} 循环 4 个季度 {% endcomment %}
    {% for q in (0..3) %}
      <div class="quarter-box">
        <div class="q-header">Quarter {{ q | plus: 1 }}</div>
        <div class="matrix-sub-grid">
          
          {% assign days_in_q = 91 %} {% comment %} 每个季度固定展示 13 周 {% endcomment %}
          {% assign q_start_offset = q | times: days_in_q %}
          
          {% for i in (0..90) %}
            {% assign total_offset = q_start_offset | plus: i %}
            {% assign day_ts = year_start | date: "%s" | plus: 0 %}
            {% assign offset_sec = total_offset | times: seconds_per_day %}
            {% assign current_ts = day_ts | plus: offset_sec %}
            
            {% assign date_str = current_ts | date: "%Y-%m-%d" %}
            {% assign dow = current_ts | date: "%u" %}

            {% assign entry = site.data.moods | where: "date", date_str | first %}

            {% if entry %}
              {% comment %} 高明度 HSL 映射 {% endcomment %}
              {% assign h = entry.a | minus: 1 | times: 25 | plus: 200 %}
              {% assign l = entry.m | times: 8 | plus: 45 %}
              {% assign color = "hsl(" | append: h | append: ", 45%, " | append: l | append: "%)" %}
              {% assign tip = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
            {% else %}
              {% assign color = "var(--cell-empty)" %}
              {% assign tip = date_str %}
            {% endif %}

            <div class="cell" 
                 style="--bg-color: {{ color }}; grid-row: {{ dow }};" 
                 data-tip="{{ tip }}">
            </div>
          {% endfor %}
        </div>
      </div>
    {% endfor %}
  </div>

  <div class="wall-caption">
    <b>Fig 1.</b> Annual State Manifold ({{ current_year }}). 
    Multi-quarter layout optimized for high-density visualization. 
    Color encodes physiological/psychological variance.
  </div>
</div>

About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

