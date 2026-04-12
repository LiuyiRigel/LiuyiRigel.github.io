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
    /* --- 调色盘参数：直接对应 image_1.png 的梦幻感 --- */
    --h-base: 155;     /* 绿基准 */
    --h-range: 40;     /* 酒精带来的色相偏移量 */
    --l-floor: 25%;    /* 最深亮度 (低M/高A) */
    --l-ceiling: 80%;  /* 最高亮度 (高M/低A) */
    
    --text-gray: #78828e;
    --dot-border: #ffffff;
  }

  .academic-dashboard-wrapper {
    background: #f8fafc; padding: 40px; border-radius: 12px;
    margin: 40px auto; width: fit-content; display: flex;
    flex-direction: column; align-items: center;
    border: 1px solid #e2e8f0; font-family: sans-serif;
  }

  /* --- Wall 样式 --- */
  .matrix-main-container {
    display: grid; grid-template-columns: 35px 1fr;
    grid-template-rows: 25px 1fr; gap: 6px; margin-bottom: 50px;
  }
  .month-axis { grid-column: 2; display: flex; justify-content: space-between; font-size: 10px; color: var(--text-gray); }
  .day-axis { grid-row: 2; display: flex; flex-direction: column; justify-content: space-between; font-size: 10px; color: var(--text-gray); height: 81px; }
  .matrix-grid-final { display: grid; grid-template-rows: repeat(7, 9px); grid-auto-flow: column; grid-auto-columns: 12px; gap: 3px; }
  .cell { width: 12px; height: 9px; border-radius: 2px; background-color: #fff; border: 1px solid rgba(0,0,0,0.03); position: relative; }

  /* --- Scatter 样式 --- */
  .scatter-plots-container { width: 500px; position: relative; }
  .scatter-frame {
    width: 500px; height: 300px; border-left: 2px solid #cbd5e1; border-bottom: 2px solid #cbd5e1;
    position: relative; overflow: visible;
    /* 渐变对齐：从左下深 Midnight Blue 到右上亮 Mint Green */
    background: linear-gradient(135deg, #1e293b 0%, #334155 35%, #5eead4 100%);
  }
  .axis-label { position: absolute; font-size: 11px; color: var(--text-gray); font-weight: 600; }
  .label-y { left: -60px; top: 50%; transform: translateY(-50%) rotate(-90deg); }
  .label-x { bottom: -35px; left: 50%; transform: translateX(-50%); }
  
  .tick { position: absolute; font-size: 10px; color: #94a3b8; }
  .tick-y-1 { bottom: 5%; left: -20px; } .tick-y-5 { top: 5%; left: -20px; }
  .tick-x-1 { bottom: -20px; left: 5%; } .tick-x-5 { bottom: -20px; right: 5%; }

  .data-dot {
    width: 14px; height: 14px; border-radius: 50%; border: 2px solid var(--dot-border);
    position: absolute; transform: translate(-50%, 50%); z-index: 10;
    box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: transform 0.2s;
  }
  .data-dot:hover { transform: translate(-50%, 50%) scale(1.5); z-index: 100; }

  /* Tooltip */
  .cell:hover::after, .data-dot:hover::after {
    content: attr(data-tip); position: absolute; background: #1e293b; color: #fff;
    padding: 6px 10px; border-radius: 4px; font-size: 10px; bottom: 180%; left: 50%;
    transform: translateX(-50%); white-space: pre; z-index: 200;
  }
</style>

<div class="academic-dashboard-wrapper">
  
  <div class="matrix-main-container">
    <div class="month-axis"><span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span><span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span></div>
    <div class="day-axis"><span>M</span><span>W</span><span>F</span><span>S</span></div>
    <div class="matrix-grid-final">
      {% assign moods_data = site.data.moods %}
      {% for i in (0..364) %}
        {% assign day_ts = i | times: 86400 | plus: 1735689600 %}
        {% assign date_str = day_ts | date: "%Y-%m-%d" %}
        {% assign dow = day_ts | date: "%u" %}
        
        {% comment %} 修正检索：Liquid 必须精准匹配 {% endcomment %}
        {% assign entry = moods_data | where: "date", date_str | first %}

        {% if entry %}
          {% comment %} 强制转换为数字避免映射错误 {% endcomment %}
          {% assign m_val = entry.m | plus: 0.0 %}
          {% assign a_val = entry.a | plus: 0.0 %}
          
          {% assign h = a_val | minus: 1 | times: 11 | plus: 155 %}
          {% assign l = m_val | times: 11 | minus: a_val | times: 7 | plus: 30 %}
          {% assign color = "hsl(" | append: h | append: ", 40%, " | append: l | append: "%)" %}
          <div class="cell" style="background-color: {{ color }}; grid-row: {{ dow }};" data-tip="{{ date_str }} M:{{ entry.m }} A:{{ entry.a }}"></div>
        {% else %}
          <div class="cell" grid-row: {{ dow }}></div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="scatter-plots-container">
    <div class="axis-label label-y">Mood (1-5)</div>
    <div class="axis-label label-x">Alcohol (1-5)</div>
    <div class="tick tick-y-1">1.0</div><div class="tick tick-y-5">5.0</div>
    <div class="tick tick-x-1">1.0</div><div class="tick tick-x-5">5.0</div>

    <div class="scatter-frame">
      {% for item in site.data.moods %}
        {% assign m = item.m | plus: 0.0 %}
        {% assign a = item.a | plus: 0.0 %}

        {% comment %} 坐标映射修复：1.0->5%, 5.0->95% {% endcomment %}
        {% assign x_perc = a | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_raw = m | minus: 1 | divided_by: 4.0 | times: 90 | plus: 5 %}
        {% assign y_perc = 100 | minus: y_raw %}

        {% comment %} 颜色映射与 Wall 完全一致 {% endcomment %}
        {% assign h_dot = a | minus: 1 | times: 11 | plus: 155 %}
        {% assign l_dot = m | times: 11 | minus: a | times: 7 | plus: 30 %}
        {% assign c_dot = "hsl(" | append: h_dot | append: ", 42%, " | append: l_dot | append: "%)" %}

        <div class="data-dot" style="background-color: {{ c_dot }}; left: {{ x_perc }}%; top: {{ y_perc }}%;" 
             data-tip="{{ item.date }}&#10;M: {{ item.m }} A: {{ item.a }}"></div>
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

