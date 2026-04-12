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
    /* --- 1. 基础布局超参数 --- */
    --wall-bg: #f8fafc;
    --text-gray: #78828e;
    --empty-cell: #ffffff;
    
    /* 绿墙尺寸 */
    --cell-w: 13px; --cell-h: 10px; --gap: 3px;

    /* 坐标系尺寸 */
    --scatter-w: 520px; --scatter-h: 320px;
    --dot-size: 14px; --dot-border-col: #ffffff;

    /* --- 2. 颜色映射超参数 (可根据喜好调节) --- */
    --h-start: 155;    /* M/A 为 1 时的基准色相 (绿色) */
    --h-shift: 10;     /* 酒精(A)增加时的色相偏移增量 (向蓝色偏移) */
    --l-base: 30;      /* 基础亮度百分比 */
    --l-m-weight: 12;  /* 心情(M)对亮度的贡献权重 (越大越明亮) */
    --l-a-weight: 6;   /* 酒精(A)对亮度的扣减权重 (越大越深沉) */
  }

  .academic-dashboard-wrapper {
    background: var(--wall-bg); padding: 40px; border-radius: 12px;
    margin: 40px auto; width: fit-content; display: flex;
    flex-direction: column; align-items: center;
    border: 1px solid #e2e8f0; font-family: "Inter", sans-serif;
  }

  /* --- PART 1: Wall (年度数据墙) --- */
  .matrix-main-container {
    display: grid; grid-template-columns: 35px 1fr;
    grid-template-rows: 25px 1fr; gap: 6px; margin-bottom: 50px;
  }

  .month-axis { grid-column: 2; display: flex; justify-content: space-between; font-size: 10px; color: var(--text-gray); }
  .day-axis { grid-row: 2; display: flex; flex-direction: column; justify-content: space-between; font-size: 10px; color: var(--text-gray); height: 88px; }

  .matrix-grid-final {
    display: grid; grid-template-rows: repeat(7, var(--cell-h));
    grid-auto-flow: column; grid-auto-columns: var(--cell-w); gap: var(--gap);
  }

  .cell {
    width: var(--cell-w); height: var(--cell-h); border-radius: 2px;
    background-color: var(--empty-cell); border: 1px solid rgba(0,0,0,0.02);
    position: relative; transition: filter 0.2s;
  }

  /* --- PART 2: Scatter Plot (0-5 坐标系) --- */
  .scatter-plots-container { width: var(--scatter-w); position: relative; }
  .scatter-frame {
    width: var(--scatter-w); height: var(--scatter-h);
    border-left: 2px solid #cbd5e1; border-bottom: 2px solid #cbd5e1;
    position: relative; overflow: visible;
    /* 背景渐变：从左下(深蓝)到右上(明绿) */
    background: linear-gradient(135deg, #1e293b 0%, #334155 35%, #5eead4 100%);
  }

  .axis-label { position: absolute; font-size: 11px; color: var(--text-gray); font-weight: 600; }
  .label-y { left: -60px; top: 50%; transform: translateY(-50%) rotate(-90deg); }
  .label-x { bottom: -35px; left: 50%; transform: translateX(-50%); }

  /* 坐标轴 1-5 刻度 */
  .tick { position: absolute; font-size: 10px; color: #94a3b8; }
  .tick-y-min { bottom: 5%; left: -20px; } .tick-y-max { top: 5%; left: -20px; }
  .tick-x-min { bottom: -20px; left: 5%; } .tick-x-max { bottom: -20px; right: 5%; }

  /* 样本点 */
  .data-dot {
    width: var(--dot-size); height: var(--dot-size); border-radius: 50%;
    border: 2px solid var(--dot-border-col); position: absolute;
    transform: translate(-50%, 50%); z-index: 10;
    box-shadow: 0 2px 8px rgba(0,0,0,0.15); transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }

  .data-dot:hover { transform: translate(-50%, 50%) scale(1.5); z-index: 100; box-shadow: 0 8px 20px rgba(0,0,0,0.25); }

  /* Tooltip 样式 */
  .cell:hover::after, .data-dot:hover::after {
    content: attr(data-tip); position: absolute; background: rgba(30, 41, 59, 0.98);
    color: #fff; padding: 8px 12px; border-radius: 4px; font-size: 10px;
    bottom: 180%; left: 50%; transform: translateX(-50%);
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
      {% assign current_year = "now" | date: "%Y" %}
      {% assign year_start = current_year | append: "-01-01" %}
      
      {% for i in (0..364) %}
        {% assign day_ts = i | times: 86400 | plus: 1735689600 %} {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        
        {% comment %} 核心：从 site.data.moods 中检索匹配日期的 entry {% endcomment %}
        {% assign entry = site.data.moods | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 
            统一色彩算法：
            H = (A-1) * shift + start
            L = (M * weight_m) - (A * weight_a) + base
          {% endcomment %}
          {% assign h = entry.a | minus: 1 | times: 10 | plus: 155 %}
          {% assign l = entry.m | times: 11 | minus: entry.a | times: 6 | plus: 30 %}
          {% assign color = "hsl(" | append: h | append: ", 40%, " | append: l | append: "%)" %}
          {% assign tip = date_str | append: " | M" | append: entry.m | append: " A" | append: entry.a %}
        {% else %}
          {% assign color = "var(--empty-cell)" %}
          {% assign tip = date_str %}
        {% endif %}

        <div class="cell" style="background-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ tip }}"></div>
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="axis-label label-y">Mood (1-5)</div>
    <div class="axis-label label-x">Alcohol (1-5)</div>
    
    <div class="tick tick-y-min">1.0</div><div class="tick tick-y-max">5.0</div>
    <div class="tick tick-x-min">1.0</div><div class="tick tick-x-max">5.0</div>

    <div class="scatter-frame">
      {% for entry in site.data.moods %}
        {% comment %} 
          1. 坐标映射逻辑：
          使用 7% 到 93% 的安全边距，确保圆圈不溢出 
        {% endcomment %}
        {% assign x_pos = entry.a | minus: 1 | divided_by: 4.0 | times: 86 | plus: 7 %}
        {% assign y_raw = entry.m | minus: 1 | divided_by: 4.0 | times: 86 | plus: 7 %}
        {% assign y_pos = 100 | minus: y_raw %}

        {% comment %} 
          2. 颜色映射逻辑 (与 Wall 严格保持 100% 物理一致) 
        {% endcomment %}
        {% assign h_dot = entry.a | minus: 1 | times: 10 | plus: 155 %}
        {% assign l_dot = entry.m | times: 11 | minus: entry.a | times: 6 | plus: 30 %}
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 42%, " | append: l_dot | append: "%)" %}

        {% capture tip_scat %}{{ entry.date }}&#10;M: {{ entry.m }} A: {{ entry.a }}{% if entry.note %}&#10;Note: {{ entry.note }}{% endif %}{% endcapture %}

        <div class="data-dot" 
             style="background-color: {{ color_dot }}; left: {{ x_pos }}%; top: {{ y_pos }}%;" 
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

