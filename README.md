<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ernest B. — Developer & Game Designer</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=JetBrains+Mono:wght@300;400;500&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg:          #07090F;
      --bg-surface:  #0D1019;
      --bg-raised:   #131724;
      --accent:      #7B6EF0;
      --accent-dim:  rgba(123,110,240,0.14);
      --accent2:     #2AD4F5;
      --text:        #E3E6F2;
      --muted:       #6B738A;
      --dim:         #3A4055;
      --border:      rgba(123,110,240,0.13);
      --border-sub:  rgba(255,255,255,0.04);
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Space Grotesk', system-ui, sans-serif;
      line-height: 1.6;
      overflow-x: hidden;
      min-height: 100vh;
    }

    /* Dot-grid overlay */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        radial-gradient(circle, rgba(123,110,240,0.07) 1px, transparent 1px);
      background-size: 36px 36px;
      z-index: 0;
      pointer-events: none;
    }

    #canvas {
      position: fixed;
      inset: 0;
      z-index: 0;
      pointer-events: none;
    }

    .wrap {
      position: relative;
      z-index: 1;
      max-width: 860px;
      margin: 0 auto;
      padding: 0 2rem;
    }

    /* ---- HERO ---- */
    .hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      padding: 6rem 0 4rem;
      position: relative;
    }

    .hero-inner {
      opacity: 0;
      transform: translateY(32px);
      animation: fadeUp 0.95s cubic-bezier(0.16,1,0.3,1) forwards 0.1s;
    }

    .pre {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.88rem;
      color: var(--accent2);
      letter-spacing: 0.06em;
      margin-bottom: 0.6rem;
      display: flex;
      align-items: center;
      gap: 0.45rem;
    }
    .pre-arrow { color: var(--accent); }

    .name {
      font-size: clamp(4.2rem, 13vw, 8.5rem);
      font-weight: 700;
      line-height: 0.9;
      letter-spacing: -0.03em;
      margin-bottom: 1.4rem;
      background: linear-gradient(130deg, #fff 15%, var(--accent) 52%, var(--accent2) 88%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .role-line {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      height: 1.9rem;
      margin-bottom: 1.6rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.9rem;
    }
    .role-prefix { color: var(--dim); }
    .role-text   { color: var(--accent); font-weight: 500; }
    .cursor {
      display: inline-block;
      width: 2px;
      height: 1em;
      background: var(--accent2);
      vertical-align: text-bottom;
      animation: blink 1s step-end infinite;
    }

    .status {
      display: inline-flex;
      align-items: center;
      gap: 0.45rem;
      background: var(--bg-raised);
      border: 1px solid var(--border);
      border-radius: 100px;
      padding: 0.28rem 0.85rem;
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      margin-bottom: 2rem;
      opacity: 0;
      animation: fadeUp 0.95s cubic-bezier(0.16,1,0.3,1) forwards 0.35s;
    }
    .dot {
      width: 6px; height: 6px;
      border-radius: 50%;
      background: #4ade80;
      box-shadow: 0 0 7px rgba(74,222,128,0.55);
      animation: pulse 2.2s ease-in-out infinite;
    }

    .bio {
      max-width: 580px;
      font-size: 1.05rem;
      color: var(--muted);
      line-height: 1.82;
      opacity: 0;
      transform: translateY(20px);
      animation: fadeUp 0.95s cubic-bezier(0.16,1,0.3,1) forwards 0.55s;
    }
    .bio strong { color: var(--text); font-weight: 600; }

    /* Scroll hint */
    .scroll-hint {
      position: absolute;
      bottom: 2.5rem; left: 50%;
      transform: translateX(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.4rem;
      opacity: 0;
      animation: fadeUp 1s ease forwards 1.4s;
    }
    .scroll-hint span {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.6rem;
      color: var(--dim);
      letter-spacing: 0.14em;
      text-transform: uppercase;
    }
    .scroll-bar {
      width: 1px;
      height: 44px;
      background: linear-gradient(180deg, var(--accent), transparent);
      animation: drip 2s ease-in-out infinite;
    }

    /* ---- DIVIDER ---- */
    .div { height:1px; background: linear-gradient(90deg,transparent,var(--border),transparent); }

    /* ---- SECTIONS ---- */
    section { padding: 5rem 0; }

    .sec-label {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin-bottom: 2.4rem;
      opacity: 0;
      transform: translateY(14px);
      transition: opacity 0.6s ease, transform 0.6s ease;
    }
    .sec-label.on { opacity:1; transform:none; }
    .sec-label-txt {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--muted);
      white-space: nowrap;
    }
    .sec-line {
      flex:1; height:1px;
      background: linear-gradient(90deg, var(--border), transparent);
    }

    /* ---- BADGES ---- */
    .badges {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
      opacity: 0;
      transform: translateY(14px);
      transition: opacity 0.65s ease 0.08s, transform 0.65s ease 0.08s;
    }
    .badges.on { opacity:1; transform:none; }
    .b {
      border-radius: 6px;
      overflow: hidden;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      cursor: default;
    }
    .b:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow: 0 6px 22px rgba(123,110,240,0.22);
    }
    .b img { display:block; height:28px; width:auto; }

    /* ---- STATS ---- */
    .stats {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.2rem;
      opacity: 0;
      transform: translateY(14px);
      transition: opacity 0.65s ease 0.08s, transform 0.65s ease 0.08s;
    }
    .stats.on { opacity:1; transform:none; }
    .stat-card {
      background: var(--bg-surface);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 0.7rem;
      transition: border-color 0.25s, box-shadow 0.25s;
      overflow: hidden;
    }
    .stat-card:hover {
      border-color: rgba(123,110,240,0.32);
      box-shadow: 0 0 28px rgba(123,110,240,0.1);
    }
    .stat-card img { width:100%; display:block; border-radius:8px; }

    /* ---- FOOTER ---- */
    footer {
      padding: 2.5rem 0 4rem;
      text-align: center;
      border-top: 1px solid var(--border-sub);
      opacity: 0;
      transform: translateY(14px);
      transition: opacity 0.6s ease, transform 0.6s ease;
    }
    footer.on { opacity:1; transform:none; }
    footer p {
      font-family: 'JetBrains Mono', monospace;
      font-size: 0.72rem;
      color: var(--dim);
      letter-spacing: 0.04em;
    }
    footer p span { color: var(--accent); }

    /* ---- ANIMATIONS ---- */
    @keyframes fadeUp { to { opacity:1; transform:translateY(0); } }
    @keyframes blink  { 50% { opacity:0; } }
    @keyframes pulse  { 0%,100% { opacity:1; } 50% { opacity:0.35; } }
    @keyframes drip {
      0%   { transform: scaleY(0); transform-origin: top; }
      49%  { transform: scaleY(1); transform-origin: top; }
      50%  { transform: scaleY(1); transform-origin: bottom; }
      100% { transform: scaleY(0); transform-origin: bottom; }
    }

    /* ---- RESPONSIVE ---- */
    @media (max-width: 600px) {
      .wrap  { padding: 0 1.2rem; }
      .stats { grid-template-columns: 1fr; }
    }
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration:.01ms !important; transition-duration:.01ms !important; }
    }
  </style>
