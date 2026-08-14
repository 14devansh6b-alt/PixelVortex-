<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PixelVortex — VFX / 3D / Design / Edit</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Share+Tech+Mono&family=Rajdhani:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0a0c0e;
    --panel:#13161a;
    --panel-2:#191d22;
    --line:#2a2c2f;
    --text:#eceded;
    --text-dim:#8f9296;
    --teal:#c7cacd;
    --orange:#5c6064;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    background:var(--bg);
    color:var(--text);
    font-family:'Rajdhani',sans-serif;
    font-weight:500;
    line-height:1.5;
    overflow-x:hidden;
  }
  ::selection{background:var(--teal);color:#000;}
  a{color:inherit;text-decoration:none;}

  .mono{font-family:'Share Tech Mono',monospace;}
  h1,h2,h3,.display{
    font-family:'Orbitron',sans-serif;
    letter-spacing:0.02em;
    line-height:1.08;
    font-weight:700;
  }

  .wrap{max-width:1180px;margin:0 auto;padding:0 28px;}

  /* ---------- corner-bracket "viewfinder" frame, the signature element ---------- */
  .viewfinder{position:relative;}
  .viewfinder::before,.viewfinder::after,
  .viewfinder .vf-tl,.viewfinder .vf-tr,.viewfinder .vf-bl,.viewfinder .vf-br{
    content:"";position:absolute;width:22px;height:22px;
    border-color:var(--teal);border-style:solid;border-width:0;
    opacity:0.85;transition:all .25s ease;
  }
  .viewfinder::before{top:-1px;left:-1px;border-top-width:2px;border-left-width:2px;}
  .viewfinder::after{top:-1px;right:-1px;border-top-width:2px;border-right-width:2px;}
  .viewfinder .vf-bl{bottom:-1px;left:-1px;border-bottom-width:2px;border-left-width:2px;}
  .viewfinder .vf-br{bottom:-1px;right:-1px;border-bottom-width:2px;border-right-width:2px;}
  .viewfinder:hover::before,.viewfinder:hover::after,
  .viewfinder:hover .vf-bl,.viewfinder:hover .vf-br{width:30px;height:30px;border-color:var(--orange);}

  /* ---------- top bar ---------- */
  header{
    position:fixed;top:0;left:0;right:0;z-index:100;
    background:rgba(10,12,14,0.85);backdrop-filter:blur(10px);
    border-bottom:1px solid var(--line);
  }
  .nav{display:flex;align-items:center;justify-content:space-between;height:64px;}
  .brand{font-family:'Orbitron',sans-serif;font-size:19px;font-weight:700;letter-spacing:0.05em;}
  .brand span{color:var(--teal);}
  .navlinks{display:flex;gap:28px;font-size:13px;letter-spacing:0.05em;text-transform:uppercase;}
  .navlinks a{color:var(--text-dim);transition:color .2s;}
  .navlinks a:hover{color:var(--teal);}
  .tc{font-size:12px;color:var(--text-dim);letter-spacing:0.05em;}
  .tc .rec{display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--orange);margin-right:6px;animation:pulse 1.4s infinite;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.2;}}
  .navtoggle{display:none;background:none;border:none;color:var(--text);font-size:22px;cursor:pointer;}
  .nav-icon{width:22px;height:22px;color:var(--teal);flex-shrink:0;}
  .nav-icon svg{width:100%;height:100%;}
  .nav-icon path,.nav-icon rect,.nav-icon circle{fill:none;stroke:currentColor;stroke-width:1.6;stroke-linecap:round;stroke-linejoin:round;}

  .hero-topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:28px;}

  /* ---------- hero ---------- */
  .hero{
    min-height:100svh;display:flex;flex-direction:column;justify-content:center;
    padding-top:120px;padding-bottom:60px;position:relative;
  }
  .hero::before{
    content:"";position:absolute;inset:0;pointer-events:none;
    background:
      radial-gradient(ellipse 60% 40% at 80% 20%, rgba(0,217,192,0.08), transparent 60%),
      radial-gradient(ellipse 50% 40% at 15% 80%, rgba(255,122,61,0.06), transparent 60%);
  }
  .eyebrow{
    font-family:'Share Tech Mono',monospace;font-size:12px;color:var(--teal);
    letter-spacing:0.15em;text-transform:uppercase;margin-bottom:18px;
    display:flex;align-items:center;gap:10px;
  }
  .eyebrow::before{content:"";width:28px;height:1px;background:var(--teal);}
  .hero h1{
    font-size:clamp(42px,7.5vw,96px);
    text-transform:uppercase;
  }
  .hero h1 .accent{color:var(--orange);}

  /* ---------- dust-to-word text reveal ---------- */
  .dchar{
    display:inline-block;
    opacity:0;
    filter:blur(10px);
    transform:translate(var(--dx,0),var(--dy,0)) rotate(var(--dr,0)) scale(.35);
    animation:dustIn .85s cubic-bezier(.16,.84,.44,1) forwards;
    animation-play-state:paused;
  }
  .title-text.go .dchar{animation-play-state:running;}
  @keyframes dustIn{
    0%{opacity:0;filter:blur(10px);transform:translate(var(--dx,0),var(--dy,0)) rotate(var(--dr,0)) scale(.35);}
    60%{opacity:1;filter:blur(2px);}
    100%{opacity:1;filter:blur(0);transform:translate(0,0) rotate(0) scale(1);}
  }
  @media (prefers-reduced-motion: reduce){
    .dchar{opacity:1;filter:none;transform:none;animation:none;}
  }
  .hero-sub{
    max-width:520px;color:var(--text-dim);font-size:17px;margin-top:22px;
  }
  .hero-cta{display:flex;gap:16px;margin-top:36px;flex-wrap:wrap;}
  .btn{
    font-family:'Share Tech Mono',monospace;font-size:13px;letter-spacing:0.05em;
    padding:14px 26px;border:1px solid var(--line);text-transform:uppercase;
    transition:all .2s;cursor:pointer;background:none;color:var(--text);
  }
  .btn-primary{background:var(--teal);color:#04120f;border-color:var(--teal);font-weight:600;}
  .btn-primary:hover{background:var(--orange);border-color:var(--orange);color:var(--text);}
  .btn-ghost:hover{border-color:var(--teal);color:var(--teal);}

  .hero-strip{
    display:flex;gap:0;margin-top:64px;border-top:1px solid var(--line);
    padding-top:20px;flex-wrap:wrap;
  }
  .hero-strip div{
    flex:1;min-width:130px;padding-right:24px;
  }
  .hero-strip .num{font-family:'Orbitron',sans-serif;font-size:34px;color:var(--teal);}
  .hero-strip .lbl{font-size:12px;color:var(--text-dim);text-transform:uppercase;letter-spacing:0.05em;margin-top:2px;}

  /* ---------- section shell ---------- */
  section{padding:110px 0;border-bottom:1px solid var(--line);}
  .sec-head{display:flex;align-items:flex-end;justify-content:space-between;margin-bottom:56px;flex-wrap:wrap;gap:16px;}
  .sec-eyebrow{font-family:'Share Tech Mono',monospace;font-size:12px;color:var(--text-dim);letter-spacing:0.15em;text-transform:uppercase;}
  .sec-title{font-size:clamp(36px,5vw,58px);text-transform:uppercase;margin-top:6px;}
  .sec-title .accent{color:var(--teal);}

  /* ---------- services (reels) ---------- */
  .reels{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--line);border:1px solid var(--line);}
  .reel{background:var(--panel);padding:36px 28px;transition:background .25s;}
  .reel:hover{background:var(--panel-2);}
  .reel .tag{font-family:'Share Tech Mono',monospace;font-size:12px;color:var(--orange);}
  .reel h3{font-size:26px;text-transform:uppercase;margin:14px 0 10px;}
  .reel p{font-size:14px;color:var(--text-dim);}

  /* ---------- portfolio ---------- */
  .grid{display:grid;grid-template-columns:repeat(3,1fr);gap:26px;}
  .frame{
    background:var(--panel);border:1px solid var(--line);
    aspect-ratio:16/10;position:relative;overflow:hidden;cursor:pointer;
  }
  .frame .noise{
    position:absolute;inset:0;
    background:
      repeating-linear-gradient(135deg, rgba(255,255,255,0.025) 0px, rgba(255,255,255,0.025) 2px, transparent 2px, transparent 10px);
  }
  .frame .center{
    position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;
    color:var(--text-dim);
  }
  .frame .ph-label{font-family:'Share Tech Mono',monospace;font-size:11px;letter-spacing:0.1em;text-transform:uppercase;}
  .frame .cat{
    position:absolute;top:14px;left:14px;font-family:'Share Tech Mono',monospace;
    font-size:11px;color:var(--teal);letter-spacing:0.05em;text-transform:uppercase;
  }
  .frame .tc-mini{
    position:absolute;bottom:14px;right:14px;font-family:'Share Tech Mono',monospace;
    font-size:11px;color:var(--text-dim);
  }
  .frame-title{margin-top:14px;font-size:15px;font-weight:600;}
  .frame-meta{font-size:13px;color:var(--text-dim);margin-top:2px;}

  /* ---------- about ---------- */
  .about{display:grid;grid-template-columns:1fr 1fr;gap:64px;align-items:start;}
  .about p{color:var(--text-dim);font-size:16px;margin-bottom:18px;}
  .skills{display:grid;grid-template-columns:1fr 1fr;gap:12px 24px;margin-top:28px;}
  .skill{display:flex;justify-content:space-between;font-family:'Share Tech Mono',monospace;font-size:13px;
    border-bottom:1px solid var(--line);padding-bottom:8px;}
  .skill span:last-child{color:var(--teal);}

  .toolstrip{display:flex;flex-wrap:wrap;gap:10px;margin-top:24px;}
  .tool{font-family:'Share Tech Mono',monospace;font-size:12px;border:1px solid var(--line);padding:7px 12px;color:var(--text-dim);}

  /* ---------- contact ---------- */
  .contact-wrap{display:grid;grid-template-columns:1fr 1.2fr;gap:64px;}
  .contact-info h2{font-size:clamp(34px,5vw,52px);text-transform:uppercase;}
  .contact-info p{color:var(--text-dim);margin-top:16px;max-width:380px;}
  .contact-links{margin-top:34px;display:flex;flex-direction:column;gap:14px;}
  .contact-links a{font-family:'Share Tech Mono',monospace;font-size:14px;color:var(--text);border-bottom:1px solid var(--line);padding-bottom:10px;display:flex;justify-content:space-between;transition:color .2s;}
  .contact-links a:hover{color:var(--teal);}

  form{display:flex;flex-direction:column;gap:20px;}
  .row2{display:grid;grid-template-columns:1fr 1fr;gap:20px;}
  label{font-family:'Share Tech Mono',monospace;font-size:11px;text-transform:uppercase;letter-spacing:0.08em;color:var(--text-dim);display:block;margin-bottom:8px;}
  input,select,textarea{
    width:100%;background:var(--panel);border:1px solid var(--line);color:var(--text);
    padding:13px 14px;font-family:'Rajdhani',sans-serif;font-size:16px;font-weight:500;
  }
  input:focus,select:focus,textarea:focus{outline:none;border-color:var(--teal);}
  textarea{resize:vertical;min-height:120px;}
  .submit-row{display:flex;align-items:center;gap:18px;margin-top:6px;}
  .form-note{font-size:12px;color:var(--text-dim);}

  footer{padding:36px 0;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px;}
  footer .mono{font-size:12px;color:var(--text-dim);}
  .footer-links{display:flex;gap:20px;}
  .footer-links a{font-size:12px;color:var(--text-dim);}
  .footer-links a:hover{color:var(--teal);}

  @media (max-width:860px){
    .navlinks{display:none;}
    .navtoggle{display:block;}
    .reels{grid-template-columns:1fr 1fr;}
    .grid{grid-template-columns:1fr 1fr;}
    .about,.contact-wrap{grid-template-columns:1fr;}
    .row2{grid-template-columns:1fr;}
  }
  @media (max-width:560px){
    .reels{grid-template-columns:1fr;}
    .grid{grid-template-columns:1fr;}
    .hero-strip{gap:20px 0;}
  }

  @media (prefers-reduced-motion: reduce){
    html{scroll-behavior:auto;}
    .tc .rec{animation:none;}
    *{transition:none !important;}
  }

  .reveal{opacity:0;transform:translateY(16px);transition:opacity .6s ease,transform .6s ease;}
  .reveal.in{opacity:1;transform:translateY(0);}

  /* ---------- intro / loading screen ---------- */
  #intro{
    position:fixed;inset:0;z-index:1000;background:var(--bg);
    display:flex;flex-direction:column;align-items:center;justify-content:center;gap:34px;
    transition:opacity .7s ease, visibility .7s ease;
  }
  #intro.hide{opacity:0;visibility:hidden;pointer-events:none;}
  body.locked{overflow:hidden;}
  .intro-icons{display:flex;gap:56px;align-items:center;}
  .intro-icon{width:64px;height:64px;position:relative;}
  .intro-icon svg{width:100%;height:100%;overflow:visible;}
  .intro-icon path,.intro-icon .flame{
    fill:none;stroke:var(--text-dim);stroke-width:1.4;
    stroke-linecap:round;stroke-linejoin:round;
    stroke-dasharray:300;stroke-dashoffset:300;
    animation:draw 1.1s ease forwards;
  }
  .intro-icon.laptop path{animation-delay:.35s;}
  .intro-icon.car path{animation-delay:.7s;}
  .intro-icon .lit{
    fill:var(--teal);stroke:none;opacity:0;transform-origin:center;
    animation:litup .5s ease forwards;
  }
  .intro-icon.fire .lit{animation-delay:.9s;}
  .intro-icon.laptop .lit{animation-delay:1.25s;}
  .intro-icon.car .lit{animation-delay:1.6s;}
  @keyframes draw{to{stroke-dashoffset:0;}}
  @keyframes litup{to{opacity:1;}}

  .intro-label{
    font-family:'Share Tech Mono',monospace;font-size:12px;letter-spacing:0.2em;
    text-transform:uppercase;color:var(--text-dim);
    opacity:0;animation:litup .6s ease forwards;animation-delay:1.9s;
  }
  .intro-label .accent{color:var(--teal);}
  .intro-bar{
    width:220px;height:2px;background:var(--line);position:relative;overflow:hidden;
    opacity:0;animation:litup .4s ease forwards;animation-delay:1.9s;
  }
  .intro-bar::after{
    content:"";position:absolute;left:0;top:0;bottom:0;width:0%;
    background:var(--teal);
    animation:fillbar 1.3s ease forwards;animation-delay:2.0s;
  }
  @keyframes fillbar{to{width:100%;}}

  @media (prefers-reduced-motion: reduce){
    .intro-icon path,.intro-icon .lit,.intro-label,.intro-bar,.intro-bar::after{animation:none;opacity:1;stroke-dashoffset:0;width:100%;}
    #intro{transition:none;}
  }
