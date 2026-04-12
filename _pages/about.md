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
    /* --- 1. 坐标系尺寸与外观 --- */
    --scatter-w: 520px;      
    --scatter-h: 320px;     
    --dot-size: 14px;            
    --dot-border-w: 2px;         
    --dot-border-col: #ffffff;
    --text-gray: #78828e;

    /* --- 2. 颜色映射超参数 (在此分别更改两个参数的逻辑) --- */
    /* 酒精度 (A) 影响色相与深度 */
    --h-min: 155;        /* 1.0酒精时的色相 (绿色) */
    --h-max: 200;        /* 5.0酒精时的色相 (青蓝色) */
    --a-dark-factor: 6;  /* 酒精对明度的扣减系数 (越大则高酒精越深沉) */

    /* 心情 (M) 影响亮度 */
    --l-min: 25%;        /* 1.0心情时的最低亮度 (梦幻暗色) */
    --l-max: 82%;        /* 5.0心情时的最高亮度 (清醒亮色) */
  }

  .academic-dashboard-wrapper {
    background: #f8fafc;
    padding: 40px;
    border-radius: 12px;
    margin: 40px auto;
    width: fit-content;
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #e2e8f0;
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
    grid-column: 2; display: flex; justify-content: space-between;
    font-size: 10px; color: var(--text-gray);
  }

  .day-axis {
    grid-row: 2; display: flex; flex-direction: column; justify-content: space-between;
    font-size: 10px; color: var(--text-gray); height: 81px; /* 7*9 + 6*3 */
  }

  .matrix-grid-final {
    display: grid; grid-template-rows: repeat(7, 9px);
    grid-auto-flow: column; grid-auto-columns: 12px; gap: 3px;
  }

  .cell {
    width: 12px; height: 9px; border-radius: 2px;
    background-color: var(--bg-color); border: 1px solid rgba(0,0,0,0.02);
  }

  /* --- Scatter 坐标系部分 --- */
  .scatter-plots-container {
    width: var(--scatter-w);
    position: relative;
    margin-top: 10px;
  }

  .scatter-frame {
    width: var(--scatter-w);
    height: var(--scatter-h);
    border-left: 2px solid #cbd5e1;
    border-bottom: 2px solid #cbd5e1;
    position: relative;
    /* 适配图示：左下(深) -> 右上(明) 的渐变 */
    background: linear-gradient(135deg, #1e293b 0%, #334155 30%, #5eead4 100%);
    overflow: visible;
  }

  /* 坐标轴文本与刻度 (0-5) */
  .axis-label {
    position: absolute; font-size: 11px; color: var(--text-gray); font-weight: 500;
  }
  .label-y { left: -60px; top: 50%; transform: translateY(-50%) rotate(-90deg); }
  .label-x { bottom: -35px; left: 50%; transform: translateX(-50%); }

  .tick { position: absolute; font-size: 9px; color: #94a3b8; }
  .tick-y-0 { bottom: 0; left: -15px; }
  .tick-y-5 { top: 0; left: -15px; }
  .tick-x-0 { bottom: -18px; left: 0; }
  .tick-x-5 { bottom: -18px; right: 0; }

  /* 样本点 */
  .data-dot {
    width: var(--dot-size); height: var(--dot-size);
    border-radius: 50%; border: var(--dot-border-w) solid var(--dot-border-col);
    background-color: var(--dot-color); position: absolute;
    transform: translate(-50%, 50%);
    transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    z-index: 10;
  }

  .data-dot:hover {
    transform: translate(-50%, 50%) scale(1.5);
    z-index: 100;
    box-shadow: 0 8px 20px rgba(0,0,0,0.2);
  }

  .cell:hover::after, .data-dot:hover::after {
    content: attr(data-tip); position: absolute;
    background: rgba(30, 41, 59, 0.98); color: #fff;
    padding: 8px 12px; border-radius: 4px; font-size: 10px;
    bottom: 200%; left: 50%; transform: translateX(-50%);
    white-space: pre-wrap; width: max-content; max-width: 220px; z-index: 200;
  }
</style>

<div class="academic-dashboard-wrapper">
  
  <div class="matrix-main-container">
    <div class="month-axis">
      <span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span><span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span>
    </div>
    <div class="day-axis"><span>M</span><span>W</span><span>F</span><span>S</span></div>
    <div class="matrix-grid-final">
      {% assign year_start = "now" | date: "%Y-01-01" %}
      {% for i in (0..364) %}
        {% assign day_ts = i | times: 86400 | plus: 1735689600 %}
        {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 色彩逻辑同步计算 {% endcomment %}
          {% assign h = entry.a | minus: 1 | times: 11 | plus: 155 %}
          {% assign l = entry.m | times: 11 | minus: entry.a | times: 6 | plus: 30 %}
          {% assign color = "hsl(" | append: h | append: ", 38%, " | append: l | append: "%)" %}
          {% assign tip = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
        {% else %}
          {% assign color = "#ffffff" %}
          {% assign tip = date_str %}
        {% endif %}
        <div class="cell" style="--bg-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ tip }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="axis-label label-y">Mood (1-5)</div>
    <div class="axis-label label-x">Alcohol (1-5)</div>
    
    <div class="tick tick-y-0">1.0</div><div class="tick tick-y-5">5.0</div>
    <div class="tick tick-x-0">1.0</div><div class="tick tick-x-5">5.0</div>

    <div class="scatter-frame">
      {% for entry in site.data.moods %}
        {% comment %} 
          1. 映射逻辑修复：
          X = (Alcohol - 1) / 4 * 90% + 5%
          Y = (Mood - 1) / 4 * 90% + 5%
        {% endcomment %}
        {% assign x_pos = entry.a | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_val = entry.m | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_pos = 100 | minus: y_val %}

        {% comment %} 
          2. 强化颜色深度：
          增加酒精对明度的扣减，使高酒精度色块更分明
        {% endcomment %}
        {% assign h_dot = entry.a | minus: 1 | times: 11 | plus: 155 %}
        {% assign l_dot = entry.m | times: 11 | minus: entry.a | times: 6 | plus: 30 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 42%, " | append: l_dot | append: "%)" %}

        {% capture tip_scat %}{{ entry.date }}&#10;M: {{ entry.m }} A: {{ entry.a }}{% if entry.note %}&#10;Note: {{ entry.note }}{% endif %}{% endcapture %}

        <div class="data-dot" 
             style="--dot-color: {{ color_dot }}; left: {{ x_pos }}%; top: {{ y_pos }}%;" 
             data-tip="{{ tip_scat }}">
        </div>
      {% endfor %}
    </div>
  </div>

  <div class="matrix-caption-v3" style="margin-top: 35px; font-size: 11px; color: #94a3b8; text-align: left; width: 100%; border-top: 1px solid #e2e8f0; padding-top: 12px;">
    <b>Fig 1.</b> Annual behavioral manifold. <b>Left-bottom dark zone</b> represents high alcohol intake with lower mood (dreamy state); <b>Right-top light zone</b> represents high-lucidity positive state.
  </div>
</div>


About this page
======
You can fork [this template](https://github.com/academicpages/academicpages.github.io) right now, modify the configuration and Markdown files, add your own PDFs and other content, and have your own site for free, with no ads!

For more info
------
More info about configuring Academic Pages can be found in [the guide](https://academicpages.github.io/markdown/), the [growing wiki](https://github.com/academicpages/academicpages.github.io/wiki), and you can always [ask a question on GitHub](https://github.com/academicpages/academicpages.github.io/discussions). The [guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) (which this theme was forked from) might also be helpful.

