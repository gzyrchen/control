<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>二阶系统单位阶跃响应 · 交互动画</title>
<style>
:root{
  --color-text-primary:#2C2C2A;
  --color-text-secondary:#5F5E5A;
  --color-text-tertiary:#888780;
  --color-background-secondary:#F1EFE8;
  --color-border-tertiary:rgba(0,0,0,.15);
  --font-sans:-apple-system,BlinkMacSystemFont,"Segoe UI","Microsoft YaHei","PingFang SC",sans-serif;
  --font-mono:ui-monospace,Consolas,"Courier New",monospace;
  --border-radius-md:8px;
}
body{margin:0;padding:24px;background:#fff;font-family:var(--font-sans);color:var(--color-text-primary)}
.wrap{max-width:820px;margin:0 auto}
h1{font-size:15px;font-weight:500;margin:0 0 6px}
p.lead{font-size:13px;line-height:1.6;color:var(--color-text-secondary);margin:0 0 16px}
input[type=range]{accent-color:#185FA5}
input[type=checkbox]{accent-color:#185FA5}
select{font-family:inherit;font-size:13px;padding:3px 6px;border:0.5px solid rgba(0,0,0,.3);border-radius:6px;background:transparent;color:var(--color-text-primary)}
button{font-family:inherit;font-size:13px;padding:5px 14px;border:0.5px solid rgba(0,0,0,.3);border-radius:8px;background:transparent;color:var(--color-text-primary);cursor:pointer}
button:hover{background:var(--color-background-secondary)}
.note{font-size:12px;line-height:1.85;color:var(--color-text-secondary);margin-top:22px;padding-top:14px;border-top:0.5px solid rgba(0,0,0,.15)}
.note h2{font-size:13px;font-weight:500;color:var(--color-text-primary);margin:14px 0 4px}
.note p{margin:4px 0}
.note ol,.note ul{margin:4px 0;padding-left:22px}
.note code{font-family:var(--font-mono);font-size:11px;background:var(--color-background-secondary);padding:1px 4px;border-radius:3px}
.gr{stroke:#F1EFE8;stroke-width:1;fill:none}
.ax2{stroke:#B4B2A9;stroke-width:1;fill:none}
.kl{font-family:var(--font-sans);font-size:11px;fill:var(--color-text-tertiary)}
.lb{font-family:var(--font-sans);font-size:12px;fill:var(--color-text-secondary)}
.tt{font-family:var(--font-sans);font-size:12px;font-weight:500;fill:var(--color-text-primary)}
.card{background:var(--color-background-secondary);border-radius:var(--border-radius-md);padding:10px 12px}
.cl{font-size:12px;color:var(--color-text-secondary)}
.cv{font-size:22px;font-weight:500;line-height:1.3;font-variant-numeric:tabular-nums}
.cs{font-size:11px;color:var(--color-text-tertiary);font-family:var(--font-mono)}
.param{display:flex;flex-direction:column;gap:4px}
.phead{display:flex;justify-content:space-between;align-items:baseline;gap:12px;font-size:12px;color:var(--color-text-secondary)}
.pval{font-family:var(--font-mono);font-size:14px;font-weight:500;color:var(--color-text-primary);font-variant-numeric:tabular-nums}
.formula{background:var(--color-background-secondary);border-radius:var(--border-radius-md);padding:10px 12px;margin-top:10px;line-height:1.6}
.ftag{font-size:11px;color:var(--color-text-secondary);margin-right:10px}
.fsym{font-size:11px;font-family:var(--font-mono);color:var(--color-text-secondary)}
.fnum{font-size:13px;font-weight:500;font-family:var(--font-mono);color:var(--color-text-primary);overflow-wrap:anywhere}
</style>
</head>
<body>
<div class="wrap">
<h1>二阶系统 · 单位阶跃响应</h1>
<p class="lead">调节阻尼比 ζ（0.01~2）与自然振荡角频率 ωₙ（0.1~10 rad/s），实时计算上升时间、峰值时间、超调量、调节时间并绘制响应波形。播放时右侧弹簧质量块与波形时间游标同步运动；在波形图上点击或拖动可定位任意时刻。</p>
<div style="display:flex;align-items:baseline;gap:10px;margin-bottom:8px">
<span style="font-size:15px;font-weight:500">二阶系统 · 单位阶跃响应</span>
<span style="font-size:11px;color:var(--color-text-tertiary);font-family:var(--font-mono)">Φ(s) = ωₙ² / (s² + 2ζωₙs + ωₙ²)</span>
</div>
<div style="display:grid;grid-template-columns:minmax(0,440fr) minmax(0,200fr);gap:14px;align-items:start">
<svg id="plot" viewBox="0 0 440 280" width="100%" role="img" aria-label="二阶系统单位阶跃响应波形，含误差带、峰值点、调节时刻与时间游标" style="display:block;touch-action:none">
<title>单位阶跃响应波形</title>
<desc>蓝色曲线为输出响应，浅蓝带为调节误差带，橙色竖线为时间游标，可点击拖动定位。</desc>
<path class="gr" d="M46 187.6H420 M46 135.2H420 M46 82.9H420 M46 30.5H420"/>
<path class="ax2" d="M46 20V240 M46 240H420"/>
<text class="kl" x="38" y="244" text-anchor="end">0</text>
<text class="kl" x="38" y="191.6" text-anchor="end">0.5</text>
<text class="kl" x="38" y="139.2" text-anchor="end">1.0</text>
<text class="kl" x="38" y="86.9" text-anchor="end">1.5</text>
<text class="kl" x="38" y="34.5" text-anchor="end">2.0</text>
<text class="kl" id="xk0" x="46" y="254" text-anchor="middle">0</text>
<text class="kl" id="xk1" x="139.5" y="254" text-anchor="middle">2.0</text>
<text class="kl" id="xk2" x="233" y="254" text-anchor="middle">4.0</text>
<text class="kl" id="xk3" x="326.5" y="254" text-anchor="middle">6.0</text>
<text class="kl" id="xk4" x="420" y="254" text-anchor="middle">8.0</text>
<text class="kl" x="420" y="270" text-anchor="end">t / s</text>
<text class="lb" x="50" y="32">c(t)</text>
<rect id="bandRect" x="46" y="130" width="374" height="10.5" fill="#E6F1FB" stroke="#85B7EB" stroke-width="0.5"/>
<path d="M46 240V135.2H420" fill="none" stroke="#B4B2A9" stroke-width="1" stroke-dasharray="3 4"/>
<text class="kl" x="52" y="130">u(t) = 1</text>
<path id="curve" fill="none" stroke="#185FA5" stroke-width="2" stroke-linejoin="round"/>
<line id="tsLine" stroke="#D85A30" stroke-width="1" stroke-dasharray="4 4" opacity="0.65" y1="20" y2="240"/>
<text id="tsLab" class="kl" style="fill:#993C1D" y="28">t_s</text>
<circle id="trDot" r="4" fill="#FFFFFF" stroke="#185FA5" stroke-width="2"/>
<circle id="tpDot" r="4.5" fill="#D85A30"/>
<text id="tpLab" class="kl" style="fill:#993C1D">σ</text>
<line id="cursorLine" stroke="#D85A30" stroke-width="1" opacity="0.45" y1="20" y2="240"/>
<circle id="cursorDot" r="4.5" fill="#D85A30"/>
</svg>
<svg id="massSvg" viewBox="0 0 200 280" width="100%" role="img" aria-label="弹簧阻尼质量块类比，质量块位移实时对应输出响应">
<title>弹簧阻尼类比</title>
<desc>质量块位置对应输出 c(t)，可直观看到冲过目标位置与回摆停稳。</desc>
<path d="M18 8H182" stroke="#B4B2A9" stroke-width="2" fill="none"/>
<line x1="18" y1="135.2" x2="182" y2="135.2" stroke="#85B7EB" stroke-width="1" stroke-dasharray="4 4"/>
<line x1="18" y1="240" x2="182" y2="240" stroke="#D3D1C7" stroke-width="1" stroke-dasharray="3 4"/>
<path id="spring" fill="none" stroke="#5F5E5A" stroke-width="1.5"/>
<rect id="cylBg" x="121" y="192" width="22" height="32" fill="#FFFFFF" stroke="none"/>
<line id="dash" x1="132" x2="132" y1="8" y2="205" stroke="#888780" stroke-width="1.5"/>
<line id="pistonBar" x1="122" x2="142" y1="205" y2="205" stroke="#5F5E5A" stroke-width="2.5"/>
<path id="cyl" fill="none" stroke="#888780" stroke-width="1.2" d="M121 192V224H143V192"/>
<rect id="block" x="40" y="224" width="120" height="32" rx="4" fill="#E6F1FB" stroke="#185FA5" stroke-width="1.5"/>
<text id="swingArrow" class="tt" x="172" y="240" text-anchor="middle" style="font-size:15px;fill:#B4B2A9;paint-order:stroke;stroke:#FFFFFF;stroke-width:3px;stroke-linejoin:round" aria-hidden="true">·</text>
<circle id="jointS" cx="68" cy="224" r="2.5" fill="#5F5E5A"/>
<circle id="jointD" cx="132" cy="224" r="2.5" fill="#5F5E5A"/>
<text class="kl" x="18" y="130.2" style="fill:#185FA5;paint-order:stroke;stroke:#FFFFFF;stroke-width:3px;stroke-linejoin:round">目标 1</text>
<text class="kl" x="18" y="236" style="paint-order:stroke;stroke:#FFFFFF;stroke-width:3px;stroke-linejoin:round">起始 0</text>
<text id="cval" class="tt" x="100" y="268" text-anchor="middle">c(t) = 0.000</text>
</svg>
</div>
<div class="formula">
<div><span class="ftag" id="fTag">欠阻尼 0&lt;ζ&lt;1</span><span class="fsym" id="fSym">c(t) = 1 − e^(−ζωₙt)/√(1−ζ²) · sin(ω_d t + β)</span></div>
<div class="fnum" id="fNum">c(t) = 1 − 1.091·e^(−0.800t)·sin(1.833t + 1.159)</div>
</div>
<div style="display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:10px;margin-top:10px">
<div class="card"><div class="cl">上升时间 t<sub>r</sub></div><div class="cv" id="vTr">—</div><div class="cs" id="sTr">(π−β)/ω_d</div></div>
<div class="card"><div class="cl">峰值时间 t<sub>p</sub></div><div class="cv" id="vTp">—</div><div class="cs" id="sTp">π/ω_d</div></div>
<div class="card"><div class="cl">超调量 σ%</div><div class="cv" id="vSg">—</div><div class="cs" id="sSg">e^(−πζ/√(1−ζ²))</div></div>
<div class="card"><div class="cl">调节时间 t<sub>s</sub></div><div class="cv" id="vTs">—</div><div class="cs" id="sTs">严格数值解</div></div>
</div>
<div style="display:flex;align-items:flex-end;gap:12px;flex-wrap:wrap;font-size:13px;margin-top:12px">
<div class="param" style="width:158px"><div class="phead"><span>阻尼比 ζ</span><span class="pval" id="vZeta">0.40</span></div><input id="zR" type="range" min="0.01" max="2" step="0.01" value="0.4" style="width:100%"></div>
<div class="param" style="width:158px"><div class="phead"><span>自然振荡角频率 ωₙ</span><span class="pval" id="vWn">2.0</span></div><input id="wR" type="range" min="0.1" max="10" step="0.1" value="2" style="width:100%"></div>
<label style="display:flex;align-items:center;gap:7px;white-space:nowrap">误差带 <select id="band"><option value="0.05">±5%</option><option value="0.02">±2%</option></select></label>
<label style="display:flex;align-items:center;gap:6px;white-space:nowrap"><input id="axmode" type="checkbox" checked>固定 8 s</label>
<label style="display:flex;align-items:center;gap:7px;white-space:nowrap">倍速 <select id="speed"><option value="1">1× 实时</option><option value="2">2×</option><option value="5">5×</option><option value="20">20×</option><option value="100">100×</option></select></label>
<div style="display:flex;align-items:center;gap:12px;white-space:nowrap">
<label style="display:flex;align-items:center;gap:6px" title="跟着质量块来回滑动的口哨声；过快或过慢会自动静音"><input id="snd" type="checkbox" checked>音效</label>
<label style="display:flex;align-items:center;gap:7px" title="只改哨音的音区，不改变节拍与门限">哨音 <select id="sndRange"><option value="0">低沉</option><option value="1" selected>标准</option><option value="2">清亮</option></select></label>
</div>
<div style="display:flex;gap:8px"><button id="play" type="button">暂停</button><button id="reset" type="button">复位</button></div>
</div>
<div style="font-size:11px;color:var(--color-text-tertiary);margin-top:6px"><span id="regime">欠阻尼 0 &lt; ζ &lt; 1</span> · <span id="speedTag">1× 实时</span> · <span id="sndTag">音效待激活</span> · 点击或拖动波形图可定位游标</div>

<div class="note">
<h2>模型与响应解析式</h2>
<p>闭环传递函数 <code>Φ(s) = ωₙ² / (s² + 2ζωₙs + ωₙ²)</code>，输入为单位阶跃 1(t)，零初始条件。</p>
<p>欠阻尼 0&lt;ζ&lt;1：<code>c(t) = 1 − e^(−ζωₙt)/√(1−ζ²) · sin(ω_d t + β)</code>，其中 <code>ω_d = ωₙ√(1−ζ²)</code>，<code>β = arccos ζ</code>。</p>
<p>临界阻尼 ζ=1：<code>c(t) = 1 − (1+ωₙt)e^(−ωₙt)</code>。</p>
<p>过阻尼 ζ&gt;1：<code>c(t) = 1 − [p₂e^(−p₁t) − p₁e^(−p₂t)]/(p₂−p₁)</code>，<code>p₁,₂ = ωₙ(ζ ∓ √(ζ²−1))</code>。</p>
<p>波形图下方的解析式随参数实时更新：第一行给当前阻尼状态下的<b>符号形式</b>，第二行给<b>代入 ζ、ωₙ 后的数值形式</b>。拖动滑块时两者同步变化，可直接与曲线对照验算；阻尼状态跨越 ζ=1 时公式形式会自动切换。</p>

<h2>四项指标的定义域（最容易算错的地方）</h2>
<ul>
<li><b>上升时间 t<sub>r</sub></b>：响应<b>首次到达稳态值 1</b> 的时间，<code>t_r = (π−β)/ω_d</code>。ζ ≥ 1 时响应单调渐近、永不穿越 1，按此定义<b>不适用</b>，页面显示「—」。注意教材另有 10%→90% 定义，换定义后 ζ ≥ 1 也有值，数值不同。</li>
<li><b>峰值时间 t<sub>p</sub></b>：<code>π/ω_d</code>；<b>超调量 σ%</b>：<code>e^(−πζ/√(1−ζ²))×100%</code>。二者仅对欠阻尼成立；ζ ≥ 1 时无峰值、超调为 0。</li>
<li><b>调节时间 t<sub>s</sub></b>：响应<b>最后一次离开 ±Δ 误差带</b>的时刻。本页用 6000~60000 点数值搜索求严格解，不套近似公式。卡片下方给出教材近似 <code>3/(ζωₙ)</code>（Δ=5%）或 <code>4/(ζωₙ)</code>（Δ=2%）作对照——两者有偏差是正常的，近似式来自包络线，属保守估计。</li>
</ul>

<h2>类比物：弹簧–阻尼器–质量块</h2>
<p>标准二阶方程 <code>ẍ + 2ζωₙẋ + ωₙ²x = ωₙ²u</code> 本来就是弹簧质量系统的方程，所以这不是硬凑的比喻。</p>
<ul>
<li>质量块位移 ↔ 输出 c(t)；把目标位置突然拨到 1 ↔ 单位阶跃输入</li>
<li>弹簧刚度 k ↔ <code>ωₙ = √(k/m)</code>；阻尼系数 c ↔ <code>ζ = c/(2√(km))</code></li>
<li>冲过目标位置的距离 ↔ 超调量；停稳不再明显晃 ↔ 调节时间</li>
</ul>
<p><b>为什么两个元件都必须连到质量块：</b>弹簧与阻尼器是<b>并联</b>接在固定端与质量块之间，下端都落在块顶（图中两个连接圆点），位移同为 x(t)、力相加，这才对应 <code>mẍ + cẋ + kx = F</code>，归一化后才是本页的 <code>ẍ + 2ζωₙẋ + ωₙ²x = ωₙ²u</code>。若阻尼器下端悬空，两端无相对运动，就产生不了 <code>cẋ</code> 项，图与方程自相矛盾——画示意图时这是必须守住的一条。</p>
<p><b>阻尼器采用标准筒式符号：</b>细竖线是固定于天花板的活塞杆，杆端横线是活塞；下方开口矩形是缸体，缸底坐落在质量块顶面，顶部的连接圆点即为此处。质量块上下运动时活塞在缸内相对滑动，节流产生与相对速度成正比的阻尼力 <code>F_c = c·ẋ</code>——这就是 <code>cẋ</code> 项的物理来源。极端超调时行程变短，缸体高度与弹簧折点数随行程自适应，但两端连接关系始终不变。</p>
<p>右侧质量块中心与左侧波形上的游标点使用<b>同一个纵向映射 Y(c) = 240 − c/2.1·220</b>，且两幅图按相同比例等比缩放，因此任意窗口宽度下两者严格共线；右侧「目标 1」参考线与左侧稳态线（c=1）也处于同一水平线。这是绘图约束，不是视觉近似。</p>

<h2>音效：跟着质量块吹口哨</h2>
<p>勾选「音效」后，质量块每晃过一次会配一声<b>滑音口哨</b>：去程音高上滑、回程下滑、换向时收音，像荡秋千时那种「咿↗——呜↘」的一声声，比单纯的风声更有音高、更好玩。声音由页面内的 <code>Web Audio</code> 实时合成，<b>没有任何外部音频文件</b>，单文件拷走、离线打开都能用。受浏览器自动播放策略限制，首次需要一次点击或按键才能激活，状态行会提示「点击页面任意处开启」。</p>
<p><b>口哨是怎么合成的。</b>真实口哨的频谱主体是一根近纯正弦、谐波很少；照这个结构搭三层，再叠一个颤音：</p>
<ul>
<li><b>主音</b>：正弦振荡器，频率即下文的音高；</li>
<li><b>哨芯</b>：二次谐波（2f）正弦，音量只有主音的 9~16%（去程略亮、回程略暗，强化来回的对比），否则听着像电子哔；</li>
<li><b>气声</b>：循环白噪声经窄带带通（中心 <code>2.1f + 700 Hz</code>、Q = 1.1）、音量为主音的 20%，给出吹气起音的「嘶」；</li>
<li><b>颤音</b>：5.4 Hz 的 LFO 直接调制主音频率 ±9 Hz（约 ±15 音分）。它接到频率参数上、由音频线程自己跑，不依赖动画帧率，所以听起来是"人吹的"而不是机械滑音。</li>
</ul>
<p><b>音高 = 质量块位置，音量 = 速度大小，声像 = 速度方向。</b>三个量各管一件事，都由同一个响应 <code>c(t)</code> 派生，音画不可能错拍：</p>
<p style="padding-left:14px"><code>f = f_lo + (f_hi − f_lo)·clamp(c(t)/1.6, 0, 1)　[Hz]</code></p>
<p>c 上升时 f 上滑、c 回落时 f 下滑，一个完整来回正好一组「上滑 + 下滑」；ζ 小、超调大时滑音跨度更宽（c 达到 1.6 即封顶 f_hi）。控制栏的「哨音」下拉给三档音区：<b>低沉 520~1200 Hz / 标准 760~1750 Hz / 清亮 980~2200 Hz</b>。它只改 f_lo、f_hi，滑音形状、节拍与自动静音门限都不变，方便按个人听感（或对照某段参考录音）挑一档。</p>
<p><b>节拍严格跟随速度的解析导数。</b>对欠阻尼 <code>c(t) = 1 − e^(−ζωₙt)/√(1−ζ²)·sin(ω_d t + β)</code> 求导，用 <code>β = arccos ζ</code>、<code>ω_d = ωₙ√(1−ζ²)</code> 化简后 β 恰好消掉：</p>
<p style="padding-left:14px"><code>ċ(t) = (ωₙ/√(1−ζ²)) · e^(−ζωₙt) · sin(ω_d t)</code></p>
<p>定义<b>带符号归一化速度</b> <code>ŝ = ċ / (ωₙ/√(1−ζ²)) = e^(−ζωₙt)·sin(ω_d t) ∈ [−1,1]</code>。音量取 <code>0.075·|ŝ|^1.25</code>、声像取 <code>0.30·ŝ</code>；系数取得比一般噪声层小得多，是因为纯音能量集中，同幅度下听起来响得多。ŝ 在 <code>ω_d t = 0、π、2π</code>（质量块到达两端、速度为零）归零，<b>一个完整来回恰好两声、两次换向各自静音</b>；绝对值在 <code>ω_d t = β + kπ</code> 取极大（由 <code>d|ŝ|/dt = 0 ⇒ tan(ω_d t) = ω_d/(ζωₙ)</code> 解得）。<b>注意速度峰不等同于「经过目标位置」，也不在半程正中</b>：ζ 越大越提前，ζ→0 才趋于 π/2；默认参数首峰 <code>t = β/ω_d = 1.159/1.833 ≈ 0.632 s</code>、<code>|ŝ| ≈ 0.553</code>。若某一帧采样跨过精确零点，代码额外补一帧静音谷，不让换向被跳过去。衰减因子 <code>e^(−ζωₙt)</code> 让哨声随质量块停稳自然收干，不必另写淡出。</p>
<p>状态行同步显示「去程 ↑ / 换向 · / 回程 ↓ / 已停稳」，质量块右侧还有同帧刷新的方向箭头（蓝色↑ / 橙色↓ / 灰色·），可直接对照检查音画是否对齐。ζ ≥ 1 时导数改用 <code>ωₙ²t·e^(−ωₙt)</code> 与 <code>ωₙ²(e^(−p₁t) − e^(−p₂t))/(p₂−p₁)</code>，但此时不回摆，音效直接判为静音。</p>
<p><b>太快或太慢时自动取消。</b>听感舒适与否取决于<b>观察者实际听到的摆动频率</b>，所以要乘上倍速：</p>
<p style="padding-left:14px"><code>f = ω_d/(2π) × 倍速 = ωₙ√(1−ζ²)/(2π) × 倍速　[Hz]</code></p>
<p>默认 ζ=0.4、ωₙ=2、1× 时 <code>f = 1.833/2π ≈ 0.292 Hz</code>（约 3.4 秒一个来回），正是荡秋千的节奏。门限取 <b>0.15 Hz ≤ f ≤ 3.0 Hz</b>：</p>
<ul>
<li><b>f &lt; 0.15 Hz（过慢）</b>：一个来回超过 6.7 秒，哨声被拉成拖沓的长音，听着不像「荡」→ 静音。例如 ζ=0.01、ωₙ=0.1、1× 时 f ≈ 0.016 Hz。此时提高倍速可把频率带回舒适区（100× 时 1.59 Hz），音效会自动恢复——倍速改变的只是观察速度，不影响任何指标数值。</li>
<li><b>f &gt; 3.0 Hz（过快）</b>：滑音连成急促的警报式上下滑，刺耳 → 静音。例如 ωₙ=10、20× 时 f ≈ 31.8 Hz。</li>
<li><b>ζ ≥ 1</b>：单调趋近、没有回摆，也就不该有「荡」的声音 → 静音。</li>
<li><b>暂停、拖动游标、切到后台</b>：无声。切换参数、倍速、误差带时门限实时重算。</li>
</ul>
<p>门限不是硬切：在 0.15~0.20 Hz 与 2.46~3.0 Hz 两段过渡带内增益线性渐弱，状态行显示「接近舒适边界 · 渐弱」，避免临界处声音忽有忽无。勾选框可随时彻底关闭；系统开启「减少动态效果」、或链接带 <code>?still=1</code> / <code>?snd=0</code> 时默认不启用音效。</p>
<p><b>别用耳朵读指标。</b>声音只是把「位置」与「速度」翻译成直觉提示，音高与音量都做了归一化和限幅，<b>不对应任何物理声压</b>；上升时间、超调量等仍以波形和卡片数值为准。它是可选的感知辅助，关掉不影响页面任何计算。</p>

<h2>比喻边界（讲课时必须点破）</h2>
<ol>
<li><b>数学同构 ≠ 物理相同。</b>实际被控对象可以是 RLC 电路、电机、热力学系统，并没有弹簧和滑块。</li>
<li><b>假设标准二阶、无零点。</b>真实高阶系统或含零点的系统，超调与上升时间会显著偏离上述公式，不能直接套用。</li>
<li><b>假设零初始条件 + 单位阶跃。</b>输入幅度可线性缩放，但非零初始条件会改变响应形状。</li>
<li><b>t<sub>s</sub> 依赖误差带选取。</b>5% 与 2% 结果不同；ζ 接近 1 或很小时，近似式与严格解偏差明显。特别注意 <code>3/(ζωₙ)</code> 这类近似式<b>只对欠阻尼成立</b>（来自包络 e^(−ζωₙt)）；ζ ≥ 1 时衰减由主导极点 <code>p₁ = ωₙ(ζ−√(ζ²−1))</code> 决定，硬套会严重低估——例如 ζ=2、ωₙ=10 时近似给 0.15 s，严格解为 1.146 s。因此本页在 ζ ≥ 1 时改显主导极点，不显示该近似式。</li>
<li><b>ζ 与 ωₙ 分工不同。</b>σ% 只由 ζ 决定，与 ωₙ 无关；ωₙ 只压缩或拉伸时间轴。自适应时间轴下拖 ωₙ 曲线形状几乎不变，这不是 bug，是时间尺度相似性——想直观看快慢，用<b>默认勾选的「固定 8 s」</b>；想看完整调节过程，取消勾选让时间轴自适应。注意固定窗下若 <code>t_s &gt; 8 s</code>（如 ζ=0.01、ωₙ=1 时约 299 s），曲线只显示响应前段，指标数值仍是全程严格解，不要误读为"系统不收敛"。</li>
<li><b>ζ 极小或 ωₙ 极小时衰减极慢</b>。本页 ωₙ 可低至 0.1 rad/s，配合 ζ=0.01 时 <code>t<sub>s</sub> ≈ 2996 s</code>；ζ=0.01、ωₙ=1 时约 299 s。波形密集属数学上正确，不是绘图错误。页面据此自适应加密采样避免混叠，并把 <code>t<sub>s</sub></code> 的数值搜索窗口按 <code>1/(ζωₙ)</code> 的时间尺度放大到万秒量级，避免把「收敛极慢」误判成「不收敛」。这类参数下固定 8 s 窗只显示极短一段响应，取消勾选「固定 8 s」才能看到完整调节过程。</li>
<li><b>播放速度默认 1:1 实时。</b>游标横坐标每前进 1 s，真实耗时就是 1 s：由 <code>requestAnimationFrame</code> 的时间戳直接对时间积分，而不是按帧数推进；切换标签页时单帧步长封顶 0.1 s，避免切回前台瞬间跳变。默认固定 8 s 窗一轮正好 8 s，拖 ωₙ 能真实感受到系统变快变慢，这正是固定窗的教学价值。取消勾选后自适应窗可能长达数千秒（ζ=0.01、ωₙ=0.1 时约 3882 s，约 65 分钟），本页<b>不会在未经选择时自动加速</b>；需要快速扫描可用「倍速」显式选 2× / 5× / 20× / 100×（100× 下 3882 s 约 39 秒播完）。状态行会标明当前倍率，长窗耗时也按倍率重算。<b>倍速只改变观察速度</b>，四项指标与解析式始终是按真实时间求得的严格解，不受影响；「复位」恢复 1×。欠阻尼且 <code>t<sub>s</sub> &gt; 8 s</code> 时底部提示变红，恰好对应「固定窗看不全」。</li>
</ol>

<h2>数值自检</h2>
<p>默认参数 ζ=0.4、ωₙ=2 rad/s、Δ=5%：<code>t_r = 1.081 s</code>、<code>t_p = 1.714 s</code>、<code>σ% = 25.38%</code>、<code>t_s = 3.805 s</code>（近似式 3.750 s）。可用 MATLAB <code>step()</code> 或 Python 独立复核。</p>
</div>
</div>
<script>
(function(){
var $=function(id){return document.getElementById(id);};
var Z=$('zR'),W=$('wR'),BAND=$('band'),AX=$('axmode'),SPEED=$('speed'),PLAY=$('play'),PLOT=$('plot'),
    SND=$('snd'),SNDTAG=$('sndTag'),RNGE=$('sndRange');
var st={cursor:0,playing:false,tmax:8,raf:null,last:null,p:null,m:null,speed:1,
        sndOn:true,sndGate:null,vn:0,sv:0,lastSv:null,phase:'',sndTarget:null,rangeIdx:1};
function resp(t,z,wn){
if(z<1-1e-9){var wd=wn*Math.sqrt(1-z*z),b=Math.acos(z);return 1-Math.exp(-z*wn*t)/Math.sqrt(1-z*z)*Math.sin(wd*t+b);}
if(z<=1+1e-9){return 1-(1+wn*t)*Math.exp(-wn*t);}
var r=Math.sqrt(z*z-1),p1=wn*(z-r),p2=wn*(z+r);
return 1-(p2*Math.exp(-p1*t)-p1*Math.exp(-p2*t))/(p2-p1);
}
function settle(z,wn,band){
if(z<1e-6)return null;
var T;
if(z<1){T=-Math.log(band*Math.sqrt(1-z*z))/(z*wn)*1.15;}
else{var r=Math.sqrt(Math.max(z*z-1,1e-9)),p1=Math.max(wn*(z-r),1e-6);T=-Math.log(band*0.25)/p1*1.4;}
if(!isFinite(T))return null;
T=Math.max(T,0.5);T=Math.min(T,50000);
var res=null,pass=0;
while(pass<3){
var N=Math.min(60000,Math.max(6000,Math.ceil(T/0.03))),dt=T/N,last=-1;
for(var i=0;i<=N;i++){if(Math.abs(resp(i*dt,z,wn)-1)>band)last=i;}
if(last<0){res=0;break;}
if(last<N-1){res=(last+1)*dt;break;}
T=Math.min(T*2,100000);pass++;
}
return res;
}
function metrics(z,wn,band){
var under=z<1-1e-9,tr=null,tp=null,sg,peak;
if(under){var wd=wn*Math.sqrt(1-z*z),b=Math.acos(z);tr=(Math.PI-b)/wd;tp=Math.PI/wd;sg=Math.exp(-Math.PI*z/Math.sqrt(1-z*z))*100;peak=1+sg/100;}
else{sg=0;peak=1;}
return {tr:tr,tp:tp,sg:sg,ts:settle(z,wn,band),peak:peak,under:under};
}
function X(t){return 46+t/st.tmax*374;}
function Y(c){return 240-c/2.1*220;}
function curvePath(p){var d='',N=Math.min(6000,Math.max(420,Math.ceil(st.tmax*p.wn/(2*Math.PI))*40));for(var i=0;i<=N;i++){var t=st.tmax*i/N;d+=(i?'L':'M')+X(t).toFixed(1)+' '+Y(resp(t,p.z,p.wn)).toFixed(1);}return d;}
function drawSpring(c){
var top=Y(c)-16,span=Math.max(top-8,2);
$('block').setAttribute('y',top);
var n=Math.max(3,Math.min(10,Math.floor(span/6)));
var d='M68 8',step=span/(n+1);
for(var i=1;i<=n;i++){d+='L'+(i%2?58:78)+' '+(8+step*i).toFixed(1);}
d+='L68 '+top;
$('spring').setAttribute('d',d);
var ch=Math.max(4,Math.min(32,span*0.30,span*0.8)),cy0=top-ch,py=cy0+ch*0.42;
$('cylBg').setAttribute('y',cy0);
$('cylBg').setAttribute('height',ch);
$('dash').setAttribute('y2',py);
$('pistonBar').setAttribute('y1',py);
$('pistonBar').setAttribute('y2',py);
$('cyl').setAttribute('d','M121 '+cy0+'V'+top+'H143V'+cy0);
$('jointS').setAttribute('cy',top);
$('jointD').setAttribute('cy',top);
$('cval').textContent='c(t) = '+c.toFixed(3);
/* 方向箭头与音效取同一帧的摆动阶段，音画对齐可直接对照 */
var ar=$('swingArrow'),ph=st.phase;
ar.setAttribute('y',top+21);
ar.textContent=ph.indexOf('去程')===0?'↑':(ph.indexOf('回程')===0?'↓':'·');
ar.style.fill=ph.indexOf('去程')===0?'#185FA5':(ph.indexOf('回程')===0?'#D85A30':'#B4B2A9');
}
/* ---------- 音效：吹口哨式滑音（Web Audio 实时合成，页面内零音频文件） ---------- */
var AC=null,a=null,SND_LO=0.15,SND_HI=3.0;
/* 三档音区（低沉 / 标准 / 清亮）：只改 f_lo、f_hi，滑音跨度等比例，节拍与门限不变 */
var RNG=[[520,1200],[760,1750],[980,2200]];
function freqAt(c){var r=RNG[st.rangeIdx],u=Math.max(0,Math.min(1,c/1.6));return r[0]+(r[1]-r[0])*u;}
/* ċ(t)：欠阻尼求导后 β 恰好消掉，得 wn·e^(−ζwn·t)·sin(wd·t)/√(1−ζ²) */
function respDot(t,z,wn){
if(z<1-1e-9){var wd=wn*Math.sqrt(1-z*z);return wn*Math.exp(-z*wn*t)*Math.sin(wd*t)/Math.sqrt(1-z*z);}
if(z<=1+1e-9){return wn*wn*t*Math.exp(-wn*t);}
var r=Math.sqrt(z*z-1),p1=wn*(z-r),p2=wn*(z+r);
return wn*wn*(Math.exp(-p1*t)-Math.exp(-p2*t))/(p2-p1);
}
/* 舒适门：听感取决于观察者实际听到的摆动频率，故乘倍速 */
function gateFor(p,sp){
if(!p)return {mode:'idle',w:0,f:0,msg:'等待参数'};
if(p.z>=1-1e-9)return {mode:'norock',w:0,f:0,msg:'ζ ≥ 1 无回摆'};
var wd=p.wn*Math.sqrt(1-p.z*p.z),f=wd/(2*Math.PI)*sp;
if(f<SND_LO)return {mode:'slow',w:0,f:f,msg:'摆动 '+f.toFixed(2)+' Hz 过慢'};
if(f>SND_HI)return {mode:'fast',w:0,f:f,msg:'摆动 '+f.toFixed(1)+' Hz 过快'};
var wa=Math.min(1,(f-SND_LO)/(SND_LO*0.35));
var wb=Math.min(1,(SND_HI-f)/(SND_HI*0.18));
var w=Math.max(0,Math.min(wa,wb));
return {mode:w<0.999?'edge':'ok',w:w,f:f,
        msg:w<0.999?'摆动 '+f.toFixed(2)+' Hz 接近舒适边界 · 渐弱':'摆动 '+f.toFixed(2)+' Hz'};
}
function audioGate(){return gateFor(st.p,st.speed);}
function buildAudio(){
var Ctx=window.AudioContext||window.webkitAudioContext;
if(!Ctx)return false;
try{AC=new Ctx();}catch(e){AC=null;return false;}
/* 口哨的频谱主体是近纯正弦；加一层很轻的二次谐波给"哨芯"，否则听着像电子哔 */
var o1=AC.createOscillator();o1.type='sine';o1.frequency.value=freqAt(0);
var g1=AC.createGain();g1.gain.value=0;
var o2=AC.createOscillator();o2.type='sine';o2.frequency.value=2*freqAt(0);
var g2=AC.createGain();g2.gain.value=0;
/* 气声：窄带噪声跟随音高，模拟吹气的"嘶"，起音时最明显 */
var sr=AC.sampleRate,len=Math.floor(sr*2);
var buf=AC.createBuffer(1,len,sr),ch=buf.getChannelData(0);
for(var i=0;i<len;i++)ch[i]=Math.random()*2-1;
var src=AC.createBufferSource();src.buffer=buf;src.loop=true;
var bp=AC.createBiquadFilter();bp.type='bandpass';bp.frequency.value=2600;bp.Q.value=1.1;
var gb=AC.createGain();gb.gain.value=0;
/* 颤音用 LFO 直接调制频率参数，不依赖帧率；AudioParam 会把它与每帧设定值相加 */
var lfo=AC.createOscillator();lfo.type='sine';lfo.frequency.value=5.4;
var lfoG=AC.createGain();lfoG.gain.value=9;
var pan=AC.createStereoPanner?AC.createStereoPanner():null;
if(pan)pan.pan.value=0;
var master=AC.createGain(),out=pan||master;master.gain.value=0.9;
o1.connect(g1);g1.connect(out);
o2.connect(g2);g2.connect(out);
src.connect(bp);bp.connect(gb);gb.connect(out);
if(pan)pan.connect(master);
master.connect(AC.destination);
lfo.connect(lfoG);lfoG.connect(o1.frequency);
o1.start(0);o2.start(0);src.start(0);lfo.start(0);
a={o1:o1,o2:o2,g1:g1,g2:g2,src:src,bp:bp,gb:gb,lfo:lfo,lfoG:lfoG,pan:pan,master:master};
return true;
}
function ensureAudio(){
if(!st.sndOn)return;
if(!AC&&!buildAudio()){paintSnd('当前浏览器不支持 Web Audio');return;}
if(AC.state==='suspended'&&AC.resume){try{AC.resume().then(function(){paintSnd();},function(){});}catch(e){}}
st.sndGate=audioGate();paintSnd();
}
function fadeAudio(){
if(a&&AC.state==='running'){var n=AC.currentTime;
a.g1.gain.setTargetAtTime(0,n,0.03);a.g2.gain.setTargetAtTime(0,n,0.03);a.gb.gain.setTargetAtTime(0,n,0.03);}
}
function setTag(txt,col){
if(SNDTAG.textContent!==txt)SNDTAG.textContent=txt;
if(SNDTAG.style.color!==col)SNDTAG.style.color=col;
}
function paintSnd(over){
var g=st.sndGate||{mode:'idle',msg:'等待参数'};
if(over){setTag('音效不可用 · '+over,'#993C1D');return;}
if(!st.sndOn){setTag('音效 关','');return;}
if(!AC||AC.state!=='running'){setTag('音效待激活 · 点击页面任意处开启','#993C1D');return;}
if(!st.playing||dragging){setTag('音效暂停（'+g.msg+'）','');return;}
var on=(g.mode==='ok'||g.mode==='edge');
setTag((on?'音效 开 · ':'音效 自动静音 · ')+g.msg+(st.phase?' · '+st.phase:''),on?'':'#993C1D');
}
function updateAudio(){
var g=audioGate();st.sndGate=g;
var p=st.p,sv=0,vn=0;
if(p){
var v=respDot(st.cursor,p.z,p.wn);
var vmax=(p.z<1-1e-9)?p.wn/Math.sqrt(1-p.z*p.z):p.wn;
/* 带符号归一化速度：欠阻尼下精确等于 e^(−ζωₙt)·sin(ω_d t)，∈[−1,1] */
sv=Math.max(-1,Math.min(1,v/Math.max(vmax,1e-6)));
vn=Math.abs(sv);
}
st.sv=sv;st.vn=vn;
/* 摆动阶段严格由速度符号给出：sv>0 去程（c 增大、块上行），sv<0 回程，sv≈0 换向 */
if(!p||p.z>=1-1e-9)st.phase='无回摆';
else if(Math.exp(-p.z*p.wn*st.cursor)<0.012)st.phase='已停稳';
else if(vn<0.035)st.phase='换向 ·';
else st.phase=sv>0?'去程 ↑':'回程 ↓';
var live=!!(AC&&a&&st.sndOn&&st.playing&&!dragging&&!document.hidden&&AC.state==='running');
/* 帧采样可能跨过精确的速度零点，显式补一个静音谷，保证每个换向点都真的听得出来 */
var crossed=(st.lastSv!==null&&st.lastSv*sv<0);
var amp=(live&&!crossed)?g.w*Math.pow(vn,1.25)*0.075:0;
st.lastSv=sv;
/* 音高 = 质量块位置：c 上升即上滑、c 回落即下滑，一个来回正好一组「咿↗—呜↘」 */
var c=st.p?resp(st.cursor,st.p.z,st.p.wn):1;
var u=Math.max(0,Math.min(1,c/1.6)),freq=freqAt(c);
var pval=live?0.30*sv:0;
st.sndTarget={amp:amp,freq:freq,c:c,u:u,pan:pval,crossed:crossed,live:live,w:g.w};
if(a&&AC.state==='running'){
var n=AC.currentTime;
a.g1.gain.setTargetAtTime(amp,n,0.016);
/* 去程谐波略亮、回程略暗，与音高上滑/下沉同相，来回的对比更明显 */
a.g2.gain.setTargetAtTime(amp*(sv>0?0.16:0.09),n,0.016);
a.gb.gain.setTargetAtTime(amp*0.20,n,0.016);
a.o1.frequency.setTargetAtTime(freq,n,0.012);
a.o2.frequency.setTargetAtTime(freq*2,n,0.012);
a.bp.frequency.setTargetAtTime(Math.min(7000,2.1*freq+700),n,0.03);
if(a.pan)a.pan.pan.setTargetAtTime(pval,n,0.05);
}
paintSnd();
}
function drawCursor(){
var p=st.p,c=resp(st.cursor,p.z,p.wn),x=X(st.cursor);
$('cursorLine').setAttribute('x1',x);$('cursorLine').setAttribute('x2',x);
$('cursorDot').setAttribute('cx',x);$('cursorDot').setAttribute('cy',Y(c));
updateAudio();
drawSpring(c);
}
function wallDur(s){
if(s<60)return s.toFixed(0)+' 秒';
if(s<7200)return (s/60).toFixed(0)+' 分钟';
return (s/3600).toFixed(1)+' 小时';
}
function renderRegime(){
var p=st.p,m=st.m,sp=st.speed;
var rg=p.z<1e-6?'无阻尼 ζ = 0':(p.z<1?'欠阻尼 0 < ζ < 1':(Math.abs(p.z-1)<0.005?'临界阻尼 ζ = 1':'过阻尼 ζ > 1'));
if(st.tmax>60)rg+=' · 时间窗 '+st.tmax.toFixed(0)+' s，'+(sp===1?'1:1 实时':'×'+sp+' 倍速')+'播放整窗约 '+wallDur(st.tmax/sp)+'，建议拖动游标定位';
else if(m.ts!==null&&m.ts>st.tmax)rg+=' · 固定窗 '+st.tmax.toFixed(0)+' s < t_s，仅显示响应前段，取消勾选可看全程';
$('regime').textContent=rg;
$('regime').style.color=(m.under&&m.ts!==null&&m.ts>8)?'#A32D2D':'';
$('speedTag').textContent=sp===1?'1× 实时':'×'+sp+' 倍速';
}
function rebuild(){
st.lastSv=null;
var p={z:parseFloat(Z.value),wn:parseFloat(W.value),band:parseFloat(BAND.value),fixed:AX.checked};
var m=metrics(p.z,p.wn,p.band);
st.p=p;st.m=m;
st.tmax=p.fixed?8:(m.ts!==null?Math.max(m.ts*1.3,m.tp?m.tp*1.25:0,0.8):6*Math.PI/p.wn);
st.cursor=0;
$('curve').setAttribute('d',curvePath(p));
$('bandRect').setAttribute('y',Y(1+p.band));
$('bandRect').setAttribute('height',Y(1-p.band)-Y(1+p.band));
for(var i=0;i<5;i++){var e=$('xk'+i);e.setAttribute('x',46+374*i/4);e.textContent=(st.tmax*i/4).toFixed(1);}
var tr=$('trDot'),tp=$('tpDot'),tpL=$('tpLab'),tsL=$('tsLine'),tsT=$('tsLab');
if(m.tr!==null&&m.tr<=st.tmax){tr.setAttribute('cx',X(m.tr));tr.setAttribute('cy',Y(1));tr.style.display='';}else{tr.style.display='none';}
if(m.tp!==null&&m.tp<=st.tmax){tp.setAttribute('cx',X(m.tp));tp.setAttribute('cy',Y(m.peak));tp.style.display='';tpL.setAttribute('x',X(m.tp)+7);tpL.setAttribute('y',Y(m.peak)-7);tpL.textContent='σ '+m.sg.toFixed(1)+'%';tpL.style.display='';}else{tp.style.display='none';tpL.style.display='none';}
if(m.ts!==null&&m.ts<=st.tmax){tsL.setAttribute('x1',X(m.ts));tsL.setAttribute('x2',X(m.ts));tsL.style.display='';tsT.setAttribute('x',X(m.ts)+4);tsT.style.display='';}else{tsL.style.display='none';tsT.style.display='none';}
renderRegime();
$('vZeta').textContent=p.z.toFixed(2);
$('vWn').textContent=p.wn.toFixed(1);
if(p.z<1-1e-9){
var wdf=p.wn*Math.sqrt(1-p.z*p.z),bf=Math.acos(p.z),kf=1/Math.sqrt(1-p.z*p.z);
$('fTag').textContent='欠阻尼 0<ζ<1';
$('fSym').textContent='c(t) = 1 − e^(−ζωₙt)/√(1−ζ²) · sin(ω_d t + β)';
$('fNum').textContent='c(t) = 1 − '+kf.toFixed(3)+'·e^(−'+(p.z*p.wn).toFixed(3)+'t)·sin('+wdf.toFixed(3)+'t + '+bf.toFixed(3)+')';
}else if(Math.abs(p.z-1)<0.005){
$('fTag').textContent='临界阻尼 ζ=1';
$('fSym').textContent='c(t) = 1 − (1 + ωₙt)·e^(−ωₙt)';
$('fNum').textContent='c(t) = 1 − (1 + '+p.wn.toFixed(3)+'t)·e^(−'+p.wn.toFixed(3)+'t)';
}else{
var rf=Math.sqrt(p.z*p.z-1),p1f=p.wn*(p.z-rf),p2f=p.wn*(p.z+rf),Af=p2f/(p2f-p1f),Bf=p1f/(p2f-p1f);
$('fTag').textContent='过阻尼 ζ>1';
$('fSym').textContent='c(t) = 1 − [p₂e^(−p₁t) − p₁e^(−p₂t)]/(p₂ − p₁)';
$('fNum').textContent='c(t) = 1 − '+Af.toFixed(3)+'·e^(−'+p1f.toFixed(3)+'t) + '+Bf.toFixed(3)+'·e^(−'+p2f.toFixed(3)+'t)';
}
$('vTr').textContent=m.tr!==null?m.tr.toFixed(3)+' s':'—';
$('sTr').textContent=m.tr!==null?'(π−β)/ω_d':'渐近，不穿越 1';
$('vTp').textContent=m.tp!==null?m.tp.toFixed(3)+' s':'—';
$('sTp').textContent=m.tp!==null?'π/ω_d':'单调，无峰值';
$('vSg').textContent=m.sg.toFixed(2)+' %';
$('sSg').textContent=m.under?'e^(−πζ/√(1−ζ²))':'ζ ≥ 1，无超调';
$('vTs').textContent=m.ts!==null?m.ts.toFixed(3)+' s':'—';
var k=p.band===0.05?3:4;
if(m.ts===null){$('sTs').textContent='超出数值搜索窗口';}
else if(m.under){$('sTs').textContent='近似 '+k+'/(ζωₙ) = '+(k/(p.z*p.wn)).toFixed(2)+' s';}
else{var p1=p.wn*(p.z-Math.sqrt(p.z*p.z-1));$('sTs').textContent='主导极点 p₁ = '+p1.toFixed(2)+' /s';}
drawCursor();
}
function loop(t){
if(!st.playing){st.raf=null;return;}
if(st.last===null)st.last=t;
var dt=Math.min((t-st.last)/1000,0.1);
st.last=t;
st.cursor+=dt*st.speed;
if(st.cursor>st.tmax)st.cursor=st.cursor%st.tmax;
drawCursor();
st.raf=requestAnimationFrame(loop);
}
function start(){if(st.raf===null){st.last=null;st.raf=requestAnimationFrame(loop);}}
var dragging=false;
function seek(e){var r=PLOT.getBoundingClientRect();var vx=(e.clientX-r.left)/r.width*440;var t=(vx-46)/374*st.tmax;st.cursor=Math.max(0,Math.min(st.tmax,t));drawCursor();}
PLOT.addEventListener('pointerdown',function(e){dragging=true;st.playing=false;PLAY.textContent='播放';if(PLOT.setPointerCapture)PLOT.setPointerCapture(e.pointerId);seek(e);});
PLOT.addEventListener('pointermove',function(e){if(dragging)seek(e);});
PLOT.addEventListener('pointerup',function(){dragging=false;});
Z.addEventListener('input',rebuild);
W.addEventListener('input',rebuild);
BAND.addEventListener('change',rebuild);
AX.addEventListener('change',rebuild);
SPEED.addEventListener('change',function(){st.speed=parseFloat(SPEED.value);renderRegime();updateAudio();});
SND.addEventListener('change',function(){
st.sndOn=SND.checked;
if(st.sndOn)ensureAudio();else fadeAudio();
st.sndGate=audioGate();paintSnd();
});
function setRangeIdx(v){var i=parseInt(v,10);st.rangeIdx=isNaN(i)?1:Math.max(0,Math.min(RNG.length-1,i));RNGE.value=String(st.rangeIdx);}
RNGE.addEventListener('change',function(){setRangeIdx(RNGE.value);updateAudio();});
PLAY.addEventListener('click',function(){st.playing=!st.playing;PLAY.textContent=st.playing?'暂停':'播放';if(st.playing)start();updateAudio();});
$('reset').addEventListener('click',function(){Z.value='0.4';W.value='2';BAND.value='0.05';AX.checked=true;SPEED.value='1';st.speed=1;rebuild();st.playing=true;PLAY.textContent='暂停';start();updateAudio();});
/* 浏览器自动播放策略：首次手势才创建 / 恢复 AudioContext；常驻监听也让后台被挂起的上下文能回来 */
function kickAudio(){if(st.sndOn&&(!AC||AC.state==='suspended'))ensureAudio();}
document.addEventListener('pointerdown',kickAudio,{passive:true});
document.addEventListener('keydown',kickAudio,{passive:true});
document.addEventListener('visibilitychange',function(){updateAudio();});
var qs=new URLSearchParams(location.search);
if(qs.has('z'))Z.value=qs.get('z');
if(qs.has('w'))W.value=qs.get('w');
if(qs.has('band'))BAND.value=qs.get('band');
if(qs.has('fixed'))AX.checked=qs.get('fixed')==='1';
if(qs.has('speed'))SPEED.value=qs.get('speed');
if(qs.has('snd'))SND.checked=qs.get('snd')==='1';
if(qs.has('range'))setRangeIdx(qs.get('range'));else setRangeIdx(RNGE.value);
st.speed=parseFloat(SPEED.value);
st.sndOn=SND.checked;
rebuild();
if(qs.has('t')){st.cursor=parseFloat(qs.get('t'));drawCursor();}
var rm=window.matchMedia&&matchMedia('(prefers-reduced-motion: reduce)').matches;
if(qs.get('still')==='1'||rm){st.playing=false;PLAY.textContent='播放';}
else{st.playing=true;start();}
if(rm||qs.get('still')==='1'||qs.get('snd')==='0'){SND.checked=false;st.sndOn=false;}
updateAudio();
/* 诊断接口：供自动核对门限与解析导数，不影响页面行为 */
window.__SND__={
state:function(){return {on:st.sndOn,ctx:AC?AC.state:'none',gate:st.sndGate,vn:st.vn,sv:st.sv,phase:st.phase,
  playing:st.playing,speed:st.speed,cursor:st.cursor,
  p:st.p?{z:st.p.z,wn:st.p.wn}:null,gain:a?a.g1.gain.value:null,
  freq:st.sndTarget?st.sndTarget.freq:null};},
gate:function(z,wn,sp){return gateFor({z:z,wn:wn},sp||1);},
dot:function(t,z,wn){return respDot(t,z,wn);},
resp:function(t,z,wn){return resp(t,z,wn);},
pitch:function(c){return freqAt(c);},
range:function(){var r=RNG[st.rangeIdx];return {lo:r[0],hi:r[1],idx:st.rangeIdx,all:RNG,gateLo:SND_LO,gateHi:SND_HI};},
setRange:function(i){setRangeIdx(i);updateAudio();return this.range();},
nodes:function(){return a?{o1:a.o1.frequency.value,o2:a.o2.frequency.value,
  g1:a.g1.gain.value,g2:a.g2.gain.value,gb:a.gb.gain.value,
  bp:a.bp.frequency.value,lfo:a.lfo.frequency.value}:null;},
limits:function(){return {lo:SND_LO,hi:SND_HI};},
target:function(){return st.sndTarget||null;},
step:function(t){st.cursor=t;drawCursor();return {sv:st.sv,vn:st.vn,phase:st.phase,target:st.sndTarget,arrow:$('swingArrow').textContent};},
set:function(z,wn,sp,t){Z.value=z;W.value=wn;SPEED.value=sp;st.speed=parseFloat(SPEED.value);rebuild();if(t!=null){st.cursor=t;drawCursor();}return this.state();}
};
})();
</script>
</body>
</html>