</style>
</head>
<body class="locked">

<div id="intro" aria-hidden="true">
  <div class="intro-icons">
    <div class="intro-icon fire">
      <svg viewBox="0 0 64 64">
        <path d="M32 6c-6 10-18 16-18 30a18 18 0 0 0 36 0c0-9-5-13-8-18 0 6-4 9-7 9-4 0-6-3-6-7 0-6 4-10 3-14z"/>
        <path class="lit" d="M32 30c-3 4-6 6-6 11a6 6 0 0 0 12 0c0-3-2-4-3-6 0 2-1 3-2 3-2 0-2-2-2-3 0-2 1-3 1-5z"/>
      </svg>
    </div>
    <div class="intro-icon laptop">
      <svg viewBox="0 0 64 64">
        <path d="M14 16h36a2 2 0 0 1 2 2v24H12V18a2 2 0 0 1 2-2z"/>
        <path d="M6 48h52l-4 6H10z"/>
        <rect class="lit" x="18" y="21" width="28" height="16" rx="1"/>
      </svg>
    </div>
    <div class="intro-icon car">
      <svg viewBox="0 0 64 64">
        <path d="M8 40l4-13a4 4 0 0 1 4-3h32a4 4 0 0 1 4 3l4 13"/>
        <path d="M6 40h52v8a2 2 0 0 1-2 2h-4a4 4 0 0 1-8 0H26a4 4 0 0 1-8 0h-4a2 2 0 0 1-2-2z"/>
        <path d="M18 24h28l3 10H15z"/>
        <circle class="lit" cx="18" cy="48" r="3"/>
        <circle class="lit" cx="46" cy="48" r="3"/>
      </svg>
    </div>
  </div>
  <div class="intro-label">Loading <span class="accent">PixelVortex</span></div>
  <div class="intro-bar"></div>