</head>
<body>

<canvas id="canvas"></canvas>

<div class="wrap">

  <!-- HERO -->
  <section class="hero">
    <div class="hero-inner">
      <p class="pre"><span class="pre-arrow">▸</span> Hi, I'm</p>
      <h1 class="name">Ernest.</h1>
      <div class="role-line">
        <span class="role-prefix">// </span>
        <span class="role-text" id="role"></span>
        <span class="cursor"></span>
      </div>
      <div class="status">
        <span class="dot"></span>
        Dual Enrolled &middot; HS &amp; College &middot; IT
      </div>
      <p class="bio">
        Software engineering student focused on developing <strong>Apps and games</strong>
        to make better systems for all. Currently Dual Enrolled in HS and College for IT.
        Hoping to be a <strong>Software Engineer / Game Developer</strong> very soon!
      </p>
    </div>
    <div class="scroll-hint">
      <span>scroll</span>
      <div class="scroll-bar"></div>
    </div>
  </section>

  <div class="div"></div>

  <!-- SKILLS -->
  <section>
    <div class="sec-label">
      <span class="sec-label-txt">Coding Languages &amp; Tools</span>
      <div class="sec-line"></div>
    </div>
    <div class="badges">
      <div class="b"><img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"></div>
      <div class="b"><img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript"></div>
      <div class="b"><img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"></div>
      <div class="b"><img src="https://img.shields.io/badge/LuaU-00A2FF?style=for-the-badge&logo=lua&logoColor=white" alt="LuaU"></div>
      <div class="b"><img src="https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white" alt="C#"></div>
      <div class="b"><img src="https://img.shields.io/badge/Swift-F05138?style=for-the-badge&logo=swift&logoColor=white" alt="Swift"></div>
      <div class="b"><img src="https://img.shields.io/badge/Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white" alt="Kotlin"></div>
      <div class="b"><img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5"></div>
      <div class="b"><img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3"></div>
      <div class="b"><img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React"></div>
      <div class="b"><img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js"></div>
      <div class="b"><img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"></div>
      <div class="b"><img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" alt="Git"></div>
      <div class="b"><img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"></div>
      <div class="b"><img src="https://img.shields.io/badge/Visual_Studio-5C2D91?style=for-the-badge&logo=visual-studio&logoColor=white" alt="Visual Studio"></div>
      <div class="b"><img src="https://img.shields.io/badge/VS_Code-007ACC?style=for-the-badge&logo=visual-studio-code&logoColor=white" alt="VS Code"></div>
      <div class="b"><img src="https://img.shields.io/badge/Android_Studio-3DDC84?style=for-the-badge&logo=android-studio&logoColor=white" alt="Android Studio"></div>
      <div class="b"><img src="https://img.shields.io/badge/Xcode-147EFB?style=for-the-badge&logo=xcode&logoColor=white" alt="Xcode"></div>
      <div class="b"><img src="https://img.shields.io/badge/Godot-478CBF?style=for-the-badge&logo=godot-engine&logoColor=white" alt="Godot"></div>
      <div class="b"><img src="https://img.shields.io/badge/Roblox_Studio-00A2FF?style=for-the-badge&logo=roblox&logoColor=white" alt="Roblox Studio"></div>
    </div>
  </section>

  <div class="div"></div>

  <!-- STATS -->
  <section>
    <div class="sec-label">
      <span class="sec-label-txt">GitHub Stats</span>
      <div class="sec-line"></div>
    </div>
    <div class="stats">
      <div class="stat-card">
        <img src="https://github-readme-stats-eight-theta.vercel.app/api?username=Ernestb160&show_icons=true&hide_border=true&bg_color=0D1019&title_color=7B6EF0&text_color=E3E6F2&icon_color=2AD4F5" alt="Ernest's GitHub Stats">
      </div>
      <div class="stat-card">
        <img src="https://github-readme-stats-eight-theta.vercel.app/api/top-langs/?username=ernestb160&layout=compact&hide_border=true&bg_color=0D1019&title_color=7B6EF0&text_color=E3E6F2&icon_color=2AD4F5" alt="Top Languages">
      </div>
    </div>
  </section>

  <footer id="foot">
    <p>Ernest B. &nbsp;&middot;&nbsp; <span>Software Engineer / Game Developer in the Making</span></p>
  </footer>

