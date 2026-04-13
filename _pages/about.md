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


<div class="mood-dashboard-embed-context">
  <style>
    :root {
      --wall-bg: #f8fafc;
      --text-gray: #718096;
      --empty-cell: #ffffff;
      --cell-w: 14px; --cell-h: 11px; --gap: 3px;
      --scatter-w: 600px; --scatter-h: 350px;
      --dot-size: 14px;
    }

    .mood-dashboard-embed-context {
      background: var(--wall-bg); 
      padding: 50px 40px; 
      border-radius: 16px;
      margin: 20px auto; 
      width: 95%; max-width: 900px;
      display: flex; flex-direction: column; align-items: center;
      border: 1px solid #e2e8f0; 
      font-family: -apple-system, system-ui, sans-serif;
      box-sizing: border-box;
      box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05);
    }

    /* --- Wall 布局：自适应不滚动 --- */
    .matrix-wrapper {
      display: grid; 
      grid-template-columns: auto 1fr; 
      grid-template-rows: 25px 1fr; 
      column-gap: 12px; row-gap: 8px;
      margin-bottom: 60px;
      width: 100%;
    }

    .month-labels { 
      grid-column: 2; 
      display: flex; justify-content: space-between;
      font-size: 10px; color: var(--text-gray); 
    }

    .day-labels { 
      grid-row: 2; 
      display: flex; flex-direction: column; justify-content: space-between; 
      font-size: 10px; color: var(--text-gray); 
      height: calc(var(--cell-h) * 7 + var(--gap) * 6);
    }

    .matrix-grid { 
      display: grid; 
      grid-template-rows: repeat(7, var(--cell-h)); 
      grid-auto-flow: column; 
      gap: var(--gap);
      width: 100%;
    }

    .cell { 
      width: 100%; /* 关键：自适应宽度 */
      min-width: 2px;
      max-width: var(--cell-w);
      height: var(--cell-h); 
      border-radius: 2px; 
      background: var(--empty-cell); 
      border: 1px solid rgba(0,0,0,0.02); 
      position: relative; 
      transition: all 0.2s ease;
      cursor: crosshair;
    }
    .cell:hover { transform: scale(1.6); z-index: 100; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }

    /* --- Scatter 布局：修复交互优先级 --- */
    .scatter-plots { 
      width: 100%; max-width: var(--scatter-w);
      position: relative; margin-top: 20px; 
    }

    .scatter-canvas {
      width: 100%; height: var(--scatter-h);
      border-left: 2px solid #cbd5e1; border-bottom: 2px solid #cbd5e1;
      position: relative; overflow: visible;
      background: linear-gradient(to bottom right, #5eead4 0%, #334155 65%, #1e293b 100%);
      border-radius: 0 0 0 4px;
    }

    .axis-title { 
      position: absolute; font-size: 11px; color: var(--text-gray); 
      font-weight: 700; text-transform: uppercase; letter-spacing: 1px;
    }
    .title-y { left: -90px; top: 50%; transform: translateY(-50%) rotate(-90deg); width: 120px; text-align: center; }
    .title-x { bottom: -50px; left: 50%; transform: translateX(-50%); }
    
    .axis-tick { position: absolute; font-size: 10px; color: #94a3b8; font-family: monospace; }
    .tick-y1 { bottom: 5%; left: -25px; } .tick-y5 { top: 5%; left: -25px; }
    .tick-x1 { bottom: -25px; left: 5%; } .tick-x5 { bottom: -25px; right: 5%; }

    /* 修复：样本点悬停浮动与优先级 */
    .dot {
      width: var(--dot-size); height: var(--dot-size); 
      border-radius: 50%; border: 2px solid #ffffff;
      position: absolute; 
      transform: translate(-50%, -50%); 
      z-index: 10;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2); 
      transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
      cursor: pointer;
    }
    
    .dot:hover { 
      transform: translate(-50%, -50%) scale(1.8); 
      z-index: 500; /* 极高优先级 */
      box-shadow: 0 12px 24px rgba(0,0,0,0.4);
      filter: brightness(1.1);
    }

    /* 统一 Tooltip 样式与优先级修复 */
    [data-tip] { position: relative; }
    [data-tip]::after {
      content: attr(data-tip); 
      position: absolute; 
      background: rgba(15, 23, 42, 0.9); 
      -webkit-backdrop-filter: blur(4px); backdrop-filter: blur(4px);
      color: #fff;
      padding: 10px 14px; 
      border-radius: 8px; 
      font-size: 11px; line-height: 1.5;
      bottom: 180%; left: 50%;
      transform: translateX(-50%) scale(0.8); 
      white-space: pre-wrap;
      z-index: 1000; /* 全局最高 */
      pointer-events: none;
      box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
      opacity: 0;
      transition: all 0.2s ease;
      width: max-content; max-width: 220px;
    }
    [data-tip]:hover::after {
      opacity: 1;
      transform: translateX(-50%) scale(1);
    }

    /* --- 移动端策略：纵向适配与比例调整 --- */
    @media (max-width: 768px) {
      .mood-dashboard-embed-context { padding: 30px 20px; }
      :root {
        --cell-h: 8px;
        --scatter-h: 280px;
        --dot-size: 12px;
      }
      .matrix-wrapper { column-gap: 6px; }
      .title-y { left: -75px; font-size: 9px; }
      .title-x { bottom: -40px; font-size: 9px; }
      .day-labels span:nth-child(even) { display: none; } /* 手机端隐藏部分星期标签节省空间 */
    }
  </style>

  <div class="matrix-wrapper">
    <div class="month-labels">
      <span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span>
      <span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span>
    </div>
    <div class="day-labels"><span>M</span><span>W</span><span>F</span><span>S</span></div>
    <div class="matrix-grid">
      {% assign start_ts = "2026-01-01" | date: "%s" | plus: 0 %}
      {% for i in (0..364) %}
        {% assign d_ts = i | times: 86400 | plus: start_ts %}
        {% assign d_str = d_ts | date: "%Y-%m-%d" %}
        {% assign d_ow = d_ts | date: "%u" %}
        {% assign entry = site.data.moods | where: "date", d_str | first %}

        {% if entry %}
          {% assign m_f = entry.m | plus: 0.0 %}
          {% assign a_f = entry.a | plus: 0.0 %}
          {% assign hue = a_f | minus: 1.0 | times: 11.25 | plus: 155.0 %}
          {% assign l_p1 = m_f | times: 11.0 %}
          {% assign l_p2 = a_f | times: 7.0 %}
          {% assign lit = l_p1 | minus: l_p2 | plus: 30.0 %}
          {% assign color = "hsl(" | append: hue | append: ", 45%, " | append: lit | append: "%)" %}
          <div class="cell" style="background-color: {{ color }}; grid-row: {{ d_ow }};" data-tip="{{ d_str }}&#10;Mood: {{ entry.m }} Alcohol: {{ entry.a }}"></div>
        {% else %}
          <div class="cell" style="grid-row: {{ d_ow }};" data-tip="{{ d_str }}&#10;Mood: N/A Alcohol: N/A"></div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots">
    <div class="axis-title title-y">Positive Mood</div>
    <div class="axis-title title-x">Alcohol Consumption</div>
    <div class="axis-tick tick-y1">1.0</div><div class="axis-tick tick-y5">5.0</div>
    <div class="axis-tick tick-x1">1.0</div><div class="axis-tick tick-x5">5.0</div>

    <div class="scatter-canvas">
      {% for item in site.data.moods %}
        {% assign m_val = item.m | plus: 0.0 %}
        {% assign a_val = item.a | plus: 0.0 %}
        {% assign x = a_val | minus: 1.0 | divided_by: 4.0 | times: 90.0 | plus: 5.0 %}
        {% assign y_raw = m_val | minus: 1.0 | divided_by: 4.0 | times: 90.0 | plus: 5.0 %}
        {% assign y = 100.0 | minus: y_raw %}

        {% assign h_dot = a_val | minus: 1.0 | times: 11.25 | plus: 155.0 %}
        {% assign l_d1 = m_val | times: 11.0 %}
        {% assign l_d2 = a_val | times: 7.0 %}
        {% assign l_dot = l_d1 | minus: l_d2 | plus: 30.0 %}
        {% assign c_dot = "hsl(" | append: h_dot | append: ", 50%, " | append: l_dot | append: "%)" %}

        {% capture scatter_tip %}{{ item.date }}&#10;M: {{ item.m }} A: {{ item.a }}{% if item.note %}&#10;Note: {{ item.note }}{% endif %}{% endcapture %}

        <div class="dot" 
             style="background-color: {{ c_dot }}; left: {{ x }}%; top: {{ y }}%;" 
             data-tip="{{ scatter_tip }}">
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