</div>


<header>
  <div class="wrap nav">
    <!-- EDIT: replace with your name / studio name -->
    <div class="brand">PixelVortex<span>.</span></div>
    <nav class="navlinks">
      <a href="#work">Work</a>
      <a href="#services">Services</a>
      <a href="#about">About</a>
      <a href="#contact">Contact</a>
    </nav>
    <div class="tc mono"><span class="rec"></span><span id="clock">00:00:00:00</span></div>
    <button class="navtoggle">☰</button>
  </div>
</header>

<section class="hero">
  <div class="wrap">
    <div class="hero-topbar">
      <span class="nav-icon" title="Laptop">
        <svg viewBox="0 0 24 24">
          <rect x="4" y="5" width="16" height="11" rx="1"/>
          <path d="M2 19h20l-1.5-3h-17z"/>
        </svg>
      </span>
      <span class="nav-icon" title="Gamepad">
        <svg viewBox="0 0 24 24">
          <path d="M7 8h10a4 4 0 0 1 4 4l1 5a2 2 0 0 1-3.5 1.5L16 16H8l-2.5 2.5A2 2 0 0 1 2 17l1-5a4 4 0 0 1 4-4z"/>
          <path d="M8 11v3M6.5 12.5h3"/>
          <circle cx="15.5" cy="11.5" r="0.8" fill="currentColor" stroke="none"/>
          <circle cx="17.5" cy="13.5" r="0.8" fill="currentColor" stroke="none"/>
        </svg>
      </span>
    </div>
    <div class="eyebrow">VFX · 3D · Design · Edit</div>
    <h1 class="viewfinder"><span class="vf-tl"></span><span class="vf-tr"></span><span class="vf-bl"></span><span class="vf-br"></span>
      <span class="title-text">Frame by frame,<br>built for the <span class="accent">shot.</span></span>
    </h1>
    <p class="hero-sub">I'm a freelance VFX, 3D, and motion artist. I build compositing, modelling, graphic design, and video edits for brands, filmmakers, and studios who need work that holds up in the final cut.</p>
    <div class="hero-cta">
      <a href="#contact" class="btn btn-primary">Start a project</a>
      <a href="#work" class="btn btn-ghost">View the reel</a>
    </div>
    <div class="hero-strip">
      <div><div class="num">40+</div><div class="lbl">Projects delivered</div></div>
      <div><div class="num">4</div><div class="lbl">Disciplines covered</div></div>
      <div><div class="num">24H</div><div class="lbl">Avg. reply time</div></div>
      <div><div class="num">100%</div><div class="lbl">Client-first revisions</div></div>
    </div>
  </div>
