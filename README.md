# Weather-Forecast-App-UI-UX-Design


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Atmos — Weather App UI/UX Design</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#1B2430;
    --ink-soft:#5B6472;
    --cloudline:#4A6FA5;
    --cloudline-soft:#9FB4D4;
    --amber:#F2A65A;
    --amber-deep:#D98A3D;
    --mist:#EEF1F4;
    --paper:#FFFFFF;
    --line:rgba(74,111,165,0.16);
    --dusk:#232B45;
    --dusk-2:#141A2E;
    --dusk-line:rgba(242,166,90,0.28);
    --rain:#3E8E8A;
    --alert:#C4564F;
    --radius-card:18px;
  }
  *{box-sizing:border-box;margin:0;padding:0;}
  html,body{background:var(--mist);}
  body{
    font-family:'Inter',sans-serif;
    color:var(--ink);
    min-height:100vh;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:48px 20px 80px;
    background:
      radial-gradient(1200px 600px at 15% -10%, #E4ECF6 0%, transparent 60%),
      radial-gradient(900px 500px at 100% 10%, #FDEFDF 0%, transparent 55%),
      var(--mist);
  }
  .stage-header{text-align:center;max-width:560px;margin-bottom:28px;}
  .stage-header .eyebrow{
    font-family:'JetBrains Mono',monospace;
    font-size:11px;
    letter-spacing:0.14em;
    text-transform:uppercase;
    color:var(--cloudline);
    margin-bottom:10px;
  }
  .stage-header h1{
    font-family:'Space Grotesk',sans-serif;
    font-weight:500;
    font-size:34px;
    letter-spacing:-0.01em;
  }
  .stage-header p{
    font-size:14.5px;
    color:var(--ink-soft);
    margin-top:8px;
    line-height:1.55;
  }

  .tabs{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    gap:6px;
    margin:26px 0 32px;
    max-width:640px;
  }
  .tab-btn{
    font-family:'Inter',sans-serif;
    font-size:13px;
    font-weight:500;
    color:var(--ink-soft);
    background:var(--paper);
    border:1px solid var(--line);
    padding:8px 16px;
    border-radius:100px;
    cursor:pointer;
    transition:all .18s ease;
  }
  .tab-btn:hover{border-color:var(--cloudline-soft);color:var(--ink);}
  .tab-btn.active{
    background:var(--ink);
    color:var(--paper);
    border-color:var(--ink);
  }

  .layout{
    display:flex;
    gap:40px;
    align-items:flex-start;
    max-width:900px;
    width:100%;
    justify-content:center;
  }

  .device{
    width:375px;
    min-width:375px;
    height:790px;
    background:var(--dusk-2);
    border-radius:46px;
    padding:14px;
    box-shadow:0 30px 60px -20px rgba(20,26,46,0.35), 0 0 0 1px rgba(20,26,46,0.08);
    position:relative;
  }
  .device::before{
    content:"";
    position:absolute;
    top:28px; left:50%; transform:translateX(-50%);
    width:100px; height:22px;
    background:var(--dusk-2);
    border-radius:14px;
    z-index:5;
  }
  .screen-viewport{
    width:100%; height:100%;
    background:var(--mist);
    border-radius:32px;
    overflow:hidden;
    position:relative;
  }
  .screen{
    position:absolute; inset:0;
    display:flex; flex-direction:column;
    opacity:0; pointer-events:none;
    transform:translateY(8px);
    transition:opacity .22s ease, transform .22s ease;
    overflow-y:auto;
  }
  .screen::-webkit-scrollbar{display:none;}
  .screen{scrollbar-width:none;}
  .screen.active{opacity:1; pointer-events:auto; transform:translateY(0);}

  /* ---------- shared bits ---------- */
  .status-row{
    display:flex; justify-content:space-between; align-items:center;
    padding:20px 22px 0; font-family:'JetBrains Mono',monospace; font-size:11px;
  }
  .status-row.light{color:var(--ink-soft);}
  .status-row.dark{color:rgba(255,255,255,0.55);}

  .bottom-nav{
    display:flex; justify-content:space-around; align-items:center;
    padding:12px 8px 18px;
    background:var(--paper);
    border-top:1px solid var(--line);
    margin-top:auto;
  }
  .nav-item{
    display:flex; flex-direction:column; align-items:center; gap:4px;
    background:none; border:none; cursor:pointer;
    color:var(--cloudline-soft);
    font-family:'Inter',sans-serif; font-size:10px; font-weight:500;
    padding:4px 6px;
  }
  .nav-item svg{width:20px;height:20px;stroke:var(--cloudline-soft);transition:stroke .15s;}
  .nav-item.active{color:var(--ink);}
  .nav-item.active svg{stroke:var(--amber-deep);}

  svg.icon{width:22px;height:22px;fill:none;stroke:currentColor;stroke-width:1.6;stroke-linecap:round;stroke-linejoin:round;}

  /* ---------- isobar background ---------- */
  .isobars{position:absolute; inset:0; overflow:hidden; z-index:0;}
  .isobars svg{position:absolute; width:160%; left:-30%;}
  .isobars .l1{top:6%; animation:drift 34s linear infinite;}
  .isobars .l2{top:22%; animation:drift 46s linear infinite reverse;}
  .isobars .l3{top:40%; animation:drift 38s linear infinite;}
  @keyframes drift{ from{transform:translateX(0);} to{transform:translateX(-16.6%);} }

  /* ============ SPLASH ============ */
  .screen[data-screen="splash"]{
    background:linear-gradient(175deg, var(--dusk) 0%, var(--dusk-2) 100%);
    color:#F4F1E8; align-items:center; justify-content:center; text-align:center;
  }
  .splash-mark{
    width:78px;height:78px;border-radius:50%;
    border:1.4px solid var(--dusk-line);
    display:flex;align-items:center;justify-content:center;
    margin-bottom:22px; position:relative; z-index:1;
  }
  .splash-mark svg{width:38px;height:38px;stroke:var(--amber);}
  .splash h2{
    font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:30px;
    letter-spacing:-0.01em; position:relative; z-index:1;
  }
  .splash .tagline{
    font-family:'JetBrains Mono',monospace; font-size:12px; letter-spacing:0.08em;
    color:var(--amber); margin-top:8px; text-transform:uppercase; position:relative; z-index:1;
  }
  .splash .desc{
    font-size:13.5px; color:rgba(244,241,232,0.55); max-width:250px; margin:18px auto 40px;
    line-height:1.6; position:relative; z-index:1;
  }
  .get-started{
    position:relative; z-index:1;
    font-family:'Inter',sans-serif; font-weight:500; font-size:14.5px;
    background:var(--amber); color:var(--dusk-2);
    border:none; padding:15px 42px; border-radius:100px;
    cursor:pointer; letter-spacing:0.01em;
    transition:transform .15s ease;
  }
  .get-started:hover{transform:translateY(-2px);}
  .splash .skip{
    position:relative; z-index:1; margin-top:16px; background:none; border:none;
    color:rgba(244,241,232,0.45); font-size:12.5px; cursor:pointer; font-family:'Inter',sans-serif;
  }

  /* ============ HOME ============ */
  .home-header{padding:18px 22px 0; position:relative; z-index:1;}
  .home-loc{display:flex;align-items:center;gap:6px;}
  .home-loc svg{width:15px;height:15px;stroke:var(--cloudline);}
  .home-loc span{font-size:13.5px;font-weight:500;}
  .home-date{font-family:'JetBrains Mono',monospace;font-size:11px;color:var(--ink-soft);margin-top:3px;}
  .bell-btn{
    position:absolute; top:18px; right:22px;
    background:var(--paper); border:1px solid var(--line); border-radius:50%;
    width:36px;height:36px; display:flex;align-items:center;justify-content:center;
    cursor:pointer;
  }
  .bell-btn svg{width:17px;height:17px;stroke:var(--cloudline);}

  .hero{
    position:relative; margin:14px 22px 0; padding:26px 6px 10px; text-align:center;
  }
  .hero-illustration{width:120px;height:120px;margin:0 auto;position:relative;z-index:2;}
  .hero-temp{
    font-family:'Space Grotesk',sans-serif; font-weight:600; font-size:80px;
    line-height:1.15; padding-top:8px; margin-top:6px; position:relative; z-index:2;
    color:var(--ink); letter-spacing:-0.01em;
  }
  .hero-temp sup{font-size:28px; font-weight:500; vertical-align:32px; color:var(--ink-soft);}
  .hero-cond{font-size:15.5px; font-weight:600; position:relative; z-index:2; margin-top:2px;}
  .hero-range{
    font-family:'JetBrains Mono',monospace; font-size:11.5px; color:var(--ink-soft);
    margin-top:5px; position:relative; z-index:2;
  }

  .chip-row{display:flex; gap:8px; padding:18px 22px 4px; overflow-x:auto;}
  .chip{
    display:flex; align-items:center; gap:6px; white-space:nowrap;
    background:var(--paper); border:1px solid var(--line); border-radius:100px;
    padding:8px 12px; font-size:11.5px; font-family:'JetBrains Mono',monospace; color:var(--ink);
  }
  .chip svg{width:14px;height:14px;stroke:var(--cloudline);}

  .section-title{
    display:flex; justify-content:space-between; align-items:baseline;
    padding:20px 22px 10px;
  }
  .section-title h3{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:15px;}
  .section-title button{
    background:none;border:none;color:var(--cloudline);font-size:12px;
    font-family:'Inter',sans-serif;font-weight:500;cursor:pointer;
  }

  .hourly-strip{display:flex; gap:10px; padding:0 22px 6px; overflow-x:auto;}
  .hour-card{
    min-width:60px; background:var(--paper); border:1px solid var(--line);
    border-radius:14px; padding:12px 8px; text-align:center; flex-shrink:0;
  }
  .hour-card .t{font-size:10.5px; color:var(--ink-soft); font-family:'JetBrains Mono',monospace;}
  .hour-card svg{width:22px;height:22px;stroke:var(--cloudline); margin:8px auto;}
  .hour-card .temp{font-size:13px;font-weight:600;font-family:'Space Grotesk',sans-serif;}

  .day-row{
    display:flex; align-items:center; gap:12px;
    padding:11px 22px; border-bottom:1px solid var(--line);
  }
  .day-row:last-child{border-bottom:none;}
  .day-row .dname{width:38px; font-size:13px; font-weight:500;}
  .day-row svg{width:20px;height:20px;stroke:var(--cloudline); flex-shrink:0;}
  .day-row .range-track{
    flex:1; height:3px; background:var(--line); border-radius:2px; position:relative;
  }
  .day-row .range-fill{
    position:absolute; top:0; height:100%; border-radius:2px;
    background:linear-gradient(90deg, var(--cloudline), var(--amber));
  }
  .day-row .lo{font-size:12px; color:var(--ink-soft); font-family:'JetBrains Mono',monospace; width:26px;}
  .day-row .hi{font-size:12px; font-weight:600; font-family:'JetBrains Mono',monospace; width:30px; text-align:right;}

  /* ============ FORECAST ============ */
  .fc-header{padding:20px 22px 0;}
  .fc-header h2{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:22px;}
  .fc-toggle{
    display:flex; margin:16px 22px 4px; background:var(--paper);
    border:1px solid var(--line); border-radius:12px; padding:3px;
  }
  .fc-toggle button{
    flex:1; border:none; background:none; padding:9px; font-size:12.5px; font-weight:500;
    font-family:'Inter',sans-serif; color:var(--ink-soft); border-radius:9px; cursor:pointer;
  }
  .fc-toggle button.active{background:var(--ink); color:var(--paper);}
  .fc-panel{display:none; padding-bottom:10px;}
  .fc-panel.active{display:block;}
  .fc-hourly-list{padding:10px 22px;}
  .fc-hour-row{
    display:flex; align-items:center; gap:14px; padding:12px 0; border-bottom:1px solid var(--line);
  }
  .fc-hour-row .t{width:56px; font-size:12.5px; font-family:'JetBrains Mono',monospace; color:var(--ink-soft);}
  .fc-hour-row svg{width:22px;height:22px;stroke:var(--cloudline);}
  .fc-hour-row .cond{flex:1; font-size:12.5px; color:var(--ink-soft);}
  .fc-hour-row .precip{font-size:11px; color:var(--rain); font-family:'JetBrains Mono',monospace; width:34px;}
  .fc-hour-row .temp{font-size:14px; font-weight:600; font-family:'Space Grotesk',sans-serif; width:34px; text-align:right;}

  /* ============ SEARCH ============ */
  .search-header{padding:20px 22px 0;}
  .search-header h2{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:22px;}
  .search-bar{
    display:flex; align-items:center; gap:10px;
    background:var(--paper); border:1px solid var(--line); border-radius:14px;
    padding:12px 14px; margin:16px 22px;
  }
  .search-bar svg{width:18px;height:18px;stroke:var(--ink-soft); flex-shrink:0;}
  .search-bar input{
    border:none; outline:none; background:none; font-family:'Inter',sans-serif;
    font-size:13.5px; color:var(--ink); flex:1;
  }
  .search-bar input::placeholder{color:var(--cloudline-soft);}
  .recent-row{padding:0 22px;}
  .recent-chips{display:flex; flex-wrap:wrap; gap:8px; padding:0 22px 6px;}
  .recent-chip{
    display:flex; align-items:center; gap:6px;
    background:var(--paper); border:1px solid var(--line); border-radius:100px;
    padding:7px 12px 7px 12px; font-size:12px; font-family:'Inter',sans-serif; cursor:pointer;
  }
  .recent-chip svg{width:12px;height:12px;stroke:var(--cloudline-soft);}
  .suggested-row{
    display:flex; align-items:center; gap:12px;
    padding:13px 22px; border-bottom:1px solid var(--line); cursor:pointer;
  }
  .suggested-row:hover{background:rgba(74,111,165,0.05);}
  .suggested-row svg.pin{width:16px;height:16px;stroke:var(--cloudline); flex-shrink:0;}
  .suggested-row .info{flex:1;}
  .suggested-row .city{font-size:13.5px; font-weight:500;}
  .suggested-row .country{font-size:11px; color:var(--ink-soft);}
  .suggested-row .cond-icon svg{width:20px;height:20px;stroke:var(--cloudline);}
  .suggested-row .temp{font-size:14px; font-weight:600; font-family:'Space Grotesk',sans-serif; width:36px; text-align:right;}

  /* ============ DETAILS ============ */
  .det-header{padding:20px 22px 0;}
  .det-header h2{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:22px;}
  .det-header p{font-size:12.5px; color:var(--ink-soft); margin-top:4px;}
  .det-grid{
    display:grid; grid-template-columns:1fr 1fr; gap:10px; padding:18px 22px 22px;
  }
  .det-card{
    background:var(--paper); border:1px solid var(--line); border-radius:16px; padding:14px;
  }
  .det-card.wide{grid-column:1 / -1;}
  .det-card .top{display:flex; justify-content:space-between; align-items:flex-start;}
  .det-card svg{width:19px;height:19px;stroke:var(--cloudline);}
  .det-card .label{font-size:11px; color:var(--ink-soft); text-transform:uppercase; letter-spacing:0.04em; margin-top:10px;}
  .det-card .value{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:22px; margin-top:2px;}
  .det-card .sub{font-size:11px; color:var(--ink-soft); font-family:'JetBrains Mono',monospace; margin-top:2px;}
  .gauge-track{width:100%; height:4px; background:var(--line); border-radius:2px; margin-top:10px; position:relative;}
  .gauge-fill{position:absolute; top:0; left:0; height:100%; border-radius:2px; background:var(--amber);}
  .sun-arc{display:flex; justify-content:space-between; align-items:center; margin-top:12px;}
  .sun-arc .sa-item{text-align:center;}
  .sun-arc svg{width:20px;height:20px;stroke:var(--amber-deep);}
  .sun-arc .sa-time{font-family:'JetBrains Mono',monospace; font-size:13px; font-weight:500; margin-top:4px;}
  .sun-arc .sa-label{font-size:10px; color:var(--ink-soft); margin-top:1px;}

  /* ============ ALERTS ============ */
  .al-header{padding:20px 22px 0; position:relative; z-index:1;}
  .al-header h2{font-family:'Space Grotesk',sans-serif; font-weight:500; font-size:22px; color:#F4F1E8;}
  .al-header p{font-size:12.5px; color:rgba(244,241,232,0.55); margin-top:4px;}
  .screen[data-screen="alerts"]{background:linear-gradient(175deg, var(--dusk) 0%, var(--dusk-2) 55%, var(--mist) 55%);}
  .al-list{padding:16px 22px; position:relative; z-index:1;}
  .al-card{
    display:flex; gap:12px; align-items:flex-start;
    background:var(--paper); border-left:3px solid var(--cloudline); border-radius:0 14px 14px 0;
    padding:13px 14px; margin-bottom:10px;
  }
  .al-card.storm{border-left-color:var(--alert);}
  .al-card.temp{border-left-color:var(--amber-deep);}
  .al-card svg{width:20px;height:20px;stroke:var(--cloudline); flex-shrink:0; margin-top:2px;}
  .al-card.storm svg{stroke:var(--alert);}
  .al-card.temp svg{stroke:var(--amber-deep);}
  .al-card .body{flex:1;}
  .al-card .title{font-size:13px; font-weight:600;}
  .al-card .msg{font-size:12px; color:var(--ink-soft); margin-top:3px; line-height:1.4;}
  .al-card .time{font-size:10.5px; color:var(--cloudline-soft); font-family:'JetBrains Mono',monospace; margin-top:6px;}

  .pref-title{padding:6px 22px 8px; font-size:12px; font-weight:600; color:var(--ink-soft); text-transform:uppercase; letter-spacing:0.05em; position:relative; z-index:1;}
  .pref-row{
    display:flex; justify-content:space-between; align-items:center;
    padding:13px 22px; border-bottom:1px solid var(--line); position:relative; z-index:1; background:var(--mist);
  }
  .pref-row .name{font-size:13.5px; font-weight:500;}
  .pref-row .desc{font-size:11px; color:var(--ink-soft); margin-top:2px;}
  .toggle{
    width:38px; height:22px; border-radius:100px; background:var(--line);
    position:relative; cursor:pointer; border:none; flex-shrink:0;
  }
  .toggle::after{
    content:""; position:absolute; top:2px; left:2px; width:18px; height:18px;
    border-radius:50%; background:var(--paper); transition:transform .18s ease;
    box-shadow:0 1px 2px rgba(0,0,0,0.2);
  }
  .toggle.on{background:var(--amber);}
  .toggle.on::after{transform:translateX(16px);}

  .caption-panel{
    max-width:280px; padding-top:6px;
  }
  .caption-panel h4{
    font-family:'Space Grotesk',sans-serif; font-size:13px; font-weight:500;
    color:var(--cloudline); text-transform:uppercase; letter-spacing:0.06em; margin-bottom:14px;
  }
  .caption-item{margin-bottom:18px; padding-left:14px; border-left:2px solid var(--line);}
  .caption-item .n{font-family:'JetBrains Mono',monospace; font-size:10.5px; color:var(--amber-deep);}
  .caption-item .t{font-size:13.5px; font-weight:600; margin-top:2px;}
  .caption-item .d{font-size:12px; color:var(--ink-soft); margin-top:3px; line-height:1.5;}

  @media (max-width:820px){
    .layout{flex-direction:column; align-items:center;}
    .caption-panel{max-width:375px;}
  }
</style>
</head>
<body>

<div class="stage-header">
  <div class="eyebrow">UI / UX case study</div>
  <h1>Atmos — weather, read plainly</h1>
  <p>A weather app built around the visual language of instruments and pressure maps — thin isobar contour lines, mono data readouts, and a calm two-mode palette instead of another blue gradient sky.</p>
</div>

<div class="tabs" id="tabs"></div>

<div class="layout">
  <div class="device">
    <div class="screen-viewport" id="viewport">

      <!-- SPLASH -->
      <section class="screen splash" data-screen="splash">
        <div class="isobars">
          <svg class="l1" viewBox="0 0 1000 200" preserveAspectRatio="none"><path d="M0,100 C150,60 300,140 500,100 C700,60 850,140 1000,100" stroke="var(--dusk-line)" stroke-width="1" fill="none"/></svg>
          <svg class="l2" viewBox="0 0 1000 200" preserveAspectRatio="none"><path d="M0,80 C180,130 320,40 500,80 C680,130 820,40 1000,80" stroke="var(--dusk-line)" stroke-width="1" fill="none"/></svg>
          <svg class="l3" viewBox="0 0 1000 200" preserveAspectRatio="none"><path d="M0,120 C160,70 340,150 500,120 C660,70 840,150 1000,120" stroke="var(--dusk-line)" stroke-width="1" fill="none"/></svg>
        </div>
        <div class="splash-mark">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round"><circle cx="12" cy="12" r="3.6"/><path d="M12 3v2.4M12 18.6V21M3 12h2.4M18.6 12H21M5.6 5.6l1.7 1.7M16.7 16.7l1.7 1.7M5.6 18.4l1.7-1.7M16.7 7.3l1.7-1.7"/></svg>
        </div>
        <h2 class="splash">Atmos</h2>
        <div class="tagline">Read the sky</div>
        <p class="desc">Live conditions, hourly shifts and the week ahead — mapped clearly, wherever you are.</p>
        <button class="get-started" onclick="showScreen('home')">Get started</button>
        <button class="skip" onclick="showScreen('home')">Skip for now</button>
      </section>

      <!-- HOME -->
      <section class="screen" data-screen="home">
        <div class="status-row light"><span>9:41</span><span>KHI</span></div>
        <div class="home-header">
          <div class="home-loc">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 21s-7-6.1-7-11.2A7 7 0 0 1 19 9.8C19 14.9 12 21 12 21z"/><circle cx="12" cy="9.5" r="2.4"/></svg>
            <span id="homeLocation">Karachi, Sindh</span>
          </div>
          <div class="home-date">Mon, 24 Aug &middot; 2:14 PM</div>
          <button class="bell-btn" onclick="showScreen('alerts')">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"><path d="M6 10a6 6 0 0 1 12 0c0 4 1.5 5.5 1.5 5.5H4.5S6 14 6 10z"/><path d="M10 19a2 2 0 0 0 4 0"/></svg>
          </button>
        </div>

        <div class="hero">
          <div class="isobars" style="opacity:0.28;">
            <svg class="l1" viewBox="0 0 1000 200" preserveAspectRatio="none"><path d="M0,100 C150,60 300,140 500,100 C700,60 850,140 1000,100" stroke="var(--line)" stroke-width="1" fill="none"/></svg>
            <svg class="l3" viewBox="0 0 1000 200" preserveAspectRatio="none"><path d="M0,120 C160,70 340,150 500,120 C660,70 840,150 1000,120" stroke="var(--line)" stroke-width="1" fill="none"/></svg>
          </div>
          <div class="hero-illustration">
            <svg viewBox="0 0 120 120" fill="none">
              <circle cx="48" cy="46" r="20" stroke="var(--amber)" stroke-width="1.8"/>
              <g stroke="var(--amber)" stroke-width="1.8" stroke-linecap="round">
                <path d="M48 14v8M48 70v8M16 46h8M72 46h8M25 23l6 6M65 63l6 6M25 69l6-6M65 29l6-6"/>
              </g>
              <path d="M42 78c0-9 7-16 16-16 7 0 13 4.5 15 11 6 .6 10.5 5.6 10.5 11.5S78.5 96 72.5 96H45c-7 0-12.5-5.6-12.5-12.5C32.5 77.6 36.7 72.7 42 78z" fill="var(--paper)" stroke="var(--cloudline)" stroke-width="1.8"/>
            </svg>
          </div>
          <div class="hero-temp" id="homeTemp">29<sup>&deg;C</sup></div>
          <div class="hero-cond">Partly cloudy</div>
          <div class="hero-range">Feels like 31&deg; &middot; H:32&deg; L:24&deg;</div>
        </div>

        <div class="chip-row">
          <div class="chip"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none" stroke-width="1.6"><path d="M3 8h11a3 3 0 1 0-3-3M3 16h15a3 3 0 1 1-3 3M3 12h8"/></svg>14 km/h</div>
          <div class="chip"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none" stroke-width="1.6"><path d="M12 3c3 4.5 6 8 6 11.5A6 6 0 0 1 6 14.5C6 11 9 7.5 12 3z"/></svg>68% humidity</div>
          <div class="chip"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none" stroke-width="1.6"><circle cx="12" cy="12" r="4"/><path d="M12 3v2M12 19v2M4.2 4.2l1.4 1.4M18.4 18.4l1.4 1.4M3 12h2M19 12h2M4.2 19.8l1.4-1.4M18.4 5.6l1.4-1.4"/></svg>UV 7</div>
        </div>

        <div class="section-title"><h3>Hourly</h3><button onclick="showScreen('forecast')">See all</button></div>
        <div class="hourly-strip" id="homeHourly"></div>

        <div class="section-title"><h3>7-day forecast</h3><button onclick="showScreen('forecast')">See all</button></div>
        <div id="homeDays"></div>

        <div class="bottom-nav" id="nav1"></div>
      </section>

      <!-- FORECAST -->
      <section class="screen" data-screen="forecast">
        <div class="status-row light"><span>9:41</span><span>KHI</span></div>
        <div class="fc-header"><h2>Forecast</h2></div>
        <div class="fc-toggle">
          <button class="active" data-fc="hourly" onclick="setForecastTab('hourly')">Hourly</button>
          <button data-fc="week" onclick="setForecastTab('week')">7-day</button>
        </div>
        <div class="fc-panel active" data-fcpanel="hourly">
          <div class="fc-hourly-list" id="fcHourly"></div>
        </div>
        <div class="fc-panel" data-fcpanel="week">
          <div id="fcDays" style="padding:6px 0 10px;"></div>
        </div>
        <div class="bottom-nav" id="nav2"></div>
      </section>

      <!-- SEARCH -->
      <section class="screen" data-screen="search">
        <div class="status-row light"><span>9:41</span><span>KHI</span></div>
        <div class="search-header"><h2>Search location</h2></div>
        <div class="search-bar">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4.3-4.3"/></svg>
          <input type="text" placeholder="Search city or airport code">
        </div>
        <div class="section-title" style="padding-top:4px;"><h3>Recent searches</h3></div>
        <div class="recent-chips" id="recentChips"></div>
        <div class="section-title"><h3>Suggested for you</h3></div>
        <div id="suggestedList"></div>
        <div class="bottom-nav" id="nav3"></div>
      </section>

      <!-- DETAILS -->
      <section class="screen" data-screen="details">
        <div class="status-row light"><span>9:41</span><span>KHI</span></div>
        <div class="det-header"><h2>Weather details</h2><p>Karachi, Sindh &middot; updated 2 min ago</p></div>
        <div class="det-grid">
          <div class="det-card">
            <div class="top"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none"><path d="M12 3c3 4.5 6 8 6 11.5A6 6 0 0 1 6 14.5C6 11 9 7.5 12 3z"/></svg></div>
            <div class="label">Humidity</div>
            <div class="value">68%</div>
            <div class="sub">Dew point 22&deg;</div>
            <div class="gauge-track"><div class="gauge-fill" style="width:68%;"></div></div>
          </div>
          <div class="det-card">
            <div class="top"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none"><path d="M3 8h11a3 3 0 1 0-3-3M3 16h15a3 3 0 1 1-3 3M3 12h8"/></svg></div>
            <div class="label">Wind speed</div>
            <div class="value">14<span style="font-size:13px;">km/h</span></div>
            <div class="sub">From SW &middot; gusts 22</div>
            <div class="gauge-track"><div class="gauge-fill" style="width:42%;"></div></div>
          </div>
          <div class="det-card">
            <div class="top"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none"><circle cx="12" cy="12" r="4"/><path d="M12 3v2M12 19v2M4.2 4.2l1.4 1.4M18.4 18.4l1.4 1.4M3 12h2M19 12h2M4.2 19.8l1.4-1.4M18.4 5.6l1.4-1.4"/></svg></div>
            <div class="label">UV index</div>
            <div class="value">7 <span style="font-size:13px;font-weight:400;color:var(--ink-soft);">High</span></div>
            <div class="sub">Peaks 12&ndash;3pm</div>
            <div class="gauge-track"><div class="gauge-fill" style="width:64%;"></div></div>
          </div>
          <div class="det-card">
            <div class="top"><svg viewBox="0 0 24 24" stroke="currentColor" fill="none"><circle cx="12" cy="12" r="4"/><path d="M2 12h2M20 12h2M12 2v2M12 20v2M5 5l1.5 1.5M17.5 17.5L19 19M5 19l1.5-1.5M17.5 6.5L19 5"/></svg></div>
            <div class="label">Air quality</div>
            <div class="value">54 <span style="font-size:13px;font-weight:400;color:var(--ink-soft);">Moderate</span></div>
            <div class="sub">PM2.5 dominant</div>
            <div class="gauge-track"><div class="gauge-fill" style="width:54%;background:var(--rain);"></div></div>
          </div>
          <div class="det-card wide">
            <div class="label" style="margin-top:0;">Sunrise &amp; sunset</div>
            <div class="sun-arc">
              <div class="sa-item">
                <svg viewBox="0 0 24 24" stroke="currentColor" fill="none" stroke-linecap="round"><path d="M4 18h16M6 18a6 6 0 0 1 12 0"/><path d="M12 8v4M8.5 9.5l1 1M15.5 9.5l-1 1"/><path d="M12 2v3"/></svg>
                <div class="sa-time">5:52 AM</div>
                <div class="sa-label">Sunrise</div>
              </div>
              <div style="flex:1;height:1px;background:var(--line);margin:0 12px;position:relative;top:-14px;"></div>
              <div class="sa-item">
                <svg viewBox="0 0 24 24" stroke="currentColor" fill="none" stroke-linecap="round"><path d="M4 18h16M6 18a6 6 0 0 1 12 0"/><path d="M12 8v4M8.5 9.5l1 1M15.5 9.5l-1 1"/><path d="M12 22v-3"/></svg>
                <div class="sa-time">6:47 PM</div>
                <div class="sa-label">Sunset</div>
              </div>
            </div>
          </div>
        </div>
        <div class="bottom-nav" id="nav4"></div>
      </section>

      <!-- ALERTS -->
      <section class="screen" data-screen="alerts">
        <div class="status-row dark"><span>9:41</span><span>KHI</span></div>
        <div class="al-header"><h2>Alerts</h2><p>3 active for Karachi, Sindh</p></div>
        <div class="al-list">
          <div class="al-card">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M8 16c-2.5 0-4-1.9-4-4a4 4 0 0 1 4-4 5.5 5.5 0 0 1 10.6 1.6A3.5 3.5 0 0 1 18 16H8z"/><path d="M8 19l1.5-2M12 19l1.5-2M16 19l1.5-2"/></svg>
            <div class="body">
              <div class="title">Rain alert</div>
              <div class="msg">Light rain expected from 4:00&ndash;6:00 PM. Carry an umbrella if heading out.</div>
              <div class="time">Today, 2:10 PM</div>
            </div>
          </div>
          <div class="al-card storm">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M8 15c-2.5 0-4-1.9-4-4a4 4 0 0 1 4-4 5.5 5.5 0 0 1 10.6 1.6A3.5 3.5 0 0 1 18 15H8z"/><path d="M13 15l-3 5h3l-2 4"/></svg>
            <div class="body">
              <div class="title">Storm warning</div>
              <div class="msg">Thunderstorms possible overnight with gusty winds up to 45 km/h.</div>
              <div class="time">Tonight, 11:00 PM</div>
            </div>
          </div>
          <div class="al-card temp">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round"><path d="M12 3v11.5"/><circle cx="12" cy="17" r="3"/><path d="M9.5 6h5"/></svg>
            <div class="body">
              <div class="title">Temperature alert</div>
              <div class="msg">Heat index climbing to 38&deg; between 1&ndash;4 PM. Stay hydrated.</div>
              <div class="time">Today, 1:00 PM</div>
            </div>
          </div>
        </div>
        <div class="pref-title">Alert preferences</div>
        <div class="pref-row">
          <div><div class="name">Rain alerts</div><div class="desc">Notify before rain starts nearby</div></div>
          <button class="toggle on" onclick="this.classList.toggle('on')"></button>
        </div>
        <div class="pref-row">
          <div><div class="name">Storm warnings</div><div class="desc">Severe weather in your area</div></div>
          <button class="toggle on" onclick="this.classList.toggle('on')"></button>
        </div>
        <div class="pref-row">
          <div><div class="name">Temperature alerts</div><div class="desc">Extreme heat or cold thresholds</div></div>
          <button class="toggle" onclick="this.classList.toggle('on')"></button>
        </div>
        <div class="bottom-nav" id="nav5"></div>
      </section>

    </div>
  </div>

  <div class="caption-panel">
    <h4>Design notes</h4>
    <div class="caption-item"><div class="n">01</div><div class="t">Splash</div><div class="d">Isobar contour lines drift slowly behind the mark — the same line language used across the app, introduced first.</div></div>
    <div class="caption-item"><div class="n">02</div><div class="t">Home</div><div class="d">One hero reading, quick-glance chips, then hourly and weekly previews so nothing needs a tap to get the basics.</div></div>
    <div class="caption-item"><div class="n">03</div><div class="t">Forecast</div><div class="d">A single toggle switches the whole panel between hourly and 7-day, keeping one scroll axis instead of two.</div></div>
    <div class="caption-item"><div class="n">04</div><div class="t">Search</div><div class="d">Recent searches as tappable chips; suggested cities carry a live temperature so browsing itself is informative.</div></div>
    <div class="caption-item"><div class="n">05</div><div class="t">Details</div><div class="d">Instrument-style cards — mono numerals, thin gauges — for humidity, wind, UV, air quality and the sun arc.</div></div>
    <div class="caption-item"><div class="n">06</div><div class="t">Alerts</div><div class="d">Dusk-toned header signals urgency; colour on the card edge (not the whole card) keeps severity legible but calm.</div></div>
  </div>
</div>

<script>
  const screens = ['splash','home','forecast','search','details','alerts'];
  const navScreens = ['home','forecast','search','details','alerts'];

  const navIcons = {
    home: '<path d="M4 11.5 12 4l8 7.5"/><path d="M6 10v9a1 1 0 0 0 1 1h4v-6h2v6h4a1 1 0 0 0 1-1v-9"/>',
    forecast: '<rect x="4" y="5" width="16" height="15" rx="2"/><path d="M4 9.5h16M8 3v3.5M16 3v3.5"/><path d="M9 13.5h.01M12 13.5h.01M15 13.5h.01M9 16.5h.01M12 16.5h.01"/>',
    search: '<circle cx="11" cy="11" r="6.5"/><path d="M20 20l-4-4"/>',
    details: '<path d="M12 3v4M12 3a9 9 0 1 0 6.4 2.6"/><path d="M12 12l4-2"/>',
    alerts: '<path d="M6 10a6 6 0 0 1 12 0c0 4 1.5 5.5 1.5 5.5H4.5S6 14 6 10z"/><path d="M10 19a2 2 0 0 0 4 0"/>'
  };
  const navLabels = {home:'Home', forecast:'Forecast', search:'Search', details:'Details', alerts:'Alerts'};

  document.querySelectorAll('.bottom-nav').forEach(nav=>{
    nav.innerHTML = navScreens.map(s=>`
      <button class="nav-item" data-target="${s}" onclick="showScreen('${s}')">
        <svg class="icon" viewBox="0 0 24 24">${navIcons[s]}</svg>
        <span>${navLabels[s]}</span>
      </button>`).join('');
  });

  const tabsEl = document.getElementById('tabs');
  const tabLabels = {splash:'Splash', home:'Home', forecast:'Forecast', search:'Search', details:'Details', alerts:'Alerts'};
  tabsEl.innerHTML = screens.map(s=>`<button class="tab-btn" data-target="${s}" onclick="showScreen('${s}')">${tabLabels[s]}</button>`).join('');

  function showScreen(name){
    document.querySelectorAll('.screen').forEach(s=>s.classList.toggle('active', s.dataset.screen===name));
    document.querySelectorAll('.tab-btn').forEach(t=>t.classList.toggle('active', t.dataset.target===name));
    document.querySelectorAll('.nav-item').forEach(t=>t.classList.toggle('active', t.dataset.target===name));
  }

  function setForecastTab(which){
    document.querySelectorAll('.fc-toggle button').forEach(b=>b.classList.toggle('active', b.dataset.fc===which));
    document.querySelectorAll('.fc-panel').forEach(p=>p.classList.toggle('active', p.dataset.fcpanel===which));
  }

  const weatherIcons = {
    sun:'<circle cx="12" cy="12" r="4.2"/><path d="M12 2.5v3M12 18.5v3M2.5 12h3M18.5 12h3M5 5l2.1 2.1M16.9 16.9L19 19M5 19l2.1-2.1M16.9 7.1L19 5"/>',
    cloud:'<path d="M7 18a4.5 4.5 0 0 1-.6-9 6 6 0 0 1 11.4 1.8A4 4 0 0 1 17 18H7z"/>',
    cloudsun:'<path d="M9 17a3.6 3.6 0 0 1-.5-7.2 4.8 4.8 0 0 1 9.1 1.4A3.2 3.2 0 0 1 17 17H9z"/><path d="M5.5 6.5l1.2 1.2M4 11h1.7"/><circle cx="6" cy="7" r="2"/>',
    rain:'<path d="M7 14a4.2 4.2 0 0 1-.6-8.3 5.6 5.6 0 0 1 10.7 1.7A3.8 3.8 0 0 1 16.7 14H7z"/><path d="M8 17.5L7 20M12 17.5l-1 2.5M16 17.5l-1 2.5"/>',
    moon:'<path d="M20 14.5A8.5 8.5 0 1 1 9.5 4a6.8 6.8 0 0 0 10.5 10.5z"/>'
  };
  function ic(name, extraStyle){ return `<svg class="icon" style="${extraStyle||''}" viewBox="0 0 24 24">${weatherIcons[name]}</svg>`; }

  const hourly = [
    {t:'Now', icon:'cloudsun', temp:'29°', precip:'10%'},
    {t:'3 PM', icon:'sun', temp:'31°', precip:'0%'},
    {t:'4 PM', icon:'cloud', temp:'30°', precip:'20%'},
    {t:'5 PM', icon:'rain', temp:'27°', precip:'60%'},
    {t:'6 PM', icon:'rain', temp:'26°', precip:'55%'},
    {t:'7 PM', icon:'cloud', temp:'26°', precip:'25%'},
    {t:'8 PM', icon:'moon', temp:'25°', precip:'5%'},
    {t:'9 PM', icon:'moon', temp:'24°', precip:'0%'}
  ];
  document.getElementById('homeHourly').innerHTML = hourly.slice(0,6).map(h=>`
    <div class="hour-card"><div class="t">${h.t}</div>${ic(h.icon)}<div class="temp">${h.temp}</div></div>
  `).join('');
  document.getElementById('fcHourly').innerHTML = hourly.map(h=>`
    <div class="fc-hour-row">
      <div class="t">${h.t}</div>${ic(h.icon)}
      <div class="cond">${h.icon==='rain'?'Light rain':h.icon==='sun'?'Clear':h.icon==='moon'?'Clear night':'Partly cloudy'}</div>
      <div class="precip">${h.precip}</div>
      <div class="temp">${h.temp}</div>
    </div>`).join('');

  const days = [
    {d:'Today', icon:'cloudsun', lo:24, hi:32},
    {d:'Tue', icon:'rain', lo:23, hi:29},
    {d:'Wed', icon:'cloud', lo:24, hi:30},
    {d:'Thu', icon:'sun', lo:25, hi:33},
    {d:'Fri', icon:'sun', lo:25, hi:34},
    {d:'Sat', icon:'cloudsun', lo:24, hi:31},
    {d:'Sun', icon:'rain', lo:22, hi:28}
  ];
  const globalLo = Math.min(...days.map(d=>d.lo));
  const globalHi = Math.max(...days.map(d=>d.hi));
  function rangeBar(lo,hi){
    const left = ((lo-globalLo)/(globalHi-globalLo))*100;
    const width = ((hi-lo)/(globalHi-globalLo))*100;
    return `<div class="range-track"><div class="range-fill" style="left:${left}%;width:${width}%;"></div></div>`;
  }
  function dayRows(list){
    return list.map(d=>`
      <div class="day-row">
        <div class="dname">${d.d}</div>${ic(d.icon)}
        ${rangeBar(d.lo,d.hi)}
        <div class="lo">${d.lo}°</div>
        <div class="hi">${d.hi}°</div>
      </div>`).join('');
  }
  document.getElementById('homeDays').innerHTML = dayRows(days.slice(0,4));
  document.getElementById('fcDays').innerHTML = dayRows(days);

  const recent = ['Karachi','Lahore','Dubai'];
  document.getElementById('recentChips').innerHTML = recent.map(c=>`
    <div class="recent-chip"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 21s-7-6.1-7-11.2A7 7 0 0 1 19 9.8C19 14.9 12 21 12 21z"/><circle cx="12" cy="9.5" r="2"/></svg>${c}</div>
  `).join('');

  const suggested = [
    {city:'Islamabad', country:'Pakistan', temp:'26°', icon:'cloud'},
    {city:'Istanbul', country:'Türkiye', temp:'24°', icon:'sun'},
    {city:'London', country:'United Kingdom', temp:'18°', icon:'rain'},
    {city:'New York', country:'United States', temp:'27°', icon:'cloudsun'}
  ];
  document.getElementById('suggestedList').innerHTML = suggested.map(s=>`
    <div class="suggested-row" onclick="pickCity('${s.city}','${s.temp}')">
      <svg class="pin" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M12 21s-7-6.1-7-11.2A7 7 0 0 1 19 9.8C19 14.9 12 21 12 21z"/><circle cx="12" cy="9.5" r="2"/></svg>
      <div class="info"><div class="city">${s.city}</div><div class="country">${s.country}</div></div>
      <div class="cond-icon">${ic(s.icon)}</div>
      <div class="temp">${s.temp}</div>
    </div>`).join('');

  function pickCity(city, temp){
    document.getElementById('homeLocation').textContent = city;
    document.getElementById('homeTemp').innerHTML = temp.replace('°','') + '<sup>°C</sup>';
    showScreen('home');
  }

  showScreen('splash');
</script>
</body>
</html>
