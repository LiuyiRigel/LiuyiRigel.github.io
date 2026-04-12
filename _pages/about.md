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
    --wall-bg: #f8fafc;
    --text-gray: #718096;
    --empty-cell: #ffffff;
    --cell-w: 13px; --cell-h: 10px; --gap: 3px;
    --scatter-w: 500px; --scatter-h: 300px;
    --dot-size: 14px;
  }

  .academic-dashboard-container {
    background: var(--wall-bg); padding: 40px; border-radius: 12px;
    margin: 20px auto; width: fit-content; display: flex;
    flex-direction: column; align-items: center;
    border: 1px solid #e2e8f0; font-family: -apple-system, system-ui, sans-serif;
  }

  .matrix-wrapper {
    display: grid; grid-template-columns: 35px 1fr;
    grid-template-rows: 25px 1fr; gap: 6px; margin-bottom: 50px;
  }
  .month-labels { grid-column: 2; display: flex; justify-content: space-between; font-size: 10px; color: var(--text-gray); }
  .day-labels { grid-row: 2; display: flex; flex-direction: column; justify-content: space-between; font-size: 10px; color: var(--text-gray); height: 88px; }
  .matrix-grid { display: grid; grid-template-rows: repeat(7, var(--cell-h)); grid-auto-flow: column; gap: var(--gap); }
  .cell { width: var(--cell-w); height: var(--cell-h); border-radius: 2px; background: var(--empty-cell); border: 1px solid rgba(0,0,0,0.03); position: relative; }

  .scatter-plots { width: var(--scatter-w); position: relative; margin-top: 20px;}
  .scatter-canvas {
    width: var(--scatter-w); height: var(--scatter-h);
    border-left: 2px solid #cbd5e1; border-bottom: 2px solid #cbd5e1;
    position: relative; overflow: visible;
    background: linear-gradient(135deg, #1e293b 0%, #334155 35%, #5eead4 100%);
  }
  .axis-title { position: absolute; font-size: 11px; color: var(--text-gray); font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
  .title-y { left: -85px; top: 50%; transform: translateY(-50%) rotate(-90deg); width: 150px; text-align: center; }
  .title-x { bottom: -45px; left: 50%; transform: translateX(-50%); }
  
  .axis-tick { position: absolute; font-size: 10px; color: #94a3b8; }
  .tick-y1 { bottom: 5%; left: -25px; } .tick-y5 { top: 5%; left: -25px; }
  .tick-x1 { bottom: -25px; left: 5%; } .tick-x5 { bottom: -25px; right: 5%; }

  .dot {
    width: var(--dot-size); height: var(--dot-size); border-radius: 50%; border: 2px solid #ffffff;
    position: absolute; transform: translate(-50%, 50%); z-index: 10;
    box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }
  .dot:hover { transform: translate(-50%, 50%) scale(1.6); z-index: 100; box-shadow: 0 8px 24px rgba(0,0,0,0.3); }

  .cell:hover::after, .dot:hover::after {
    content: attr(data-tip); position: absolute; background: #1a202c; color: #fff;
    padding: 6px 10px; border-radius: 4px; font-size: 10px; bottom: 180%; left: 50%;
    transform: translateX(-50%); white-space: pre; z-index: 200; pointer-events: none;
  }
</style>

<div class="academic-dashboard-container">
  <div class="matrix-wrapper">
    <div class="month-labels"><span>Jan</span><span>Feb</span><span>Mar</span><span>Apr</span><span>May</span><span>Jun</span><span>Jul</span><span>Aug</span><span>Sep</span><span>Oct</span><span>Nov</span><span>Dec</span></div>
    <div class="day-labels"><span>M</span><span>W</span><span>F</span><span>S</span></div>
    <div class="matrix-grid">
      {% comment %} 动态获取当前年份的第一天时间戳 {% endcomment %}
      {% assign current_year = "now" | date: "%Y" %}
      {% assign year_start_str = current_year | append: "-01-01" %}
      {% assign start_ts = year_start_str | date: "%s" | plus: 0 %}
      
      {% for i in (0..365) %}
        {% assign day_offset = i | times: 86400 %}
        {% assign day_ts = start_ts | plus: day_offset %}
        {% assign d_str = day_ts | date: "%Y-%m-%d" %}
        {% assign d_ow = day_ts | date: "%u" %}
        
        {% assign day_entry = site.data.moods | where: "date", d_str | first %}

        {% if day_entry %}
          {% assign m_f = day_entry.m | plus: 0.0 %}
          {% assign a_f = day_entry.a | plus: 0.0 %}
          
          {% comment %} 颜色算法修复：Liquid 没有乘法优先级，必须分步计算 {% endcomment %}
          {% assign hue = a_f | minus: 1.0 | times: 11.25 | plus: 155.0 %}
          
          {% assign lit_part1 = m_f | times: 11.0 %}
          {% assign lit_part2 = a_f | times: 7.0 %}
          {% assign lit = lit_part1 | minus: lit_part2 | plus: 30.0 %}
          
          {% assign cell_color = "hsl(" | append: hue | append: ", 40%, " | append: lit | append: "%)" %}
          
          <div class="cell" style="background-color: {{ cell_color }}; grid-row: {{ d_ow }};" data-tip="{{ d_str }}&#10;Mood: {{ day_entry.m }} Alcohol: {{ day_entry.a }}"></div>
        {% else %}
          {% comment %} 填充空白格保证网格不乱 {% endcomment %}
          <div class="cell" style="grid-row: {{ d_ow }};"></div>
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

        {% comment %} 坐标映射逻辑修正 {% endcomment %}
        {% assign x_coord = a_val | minus: 1.0 | divided_by: 4.0 | times: 90.0 | plus: 5.0 %}
        {% assign y_raw = m_val | minus: 1.0 | divided_by: 4.0 | times: 90.0 | plus: 5.0 %}
        {% assign y_coord = 100.0 | minus: y_raw %}

        {% comment %} 颜色映射逻辑同步修复 {% endcomment %}
        {% assign h_dot = a_val | minus: 1.0 | times: 11.25 | plus: 155.0 %}
        
        {% assign l_part1 = m_val | times: 11.0 %}
        {% assign l_part2 = a_val | times: 7.0 %}
        {% assign l_dot = l_part1 | minus: l_part2 | plus: 30.0 %}
        
        {% assign color_dot = "hsl(" | append: h_dot | append: ", 42%, " | append: l_dot | append: "%)" %}

        <div class="dot" 
             style="background-color: {{ color_dot }}; left: {{ x_coord }}%; top: {{ y_coord }}%;" 
             data-tip="{{ item.date }}&#10;M: {{ item.m }} A: {{ item.a }}{% if item.note %}&#10;Note: {{ item.note }}{% endif %}">
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