</section>

<section id="services">
  <div class="wrap">
    <div class="sec-head">
      <div>
        <div class="sec-eyebrow">What I do</div>
        <h2 class="sec-title">Four <span class="accent">reels,</span> one artist</h2>
      </div>
      <p style="max-width:340px;color:var(--text-dim);font-size:14px;">Hire me for a single deliverable or the whole pipeline, from first model to final grade.</p>
    </div>
  </div>
  <div class="wrap">
    <div class="reels">
      <div class="reel reveal">
        <div class="tag">01 — VFX</div>
        <h3>Visual Effects</h3>
        <p>Compositing, rotoscoping, tracking, particle and simulation work for film, ads, and social content.</p>
      </div>
      <div class="reel reveal">
        <div class="tag">02 — 3D</div>
        <h3>3D Modelling</h3>
        <p>Hard-surface and organic modelling, texturing, lighting, and rendering for product and character work.</p>
      </div>
      <div class="reel reveal">
        <div class="tag">03 — Design</div>
        <h3>Graphics Design</h3>
        <p>Brand visuals, key art, thumbnails, and motion graphics that hold attention in the first second.</p>
      </div>
      <div class="reel reveal">
        <div class="tag">04 — Edit</div>
        <h3>Video Editing</h3>
        <p>Narrative and commercial edits, colour grading, sound design, and delivery in any aspect ratio.</p>
      </div>
    </div>
  </div>