</div>

<script>
/* ── Particle canvas ── */
!function(){
  const cv = document.getElementById('canvas');
  const cx = cv.getContext('2d');
  let W, H, pts = [];

  function resize(){
    W = cv.width  = window.innerWidth;
    H = cv.height = window.innerHeight;
  }
  window.addEventListener('resize', resize);
  resize();

  const C = ['123,110,240','42,212,245'];

  function Dot(init){
    this.spawn = (init) => {
      this.x  = Math.random() * W;
      this.y  = init ? Math.random() * H : H + 8;
      this.r  = Math.random() * 1.1 + 0.3;
      this.vy = -(Math.random() * 0.22 + 0.07);
      this.vx = (Math.random() - 0.5) * 0.07;
      this.a  = Math.random() * 0.28 + 0.06;
      this.c  = C[Math.random() > 0.5 ? 0 : 1];
    };
    this.spawn(init);
  }
  Dot.prototype.tick = function(){
    this.x += this.vx;
    this.y += this.vy;
    if(this.y < -8) this.spawn(false);
  };
  Dot.prototype.draw = function(){
    cx.save();
    cx.globalAlpha = this.a;
    cx.fillStyle = `rgba(${this.c},1)`;
    cx.beginPath();
    cx.arc(this.x, this.y, this.r, 0, Math.PI*2);
    cx.fill();
    cx.restore();
  };

  for(let i=0;i<72;i++) pts.push(new Dot(true));

  (function loop(){
    cx.clearRect(0,0,W,H);
    pts.forEach(p=>{p.tick();p.draw();});
    requestAnimationFrame(loop);
  })();
}();

/* ── Typewriter ── */
!function(){
  const el = document.getElementById('role');
  const phrases = [
    'App & Game Developer',
    'Software Engineering Student',
    'Building Better Systems'
  ];
  let pi=0, ci=0, del=false;

  function tick(){
    const w = phrases[pi];
    if(!del){
      el.textContent = w.slice(0, ++ci);
      if(ci === w.length){ setTimeout(()=>{del=true;tick();}, 1900); return; }
      setTimeout(tick, 62);
    } else {
      el.textContent = w.slice(0, --ci);
      if(ci === 0){ del=false; pi=(pi+1)%phrases.length; }
      setTimeout(tick, 32);
    }
  }
  setTimeout(tick, 1000);
}();

/* ── Scroll reveal ── */
!function(){
  const els = document.querySelectorAll('.sec-label,.badges,.stats,footer');
  const io = new IntersectionObserver(entries=>{
    entries.forEach(e=>{ if(e.isIntersecting){ e.target.classList.add('on'); io.unobserve(e.target); } });
  }, { threshold: 0.1 });
  els.forEach(el=>io.observe(el));
}();
</script>

</body>
</html>