</section>

<section id="work">
  <div class="wrap">
    <div class="sec-head">
      <div>
        <div class="sec-eyebrow">Selected work</div>
        <h2 class="sec-title">Recent <span class="accent">cuts</span></h2>
      </div>
      <p style="max-width:340px;color:var(--text-dim);font-size:14px;">Placeholder frames — swap these for stills or clips from your own projects.</p>
    </div>
    <div class="grid">
      <!-- EDIT: replace each .frame block with a real thumbnail image once you have one -->
      <div>
        <div class="frame viewfinder reveal"><span class="vf-tl"></span><span class="vf-tr"></span><span class="vf-bl"></span><span class="vf-br"></span>
          <div class="noise"></div>
          <div class="cat">VFX</div>
          <div class="center"><div class="ph-label">Add project still</div></div>
          <div class="tc-mini">00:12:04</div>
        </div>
        <div class="frame-title">Project Title</div>
        <div class="frame-meta">Compositing · 2026</div>
      </div>
      <div>
        <div class="frame viewfinder reveal"><span class="vf-tl"></span><span class="vf-tr"></span><span class="vf-bl"></span><span class="vf-br"></span>
          <div class="noise"></div>
          <div class="cat">3D</div>
          <div class="center"><div class="ph-label">Add project still</div></div>
          <div class="tc-mini">00:08:41</div>
        </div>
        <div class="frame-title">Project Title</div>
        <div class="frame-meta">Modelling &amp; render · 2026</div>
      </div>
      <div>
        <div class="frame viewfinder reveal"><span class="vf-
