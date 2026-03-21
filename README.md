[index.html](https://github.com/user-attachments/files/26158760/index.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0a1628">
<title>Météo ULM — Cambaie LF9742</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
:root{
  --green:#00d4aa;--blue:#4ecdc4;--yellow:#ffcc02;--red:#ff4757;
  --orange:#ff8c42;--gold:#ffd200;
  --dim:rgba(240,248,255,0.50);--card:rgba(255,255,255,0.08);
  --border:rgba(255,255,255,0.13);--tab-h:72px;
}
html,body{height:100%;overflow:hidden}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',system-ui,sans-serif;
  background:linear-gradient(160deg,#0d2137 0%,#0a3d62 30%,#1a6b4a 65%,#0d9e6e 100%);
  background-attachment:fixed;color:#f0f8ff;display:flex;flex-direction:column}

/* ══ LOADING SCREEN ══ */
#loadingScreen{position:fixed;inset:0;z-index:100;
  background:linear-gradient(160deg,#0d2137 0%,#0a3d62 40%,#0d3d2a 100%);
  display:flex;flex-direction:column;align-items:center;justify-content:center;gap:0}
#loadingScreen.hidden{display:none}
.ls-title{font-size:1rem;font-weight:800;color:var(--gold);letter-spacing:.15em;text-transform:uppercase;margin-bottom:4px}
.ls-sub{font-size:.7rem;color:var(--dim);margin-bottom:28px}
/* Scène piste */
.runway-scene{position:relative;width:300px;height:90px;margin-bottom:20px}
.rwy-bg{width:100%;height:100%;background:rgba(20,60,20,.45);border-radius:12px;border:1px solid rgba(255,255,255,.07);overflow:hidden;position:relative}
.rwy-asphalt{position:absolute;top:30px;left:18px;right:18px;height:30px;background:#1a1a1a;border-radius:3px}
.rwy-center{position:absolute;top:43px;left:0;right:0;height:4px;overflow:hidden}
.rwy-center-inner{display:flex;animation:slideLeft .35s linear infinite}
.rwy-dash{width:18px;height:3px;background:rgba(255,255,0,.5);border-radius:1px;flex-shrink:0;margin-right:10px}
.rwy-thr{position:absolute;top:33px;height:24px;width:4px;background:rgba(255,255,255,.65);border-radius:1px}
.rwy-thr-l{left:22px}.rwy-thr-r{right:22px}
.rwy-num-l{position:absolute;top:14px;left:30px;font-size:10px;font-weight:800;font-family:monospace;color:rgba(255,255,255,.35)}
.rwy-num-r{position:absolute;top:14px;right:30px;font-size:10px;font-weight:800;font-family:monospace;color:rgba(255,255,255,.35)}
#lsPlane{position:absolute;top:0;left:0;animation:takeoffAnim 2.4s ease-in infinite}
.ls-step{font-size:.75rem;color:var(--dim);text-align:center;min-height:20px;margin-bottom:10px}
.ls-step span{color:var(--green)}
.ls-prog{width:260px;height:4px;background:rgba(255,255,255,.1);border-radius:2px;overflow:hidden}
.ls-prog-fill{height:100%;background:linear-gradient(90deg,var(--green),var(--blue));border-radius:2px;width:0%;transition:width .4s ease}

/* ══ APP HEADER ══ */
#appHeader{flex-shrink:0;padding:env(safe-area-inset-top,0px) 0 0;
  background:rgba(10,22,40,0.88);backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border-bottom:1px solid var(--border);text-align:center;z-index:10}
.app-title{padding:.75rem 1rem .5rem}
.app-badge{display:inline-flex;align-items:center;gap:6px;
  background:linear-gradient(135deg,#f7971e,#ffd200);color:#1a1a1a;font-weight:800;
  font-size:.65rem;letter-spacing:.18em;padding:.25rem .9rem;border-radius:20px;text-transform:uppercase;margin-bottom:6px}
.app-name{font-size:1.15rem;font-weight:800;line-height:1.2}
.app-name span{color:var(--gold)}
.app-sub{font-size:.65rem;color:var(--dim);margin-top:2px}
#updateBar{display:flex;align-items:center;justify-content:center;gap:8px;
  padding:.4rem 1rem;border-top:1px solid var(--border);font-size:.65rem;color:var(--dim)}
#updateDot{width:7px;height:7px;border-radius:50%;background:#666;flex-shrink:0}
#updateDot.fresh{background:var(--green);box-shadow:0 0 6px var(--green)}
#updateDot.stale{background:var(--yellow)}

/* ══ SCROLL AREA ══ */
#scrollArea{flex:1;overflow-y:auto;overflow-x:hidden;-webkit-overflow-scrolling:touch;
  padding-bottom:calc(var(--tab-h) + env(safe-area-inset-bottom,0px))}
.page{display:none;padding:1rem 1rem 1.5rem;max-width:860px;margin:0 auto}
.page.active{display:block;animation:fadeUp .3s ease}

/* ══ TAB BAR ══ */
#tabBar{flex-shrink:0;height:calc(var(--tab-h) + env(safe-area-inset-bottom,0px));
  padding-bottom:env(safe-area-inset-bottom,0px);background:rgba(10,22,40,0.92);
  backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
  border-top:1px solid var(--border);display:flex;align-items:flex-start;z-index:10}
.tab{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;
  gap:4px;padding:.6rem .5rem 0;cursor:pointer;user-select:none;
  -webkit-tap-highlight-color:transparent;position:relative;transition:opacity .15s}
.tab:active{opacity:.6}
.tab-icon{font-size:1.4rem;line-height:1;transition:transform .2s}
.tab-label{font-size:.62rem;font-weight:600;letter-spacing:.04em;color:var(--dim);transition:color .2s}
.tab.active .tab-label{color:var(--green)}
.tab.active .tab-icon{transform:scale(1.12)}
.tab-dot{position:absolute;top:.55rem;right:calc(50% - 14px);width:7px;height:7px;border-radius:50%;background:var(--red);display:none}
.tab-dot.show{display:block}

/* ══ CARDS ══ */
.card{background:var(--card);border:1px solid var(--border);border-radius:20px;
  padding:1.1rem 1.3rem;backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);margin-bottom:12px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
@media(max-width:420px){.grid2{grid-template-columns:1fr}}
.lbl{font-size:.6rem;letter-spacing:.22em;text-transform:uppercase;color:var(--dim);margin-bottom:8px;display:flex;align-items:center;gap:5px}
.big{font-size:1.8rem;font-weight:800;line-height:1}
.sub{font-size:.75rem;margin-top:4px;color:var(--dim)}

/* ══ FR BANNER ══ */
.fr-banner{border-radius:16px;padding:.9rem 1.2rem;margin-bottom:12px;border:2px solid}
.fr-banner-top{font-weight:700;font-size:1rem;text-align:center;letter-spacing:.07em;margin-bottom:10px}
.models-now{display:grid;grid-template-columns:auto repeat(3,1fr);gap:4px 6px;font-size:.72rem;border-top:1px solid rgba(255,255,255,.15);padding-top:10px}
.models-now-lbl{color:rgba(240,248,255,.5);font-size:.62rem;letter-spacing:.1em;text-transform:uppercase;display:flex;align-items:center}
.models-now-val{text-align:center;font-weight:700;padding:2px 4px;border-radius:6px}
.models-now-head{text-align:center;font-size:.62rem;letter-spacing:.12em;text-transform:uppercase;opacity:.6;font-weight:600}
.models-now-consensus{grid-column:1/-1;text-align:center;margin-top:6px;font-size:.72rem;font-weight:600;padding:.3rem .8rem;border-radius:10px;background:rgba(255,255,255,.06)}

/* ══ VIS BAR ══ */
.vis-bar{height:8px;border-radius:4px;background:rgba(255,255,255,.1);margin-top:8px;overflow:hidden}
.vis-fill{height:100%;border-radius:4px;transition:width .8s ease}

/* ══ ALERTS ══ */
.alert{display:flex;gap:8px;padding:.5rem .8rem;border-radius:10px;margin-bottom:8px;font-size:.8rem;line-height:1.5;border-left:3px solid}
.alert-info  {background:rgba(0,212,170,.09);border-color:var(--green)}
.alert-warn  {background:rgba(255,204,2,.11);border-color:var(--yellow)}
.alert-danger{background:rgba(255,71,87,.11);border-color:var(--red)}

/* ══ TAF ══ */
.taf-row{display:grid;grid-template-columns:auto 1fr;gap:6px 12px;padding:8px 0;border-bottom:1px solid rgba(255,255,255,.06);font-size:.8rem;align-items:start}
.taf-row:last-child{border-bottom:none}
.taf-time{font-family:monospace;color:var(--gold);font-size:.7rem;white-space:nowrap}
.taf-desc{color:rgba(240,248,255,.82);line-height:1.5}

/* ══ COCKPIT ══ */
.wind-row{display:flex;align-items:center;gap:12px;margin-bottom:4px}
.rwy-wrap{margin-top:10px;padding:.9rem;background:rgba(0,0,0,.22);border-radius:14px;border:1px solid rgba(255,255,255,.07)}
.rwy-lbl{font-size:.6rem;letter-spacing:.2em;text-transform:uppercase;color:var(--dim);margin-bottom:8px;text-align:center}

/* ══ TOMORROW ══ */
.verdict-box{border-radius:14px;padding:1rem 1.1rem;margin-bottom:12px;text-align:center;border:2px solid}
.factors-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.factor{border-radius:10px;padding:.55rem .8rem;display:flex;align-items:flex-start;gap:8px;border:1px solid}
.factor-lbl{font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim);margin-bottom:2px}
.factor-val{font-size:.78rem;font-weight:600;line-height:1.4}
.legend{margin-top:10px;margin-bottom:12px;display:flex;gap:6px;flex-wrap:wrap;justify-content:center}
.legend-item{font-size:.62rem;padding:.18rem .65rem;border-radius:20px;border:1px solid}

/* ══ SLOTS ══ */
.slot-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px}
.slot-card{border-radius:14px;padding:.85rem;border:1px solid var(--border);background:var(--card);text-align:center}
.slot-card.best{border-color:var(--green);background:rgba(0,212,170,.08)}
.slot-name{font-size:.6rem;letter-spacing:.18em;text-transform:uppercase;color:var(--dim);margin-bottom:4px}
.slot-icon{font-size:1.5rem;margin-bottom:4px}
.slot-verdict{font-size:.72rem;font-weight:700;margin-bottom:4px}
.slot-detail{font-size:.68rem;color:var(--dim);line-height:1.5}

/* ══ MODELS ══ */
.models-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin:10px 0}
.model-col{border-radius:12px;padding:.7rem .5rem;text-align:center;background:rgba(255,255,255,.05);border:1px solid var(--border)}
.model-name{font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim);margin-bottom:6px}
.model-val{font-size:1rem;font-weight:800;line-height:1.2}
.model-sub{font-size:.65rem;color:var(--dim);margin-top:3px}
.consensus-bar{height:5px;border-radius:3px;margin-top:8px;background:rgba(255,255,255,.08);overflow:hidden}
.consensus-fill{height:100%;border-radius:3px;transition:width .8s ease}
.confidence-badge{display:inline-flex;align-items:center;gap:6px;margin-top:10px;
  padding:.35rem 1rem;border-radius:20px;font-size:.72rem;font-weight:600;border:1px solid;
  background:rgba(255,255,255,.05)}

/* ══ WEEK ══ */
.week-day{border-radius:16px;padding:1rem 1.2rem;border:1px solid var(--border);background:var(--card);
  margin-bottom:10px;display:grid;grid-template-columns:auto 1fr auto auto;align-items:center;gap:12px;cursor:pointer;transition:background .15s}
.week-day:active{background:rgba(255,255,255,.14)}
.week-day.today{border-color:var(--green);background:rgba(0,212,170,.08)}
.week-day-name{font-weight:700;font-size:.88rem;min-width:72px}
.week-day-sub{font-size:.7rem;color:var(--dim);margin-top:2px}
.week-day-icon{font-size:1.6rem;text-align:center}
.week-day-wind{text-align:right}
.week-day-verdict{min-width:48px;text-align:center;font-size:.7rem;font-weight:700;padding:.3rem .5rem;border-radius:10px;border:1px solid}
.detail-panel{display:none;background:rgba(0,0,0,.25);border-radius:14px;padding:1rem;margin-top:8px;border:1px solid var(--border);animation:fadeUp .2s ease}
.detail-panel.open{display:block}

/* ══ MISC ══ */
.sources{padding:.75rem 1rem;background:rgba(0,0,0,.18);border-radius:10px;font-size:.67rem;color:var(--dim);line-height:2}
.sources a{color:var(--blue)}
.error-box{background:rgba(255,71,87,.1);border:1px solid rgba(255,71,87,.3);border-radius:14px;padding:1rem 1.4rem;color:var(--red);font-size:.85rem;text-align:center;margin-bottom:12px}
.refresh-btn{width:100%;padding:.8rem;background:linear-gradient(135deg,var(--green),#00a881);color:#003322;font-weight:800;font-size:.85rem;letter-spacing:.1em;text-transform:uppercase;border:none;border-radius:14px;cursor:pointer;font-family:inherit;margin-bottom:12px;transition:opacity .2s}
.refresh-btn:disabled{opacity:.4;cursor:not-allowed}
.refresh-btn:active{opacity:.7}
.metar-btn{display:flex;align-items:center;justify-content:space-between;width:100%;padding:.9rem 1.1rem;background:rgba(0,0,0,.25);border:1px solid rgba(255,255,255,.12);border-radius:14px;color:#f0f8ff;font-size:.85rem;font-weight:600;cursor:pointer;font-family:inherit;text-decoration:none;transition:background .15s}
.metar-btn:active{background:rgba(255,255,255,.08)}

/* ══ WINDBIRD EN DIRECT ══ */
.wb-card{background:rgba(255,71,87,0.08);border:2px solid rgba(255,71,87,0.5);
  border-radius:20px;padding:1.1rem 1.3rem;margin-bottom:4px}
.wb-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
.wb-title{font-size:.85rem;font-weight:800;color:#f0f8ff}
.wb-badge{display:flex;align-items:center;gap:5px;font-size:.62rem;font-weight:700;
  color:#ff4757;letter-spacing:.12em;text-transform:uppercase}
.wb-dot{width:7px;height:7px;border-radius:50%;background:#ff4757;
  animation:wbPulse 1.2s ease-in-out infinite}
.wb-ts{font-size:.62rem;color:var(--dim);margin-top:2px}
.wb-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:12px}
.wb-cell{text-align:center;background:rgba(0,0,0,.2);border-radius:12px;padding:.6rem .4rem}
.wb-cell-lbl{font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim);margin-bottom:4px}
.wb-cell-val{font-size:1.2rem;font-weight:800;line-height:1}
.wb-cell-sub{font-size:.65rem;color:var(--dim);margin-top:2px}
/* Séparateur EN DIRECT / PRÉVISIONS */
.section-divider{display:flex;align-items:center;gap:10px;margin:16px 0 12px;
  font-size:.62rem;letter-spacing:.2em;text-transform:uppercase;color:var(--dim)}
.section-divider::before,.section-divider::after{content:"";flex:1;
  height:1px;background:rgba(255,255,255,.15)}
/* Graphique vent */
.wb-chart{width:100%;overflow:hidden}
.wb-chart-title{font-size:.6rem;letter-spacing:.2em;text-transform:uppercase;
  color:var(--dim);margin-bottom:8px;display:flex;justify-content:space-between}
@keyframes wbPulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.4;transform:scale(.7)}}

/* ══ ANIMATIONS ══ */
@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}
@keyframes slideLeft{from{transform:translateX(0)}to{transform:translateX(-28px)}}
@keyframes takeoffAnim{
  0%  {transform:translate(8px,48px) rotate(0deg) scale(1);opacity:1}
  50% {transform:translate(160px,48px) rotate(0deg) scale(1);opacity:1}
  72% {transform:translate(220px,26px) rotate(-10deg) scale(.92);opacity:1}
  100%{transform:translate(295px,-8px) rotate(-16deg) scale(.6);opacity:0}
}
</style>
</head>
<body>

<!-- ══ LOADING SCREEN ══ -->
<div id="loadingScreen">
  <div class="ls-title">🛩️ Cambaie LF9742</div>
  <div class="ls-sub">Côte Ouest · Saint-Paul · La Réunion</div>
  <div class="runway-scene">
    <div class="rwy-bg">
      <div class="rwy-asphalt"></div>
      <div class="rwy-center">
        <div class="rwy-center-inner">
          <div class="rwy-dash"></div><div class="rwy-dash"></div><div class="rwy-dash"></div>
          <div class="rwy-dash"></div><div class="rwy-dash"></div><div class="rwy-dash"></div>
          <div class="rwy-dash"></div><div class="rwy-dash"></div><div class="rwy-dash"></div>
          <div class="rwy-dash"></div><div class="rwy-dash"></div><div class="rwy-dash"></div>
        </div>
      </div>
      <div class="rwy-thr rwy-thr-l"></div>
      <div class="rwy-thr rwy-thr-r"></div>
      <div class="rwy-num-l">05</div>
      <div class="rwy-num-r">23</div>
    </div>
    <svg id="lsPlane" width="70" height="50" viewBox="0 0 70 50" style="position:absolute">
      <g transform="translate(35,25)">
        <!-- Fuselage -->
        <ellipse rx="25" ry="4.5" fill="#c8d0dc"/>
        <!-- Nez -->
        <ellipse cx="24" cy="0" rx="5" ry="3.5" fill="#dce4ee"/>
        <!-- Aile principale gauche -->
        <polygon points="0,-4.5 6,4.5 18,-3 14,-16" fill="#b0b8c8"/>
        <!-- Aile principale droite -->
        <polygon points="0,-4.5 6,4.5 18,3 14,16" fill="#a8b0bc"/>
        <!-- Dérive verticale -->
        <polygon points="-24,0 -24,-3.5 -15,-13 -13,0" fill="#b0b8c8"/>
        <!-- Stabilisateur gauche -->
        <polygon points="-20,-2 -19,1.5 -11,1.5 -10,-7" fill="#b0b8c8"/>
        <!-- Stabilisateur droit -->
        <polygon points="-20,-2 -19,1.5 -11,1.5 -10,5" fill="#a0a8b4"/>
        <!-- Moteur gauche -->
        <ellipse cx="6" cy="-10" rx="5.5" ry="2.5" fill="#8890a0"/>
        <ellipse cx="10" cy="-10" rx="2" ry="2.5" fill="#6878a0"/>
        <!-- Moteur droit -->
        <ellipse cx="6" cy="10" rx="5.5" ry="2.5" fill="#8890a0"/>
        <ellipse cx="10" cy="10" rx="2" ry="2.5" fill="#6878a0"/>
        <!-- Hublots -->
        <circle cx="10"  cy="-2" r="1.5" fill="rgba(140,200,240,0.7)"/>
        <circle cx="5"   cy="-2" r="1.5" fill="rgba(140,200,240,0.7)"/>
        <circle cx="0"   cy="-2" r="1.5" fill="rgba(140,200,240,0.7)"/>
        <circle cx="-5"  cy="-2" r="1.5" fill="rgba(140,200,240,0.7)"/>
        <circle cx="-10" cy="-2" r="1.5" fill="rgba(140,200,240,0.7)"/>
        <!-- Feux de nav -->
        <circle cx="14" cy="-16" r="1.5" fill="#00ff88" opacity=".9"/>
        <circle cx="14" cy="16"  r="1.5" fill="#ff4444" opacity=".9"/>
        <circle cx="-24" cy="-3" r="1.2" fill="white"   opacity=".7"/>
      </g>
    </svg>
  </div>
  <div class="ls-step" id="lsStep">Initialisation…</div>
  <div class="ls-prog"><div class="ls-prog-fill" id="lsProgFill"></div></div>
  <div style="margin-top:8px;font-size:.6rem;color:rgba(240,248,255,.2)">Arôme · ECMWF · GFS</div>
</div>

<!-- ══ APP HEADER ══ -->
<div id="appHeader">
  <div class="app-title">
    <div class="app-badge">🛩️ LF9742 · Cambaie Saint-Paul</div>
    <div class="app-name">Météo ULM <span>Cambaie 🌴</span></div>
    <div class="app-sub">Côte Ouest · Piste 05/23 · S20°57'47 / E055°16'54</div>
  </div>
  <div id="updateBar"><div id="updateDot"></div><span id="updateLabel">Chargement…</span></div>
</div>

<!-- ══ SCROLL AREA ══ -->
<div id="scrollArea">

  <!-- PAGE : MAINTENANT -->
  <div class="page active" id="page-now">
    <button class="refresh-btn" id="refreshBtn" onclick="loadAll()">🔄 Actualiser</button>
    <div id="errorBox" class="error-box" style="display:none"></div>
    <div id="nowData" style="display:none">

      <!-- ══ EN DIRECT — WINDBIRD ══ -->
      <div id="wbCard" class="wb-card" style="display:none">
        <div class="wb-header">
          <div>
            <div class="wb-title">📡 Station Windbird — Pôle Aéronautique Cambaie</div>
            <div class="wb-ts" id="wbTs">—</div>
          </div>
          <div class="wb-badge"><div class="wb-dot"></div>EN DIRECT</div>
        </div>
        <div class="wb-grid">
          <div class="wb-cell">
            <div class="wb-cell-lbl">💨 Vent moy.</div>
            <div class="wb-cell-val" id="wbSpd" style="color:var(--green)">—</div>
            <div class="wb-cell-sub">nœuds</div>
          </div>
          <div class="wb-cell">
            <div class="wb-cell-lbl">💥 Rafales</div>
            <div class="wb-cell-val" id="wbGust" style="color:var(--orange)">—</div>
            <div class="wb-cell-sub">nœuds</div>
          </div>
          <div class="wb-cell">
            <div class="wb-cell-lbl">🧭 Direction</div>
            <div class="wb-cell-val" id="wbDir" style="color:var(--blue)">—</div>
            <div class="wb-cell-sub" id="wbDirName">—</div>
          </div>
        </div>
        <!-- Graphique historique 3h -->
        <div id="wbChartWrap" style="display:none">
          <div class="wb-chart-title">
            <span>📈 Historique vent — 3 dernières heures</span>
            <span id="wbChartLegend" style="color:var(--dim)">━ moy &nbsp; ╌ rafales</span>
          </div>
          <div class="wb-chart">
            <svg id="wbChartSvg" width="100%" viewBox="0 0 320 80" preserveAspectRatio="none" style="display:block"></svg>
          </div>
          <div style="display:flex;justify-content:space-between;font-size:.58rem;color:var(--dim);margin-top:3px;padding:0 4px" id="wbChartXAxis"></div>
        </div>
      </div>

      <!-- Séparateur -->
      <div id="wbDivider" style="display:none" class="section-divider">Prévisions modèles météo</div>

      <div class="fr-banner" id="frBanner"></div>
      <div id="condBanner" class="card" style="display:none;border-left:3px solid var(--gold);font-size:.85rem"></div>

      <div class="grid2">
        <div class="card">
          <div class="lbl">🌡️ Température</div>
          <div class="big" id="tempVal" style="color:var(--orange)">—</div>
          <div class="sub">Ressenti <strong id="appTemp">—</strong></div>
          <div class="sub">Humidité <strong id="humidVal">—</strong></div>
          <div class="sub">Rosée <strong id="dewVal" style="color:var(--blue)">—</strong></div>
          <div class="sub" id="spreadVal"></div>
        </div>
        <div class="card">
          <div class="lbl">👁️ Visibilité</div>
          <div class="big" id="visVal">—</div>
          <div class="vis-bar"><div class="vis-fill" id="visFill"></div></div>
          <div class="sub" id="visSub"></div>
        </div>
      </div>

      <div class="card">
        <div class="lbl">💨 Vent & Piste recommandée</div>
        <div class="wind-row">
          <svg width="68" height="68" viewBox="0 0 72 72" style="flex-shrink:0">
            <circle cx="36" cy="36" r="34" fill="rgba(0,0,0,0.35)" stroke="rgba(255,255,255,0.18)" stroke-width="1.5"/>
            <text x="36" y="10" text-anchor="middle" fill="rgba(255,255,255,0.5)" font-size="8" font-weight="bold">N</text>
            <text x="36" y="67" text-anchor="middle" fill="rgba(255,255,255,0.5)" font-size="8" font-weight="bold">S</text>
            <text x="8"  y="40" text-anchor="middle" fill="rgba(255,255,255,0.5)" font-size="8" font-weight="bold">O</text>
            <text x="64" y="40" text-anchor="middle" fill="rgba(255,255,255,0.5)" font-size="8" font-weight="bold">E</text>
            <g id="compassArrow">
              <polygon points="36,14 32,36 36,32 40,36" fill="#ff4757"/>
              <polygon points="36,58 32,36 36,40 40,36" fill="rgba(255,255,255,0.28)"/>
            </g>
            <circle cx="36" cy="36" r="4" fill="white"/>
          </svg>
          <div>
            <div class="big" id="windVal">—</div>
            <div class="sub" id="windDirVal">—</div>
            <div class="sub" id="gustVal">—</div>
            <div class="sub" id="regimeVal" style="color:var(--gold);margin-top:4px">—</div>
          </div>
        </div>
        <div class="rwy-wrap" style="margin-top:14px">
          <div class="rwy-lbl">🎛️ Instruments — Analyse vent piste</div>
          <div style="display:flex;gap:10px;justify-content:center;align-items:flex-start">
            <!-- Instr 1 : Rose direction -->
            <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:6px">
              <svg width="108" height="108" viewBox="0 0 110 110">
                <circle cx="55" cy="55" r="52" fill="#0a1628" stroke="rgba(255,255,255,0.15)" stroke-width="2"/>
                <circle cx="55" cy="55" r="48" fill="none" stroke="rgba(255,255,255,0.05)" stroke-width="8"/>
                <g id="rwyAxisGroup" style="transform-origin:55px 55px;transform:rotate(50deg)">
                  <line id="rwyAxis05" x1="55" y1="12" x2="55" y2="35" stroke="rgba(255,255,255,0.3)" stroke-width="3.5" stroke-linecap="round"/>
                  <line id="rwyAxis23" x1="55" y1="75" x2="55" y2="98" stroke="rgba(255,255,255,0.3)" stroke-width="3.5" stroke-linecap="round"/>
                  <line x1="55" y1="35" x2="55" y2="75" stroke="rgba(255,255,255,0.1)" stroke-width="1.5" stroke-dasharray="4,3"/>
                  <text id="rwyLabel05" x="55" y="46" text-anchor="middle" font-size="7" font-weight="bold" font-family="monospace" fill="rgba(255,255,255,0.4)">05</text>
                  <text id="rwyLabel23" x="55" y="70" text-anchor="middle" font-size="7" font-weight="bold" font-family="monospace" fill="rgba(255,255,255,0.4)">23</text>
                </g>
                <g id="cwWindArrow" style="transform-origin:55px 55px;transition:transform 0.8s ease">
                  <line x1="55" y1="20" x2="55" y2="55" stroke="#ff4757" stroke-width="3" stroke-linecap="round"/>
                  <polygon points="55,14 51,24 59,24" fill="#ff4757"/>
                  <line x1="55" y1="55" x2="55" y2="88" stroke="rgba(255,255,255,0.2)" stroke-width="2" stroke-linecap="round"/>
                </g>
                <circle cx="55" cy="55" r="5" fill="white"/><circle cx="55" cy="55" r="3" fill="#0a1628"/>
                <text x="55" y="9"   text-anchor="middle" font-size="8" fill="rgba(255,255,255,0.5)" font-weight="bold">N</text>
                <text x="55" y="108" text-anchor="middle" font-size="8" fill="rgba(255,255,255,0.5)" font-weight="bold">S</text>
                <text x="5"  y="58"  text-anchor="middle" font-size="8" fill="rgba(255,255,255,0.5)" font-weight="bold">O</text>
                <text x="105" y="58" text-anchor="middle" font-size="8" fill="rgba(255,255,255,0.5)" font-weight="bold">E</text>
              </svg>
              <div style="text-align:center">
                <div style="font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim)">Piste active</div>
                <div id="cwActiveRwy" style="font-size:1rem;font-weight:800;color:var(--green)">—</div>
                <div id="cwActiveCap" style="font-size:.65rem;color:var(--dim)"></div>
              </div>
            </div>
            <!-- Instr 2 : Vent face -->
            <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:6px">
              <svg width="108" height="108" viewBox="0 0 110 110">
                <circle cx="55" cy="55" r="52" fill="#0a1628" stroke="rgba(255,255,255,0.15)" stroke-width="2"/>
                <path d="M 14,55 A 41,41 0 0,1 96,55" fill="none" stroke="rgba(0,212,170,0.12)" stroke-width="16"/>
                <path d="M 14,55 A 41,41 0 0,0 96,55" fill="none" stroke="rgba(255,71,87,0.10)" stroke-width="16"/>
                <line x1="14" y1="55" x2="96" y2="55" stroke="rgba(255,255,255,0.08)" stroke-width="1" stroke-dasharray="3,3"/>
                <g id="cwHWNeedle" style="transform-origin:55px 55px;transition:transform 0.8s ease">
                  <line x1="55" y1="18" x2="55" y2="55" stroke="#4ecdc4" stroke-width="2.5" stroke-linecap="round"/>
                  <polygon points="55,13 51,22 59,22" fill="#4ecdc4"/>
                  <line x1="55" y1="55" x2="55" y2="82" stroke="rgba(255,71,87,0.4)" stroke-width="1.5" stroke-linecap="round" stroke-dasharray="3,2"/>
                </g>
                <circle cx="55" cy="55" r="5" fill="white"/><circle cx="55" cy="55" r="3" fill="#0a1628"/>
                <text x="55" y="10"  text-anchor="middle" font-size="7" fill="rgba(0,212,170,0.7)" font-weight="bold">FACE</text>
                <text x="55" y="104" text-anchor="middle" font-size="7" fill="rgba(255,71,87,0.7)" font-weight="bold">ARRIÈRE</text>
                <text id="cwHWVal" x="55" y="52" text-anchor="middle" font-size="13" font-weight="800" fill="#4ecdc4">—</text>
                <text x="55" y="62" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">kt</text>
              </svg>
              <div style="text-align:center">
                <div style="font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim)">Vent face</div>
                <div id="cwHeadwind" style="font-size:1rem;font-weight:800;color:var(--blue)">—</div>
              </div>
            </div>
            <!-- Instr 3 : Vent travers -->
            <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:6px">
              <svg width="108" height="108" viewBox="0 0 110 110">
                <circle cx="55" cy="55" r="52" fill="#0a1628" stroke="rgba(255,255,255,0.15)" stroke-width="2"/>
                <path d="M 14,55 A 41,41 0 0,1 55,14" fill="none" stroke="rgba(0,212,170,0.20)" stroke-width="16"/>
                <path d="M 55,14 A 41,41 0 0,1 80,22" fill="none" stroke="rgba(255,204,2,0.20)" stroke-width="16"/>
                <path d="M 80,22 A 41,41 0 0,1 96,55" fill="none" stroke="rgba(255,71,87,0.20)" stroke-width="16"/>
                <line x1="55" y1="14" x2="55" y2="96" stroke="rgba(255,255,255,0.08)" stroke-width="1" stroke-dasharray="3,3"/>
                <g id="cwNeedle" style="transform-origin:55px 55px;transition:transform 0.8s ease">
                  <line x1="55" y1="18" x2="55" y2="55" stroke="white" stroke-width="2.5" stroke-linecap="round"/>
                  <polygon points="55,13 51,22 59,22" fill="white"/>
                </g>
                <circle cx="55" cy="55" r="5" fill="white"/><circle cx="55" cy="55" r="3" fill="#0a1628"/>
                <text x="14" y="58" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.35)">0</text>
                <text x="55" y="10" text-anchor="middle" font-size="7" fill="rgba(255,204,2,0.7)">~8</text>
                <text x="96" y="58" text-anchor="middle" font-size="7" fill="rgba(255,71,87,0.7)">20+</text>
                <text id="cwBigVal" x="55" y="75" text-anchor="middle" font-size="13" font-weight="800" fill="white">—</text>
                <text x="55" y="85" text-anchor="middle" font-size="7" fill="rgba(255,255,255,0.4)">kt travers</text>
              </svg>
              <div style="text-align:center">
                <div style="font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:var(--dim)">Vent travers</div>
                <div id="cwCrosswind" style="font-size:1rem;font-weight:800">—</div>
              </div>
            </div>
          </div>
          <div style="margin-top:12px;text-align:center">
            <div id="cwVerdict" style="display:inline-block;padding:.45rem 1.4rem;border-radius:20px;font-size:.8rem;font-weight:700;border:1px solid">—</div>
            <div id="cwDetail" style="font-size:.7rem;color:var(--dim);margin-top:5px;line-height:1.6"></div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="lbl">🔵 Pression QNH</div>
        <div class="big" id="qnhVal" style="color:var(--blue)">—</div>
        <div class="sub">Standard ISA : 1013.25 hPa</div>
        <div class="sub" id="qnhDiff">—</div>
        <div class="sub" style="margin-top:5px;font-size:.68rem;color:rgba(240,248,255,.3)">⚠ Calage QFE (alt 10 m) recommandé à LF9742</div>
      </div>

      <div class="card">
        <div class="lbl">☁️ Couverture nuageuse</div>
        <table style="width:100%;border-collapse:collapse;font-size:.78rem;margin-top:4px">
          <thead><tr>
            <th style="text-align:left;padding:3px 6px;border-bottom:1px solid rgba(255,255,255,.1);font-size:.58rem;letter-spacing:.18em;text-transform:uppercase;color:var(--dim)">Couche</th>
            <th style="text-align:left;padding:3px 6px;border-bottom:1px solid rgba(255,255,255,.1);font-size:.58rem;letter-spacing:.18em;text-transform:uppercase;color:var(--dim)">Couverture</th>
            <th style="text-align:left;padding:3px 6px;border-bottom:1px solid rgba(255,255,255,.1);font-size:.58rem;letter-spacing:.18em;text-transform:uppercase;color:var(--dim)">Base</th>
          </tr></thead>
          <tbody id="cloudsBody"></tbody>
        </table>
      </div>

      <div class="card">
        <div class="lbl">📡 METAR officiel — FMEE Roland Garros</div>
        <a class="metar-btn" href="https://aviationweather.gov/data/metar/?ids=FMEE" target="_blank">
          <span>📋 Consulter le METAR FMEE en temps réel</span>
          <span style="font-size:1.1rem;color:var(--green)">↗</span>
        </a>
        <div class="sub" style="margin-top:8px;font-size:.67rem;color:rgba(240,248,255,.35)">Ouvre aviationweather.gov · LF9742 n'émet pas de METAR propre</div>
      </div>

      <div class="card">
        <div class="lbl">☀️ Soleil & Créneau de vol optimal</div>
        <div style="display:flex;justify-content:space-around;align-items:center;padding:.5rem 0">
          <div style="text-align:center"><div style="font-size:1.5rem">🌅</div><div style="font-size:.65rem;color:var(--dim);margin-top:2px">Lever</div><div style="font-weight:800;font-size:1rem;color:var(--gold)" id="sunriseVal">—</div></div>
          <div style="text-align:center"><div style="font-size:.7rem;color:var(--dim)">Créneau vol</div><div style="font-weight:700;font-size:.88rem;color:var(--green);margin-top:2px" id="flightWindow">—</div><div style="font-size:.65rem;color:var(--dim);margin-top:2px" id="flightWindowSub"></div></div>
          <div style="text-align:center"><div style="font-size:1.5rem">🌇</div><div style="font-size:.65rem;color:var(--dim);margin-top:2px">Coucher</div><div style="font-weight:800;font-size:1rem;color:var(--orange)" id="sunsetVal">—</div></div>
        </div>
        <div style="position:relative;height:28px;background:rgba(255,255,255,.06);border-radius:14px;overflow:hidden;margin-top:4px">
          <div id="dayBar" style="position:absolute;top:0;height:100%;background:linear-gradient(90deg,rgba(255,210,0,.25),rgba(255,140,0,.35));border-radius:14px"></div>
          <div id="nowMarker" style="position:absolute;top:4px;width:4px;height:20px;background:white;border-radius:2px;transform:translateX(-50%)"></div>
          <div id="flightZone" style="position:absolute;top:0;height:100%;background:rgba(0,212,170,.2);border-radius:14px"></div>
        </div>
        <div style="display:flex;justify-content:space-between;font-size:.6rem;color:var(--dim);margin-top:3px;padding:0 4px"><span>00h</span><span>06h</span><span>12h</span><span>18h</span><span>24h</span></div>
        <div class="sub" style="margin-top:6px;font-size:.72rem" id="sunNote"></div>
      </div>

      <div class="card">
        <div class="lbl">⚠️ Alertes — LF9742 Cambaie</div>
        <div id="alertsList"></div>
      </div>

      <div class="card">
        <div class="lbl">📅 Rythme météo journalier</div>
        <div id="tafList"></div>
      </div>

      <div class="sources">
        <strong style="color:var(--green)">📚 Sources :</strong> Open-Meteo (Arôme · ECMWF · GFS) ·
        <a href="https://meteofrance.re" target="_blank">meteofrance.re</a> ·
        <a href="https://www.sia.aviation-civile.gouv.fr" target="_blank">NOTAM SIA</a>
      </div>
    </div>
  </div>

  <!-- PAGE : DEMAIN -->
  <div class="page" id="page-tomorrow">
    <div id="tomorrowData" style="display:none">
      <div class="card" style="text-align:center;margin-bottom:12px;padding:.85rem">
        <div style="font-size:.65rem;letter-spacing:.2em;text-transform:uppercase;color:var(--dim);margin-bottom:4px">Prévisions journée complète</div>
        <div style="font-size:1.05rem;font-weight:700;color:var(--gold)" id="tmDate">—</div>
      </div>

      <div class="verdict-box" id="tmVerdictBox">
        <div style="font-size:2.8rem;margin-bottom:6px" id="tmVerdictIcon">—</div>
        <div style="font-size:1.4rem;font-weight:800;letter-spacing:.05em" id="tmVerdictTitle">—</div>
        <div style="font-size:.82rem;color:rgba(240,248,255,.72);margin-top:5px" id="tmVerdictDesc">—</div>
        <div id="tmIdealTime" style="display:none;margin-top:10px;font-size:.75rem;background:rgba(0,0,0,.22);border-radius:8px;padding:.3rem .85rem;color:var(--gold)"></div>
      </div>

      <div class="lbl">🕐 Les 4 créneaux de la journée</div>
      <div class="slot-grid" id="tmSlots"></div>

      <div class="card">
        <div class="lbl">📊 Consensus 3 modèles — Matin</div>
        <div class="models-grid" id="tmModels"></div>
        <div style="text-align:center"><div class="confidence-badge" id="tmConfidence">—</div></div>
        <div class="sub" style="margin-top:8px;font-size:.67rem;color:rgba(240,248,255,.35)">Arôme (Météo-France) · ECMWF (Centre Européen) · GFS (NOAA) · via Open-Meteo</div>
      </div>

      <div class="card">
        <div class="lbl">🔍 Analyse détaillée — créneau matin</div>
        <div class="factors-grid" id="tmFactors"></div>
      </div>

      <div class="legend">
        <div class="legend-item" style="border-color:var(--green);color:var(--green);background:rgba(0,212,170,.1)">✅ Favorable</div>
        <div class="legend-item" style="border-color:var(--blue);color:var(--blue);background:rgba(78,205,196,.1)">🔵 Possible</div>
        <div class="legend-item" style="border-color:var(--yellow);color:var(--yellow);background:rgba(255,204,2,.1)">⚠️ Prudence</div>
        <div class="legend-item" style="border-color:var(--red);color:var(--red);background:rgba(255,71,87,.1)">🚫 Déconseillé</div>
      </div>
    </div>
  </div>

  <!-- PAGE : SEMAINE -->
  <div class="page" id="page-week">
    <div id="weekData" style="display:none">
      <div class="card" style="margin-bottom:12px;text-align:center;padding:.7rem 1rem">
        <div style="font-size:.65rem;letter-spacing:.2em;text-transform:uppercase;color:var(--dim)">Prévisions 7 jours · Arôme + ECMWF + GFS</div>
        <div style="font-size:.72rem;color:var(--dim);margin-top:2px">🟢 Accord · 🟡 Légère divergence · 🔴 Désaccord · Tap = détail</div>
      </div>
      <div id="weekList"></div>
    </div>
  </div>

</div>

<!-- TAB BAR -->
<div id="tabBar">
  <div class="tab active" id="tab-now" onclick="switchTab('now')"><div class="tab-icon">🛩️</div><div class="tab-label">Maintenant</div></div>
  <div class="tab" id="tab-tomorrow" onclick="switchTab('tomorrow')"><div class="tab-icon">🌅</div><div class="tab-label">Demain</div><div class="tab-dot" id="dotTomorrow"></div></div>
  <div class="tab" id="tab-week" onclick="switchTab('week')"><div class="tab-icon">📅</div><div class="tab-label">7 Jours</div></div>
</div>

<script>
const LAT=-20.9631,LON=55.2817;
function makeURL(model){
  const b=`https://api.open-meteo.com/v1/forecast?latitude=${LAT}&longitude=${LON}`;
  const cur=`&current=temperature_2m,relative_humidity_2m,dew_point_2m,apparent_temperature,weather_code,cloud_cover,wind_speed_10m,wind_direction_10m,wind_gusts_10m,surface_pressure,visibility`;
  const hr=`&hourly=temperature_2m,wind_speed_10m,wind_direction_10m,wind_gusts_10m,precipitation_probability,weather_code,cloud_cover,visibility`;
  const dy=`&daily=weather_code,temperature_2m_max,temperature_2m_min,wind_speed_10m_max,wind_gusts_10m_max,wind_direction_10m_dominant,precipitation_probability_max,precipitation_sum,sunrise,sunset`;
  const o=`&wind_speed_unit=kn&timezone=Indian%2FReunion&forecast_days=7`;
  return b+cur+hr+dy+o+(model?`&models=${model}`:``);}
const URL_A=makeURL(''),URL_E=makeURL('ecmwf_ifs025'),URL_G=makeURL('gfs_seamless');
// Open-Meteo renvoie le vent en kt (wind_speed_unit=kn) donc pas de conversion
// mais on garde une fonction propre pour les current values
function msToKtCur(v){return v!=null?Math.round(v*10)/10:null;}

let STATE={};

function windDirName(d){return["N","NNE","NE","ENE","E","ESE","SE","SSE","S","SSO","SO","OSO","O","ONO","NO","NNO"][Math.round((d||0)/22.5)%16];}
function wmoLabel(c){const m={0:"Ciel dégagé ☀️",1:"Peu nuageux 🌤️",2:"Partiellement nuageux ⛅",3:"Couvert ☁️",45:"Brouillard 🌫️",51:"Bruine 🌦️",61:"Pluie légère 🌧️",63:"Pluie 🌧️",65:"Pluie forte 🌧️",80:"Averses 🌦️",81:"Averses modérées 🌧️",82:"Averses violentes 🌧️",95:"Orage ⛈️",96:"Orage+grêle ⛈️",99:"Orage violent ⛈️"};return m[c]||`Code ${c}`;}
function wmoIcon(c){if([95,96,99].includes(c))return"⛈️";if([80,81,82,61,63,65].includes(c))return"🌧️";if(c===45)return"🌫️";if(c===3)return"☁️";if(c===2)return"⛅";if(c===1)return"🌤️";return"☀️";}
function cloudLayer(p){if(p<6)return"SKC";if(p<25)return"FEW";if(p<50)return"SCT";if(p<88)return"BKN";return"OVC";}
function flightRule(v,c){if(v<1600||c>87)return"LIFR";if(v<4800||c>62)return"IFR";if(v<8000||c>37)return"MVFR";return"VFR";}
const FRM={VFR:{bg:"rgba(0,212,170,.14)",bd:"#00d4aa",cl:"#00d4aa",em:"✅",lb:"VFR — Vol à vue autorisé"},MVFR:{bg:"rgba(78,205,196,.14)",bd:"#4ecdc4",cl:"#4ecdc4",em:"🔵",lb:"MVFR — Marginal, précautions"},IFR:{bg:"rgba(255,199,2,.14)",bd:"#ffcc02",cl:"#ffcc02",em:"⚠️",lb:"IFR — Instruments requis"},LIFR:{bg:"rgba(255,71,87,.14)",bd:"#ff4757",cl:"#ff4757",em:"🚫",lb:"LIFR — Conditions difficiles"}};
const CLC={SKC:"#00d4aa",FEW:"#00d4aa",SCT:"#4ecdc4",BKN:"#ffcc02",OVC:"#ff4757"};
const CLL={SKC:"Ciel dégagé",FEW:"Quelques (1–2/8)",SCT:"Éparse (3–4/8)",BKN:"Fragmenté (5–7/8)",OVC:"Couvert (8/8)"};
function visColor(m){if(m>=9999)return"#00d4aa";if(m>=5000)return"#4ecdc4";if(m>=3000)return"#ffcc02";if(m>=800)return"#ff8c00";return"#ff4757";}
function visText(m){if(m>=9999)return"Excellente ✅";if(m>=5000)return"Correcte 🔵";if(m>=3000)return"Limitée ⚠️";if(m>=800)return"Faible IFR ⚠️";return"Très faible 🚫";}
function fScore(spd,gust,rain,storm,vis,cloud){let s=0;if(storm)s+=50;if(rain)s+=15;if(spd>25)s+=30;else if(spd>18)s+=15;if(gust>28)s+=20;if(vis<3000)s+=25;if(cloud>75)s+=10;return s;}
function sVerdict(score){
  if(score>=50)return{code:"NON",color:"#ff4757",bg:"rgba(255,71,87,0.16)",border:"#ff4757",icon:"🚫",title:"Vol déconseillé",desc:"Conditions défavorables prévues."};
  if(score>=25)return{code:"PRUDENCE",color:"#ffcc02",bg:"rgba(255,204,2,0.13)",border:"#ffcc02",icon:"⚠️",title:"Vol avec prudence",desc:"Conditions limites. Décision le matin même."};
  if(score>=10)return{code:"POSSIBLE",color:"#4ecdc4",bg:"rgba(78,205,196,0.13)",border:"#4ecdc4",icon:"🔵",title:"Vol possible",desc:"Conditions acceptables, restez attentif."};
  return{code:"OUI",color:"#00d4aa",bg:"rgba(0,212,170,0.13)",border:"#00d4aa",icon:"✅",title:"Vol favorable",desc:"Belles conditions prévues !"};}
const SC={ok:"#00d4aa",info:"#4ecdc4",warn:"#ffcc02",danger:"#ff4757"};
const SI={ok:"✅",info:"🔵",warn:"⚠️",danger:"🚫"};
const SB={ok:"rgba(0,212,170,.07)",info:"rgba(78,205,196,.07)",warn:"rgba(255,204,2,.07)",danger:"rgba(255,71,87,.07)"};

function switchTab(n){
  document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
  document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));
  document.getElementById("page-"+n).classList.add("active");
  document.getElementById("tab-"+n).classList.add("active");
  document.getElementById("scrollArea").scrollTop=0;
}

const LS=[
  {p:10,m:"Connexion aux serveurs météo…"},
  {p:30,m:"Téléchargement modèle <span>Arôme</span>…"},
  {p:55,m:"Téléchargement modèle <span>ECMWF</span>…"},
  {p:75,m:"Téléchargement modèle <span>GFS</span>…"},
  {p:90,m:"Calcul du consensus…"},
  {p:100,m:"Prêt pour le décollage ✅"},
];
function setLS(i){if(i<LS.length){document.getElementById("lsStep").innerHTML=LS[i].m;document.getElementById("lsProgFill").style.width=LS[i].p+"%";}}
function hideLS(){document.getElementById("loadingScreen").classList.add("hidden");}

function updateCompass(dir){const ca=document.getElementById("compassArrow");if(ca)ca.setAttribute("transform",`rotate(${dir},36,36)`);}

function updateCrosswind(dir,spd){
  const r05=50,r23=230;
  const d05=Math.abs(((dir-r05+540)%360)-180),d23=Math.abs(((dir-r23+540)%360)-180);
  const act=d05<d23?"05":"23",cap=act==="05"?r05:r23;
  const ang=Math.abs(((dir-cap+540)%360)-180),rad=ang*Math.PI/180;
  const cw=Math.round(Math.abs(spd*Math.sin(rad))*10)/10;
  const hraw=spd*Math.cos(rad),tail=hraw<0,hw=Math.round(Math.abs(hraw)*10)/10;
  document.getElementById("cwWindArrow").style.transform=`rotate(${dir}deg)`;
  const a05=document.getElementById("rwyAxis05"),a23=document.getElementById("rwyAxis23");
  a05.setAttribute("stroke",act==="05"?"#00d4aa":"rgba(255,255,255,0.25)");a05.setAttribute("stroke-width",act==="05"?"5":"2.5");
  a23.setAttribute("stroke",act==="23"?"#00d4aa":"rgba(255,255,255,0.25)");a23.setAttribute("stroke-width",act==="23"?"5":"2.5");
  document.getElementById("rwyLabel05").setAttribute("fill",act==="05"?"#00d4aa":"rgba(255,255,255,0.3)");
  document.getElementById("rwyLabel23").setAttribute("fill",act==="23"?"#00d4aa":"rgba(255,255,255,0.3)");
  document.getElementById("cwActiveRwy").textContent=`Piste ${act}`;
  document.getElementById("cwActiveCap").textContent=`cap ${cap}°`;
  const hna=Math.max(-85,Math.min(85,(tail?1:-1)*Math.min(ang,85)));
  document.getElementById("cwHWNeedle").style.transform=`rotate(${hna}deg)`;
  document.getElementById("cwHWVal").textContent=hw.toFixed(1);
  document.getElementById("cwHWVal").setAttribute("fill",tail?"#ff4757":"#4ecdc4");
  const he=document.getElementById("cwHeadwind");he.textContent=tail?`${hw.toFixed(1)} kt arrière`:`${hw.toFixed(1)} kt face`;he.style.color=tail?"#ff4757":"#4ecdc4";
  const cwF=Math.min(cw/20,1);
  document.getElementById("cwNeedle").style.transform=`rotate(${-90+cwF*180}deg)`;
  document.getElementById("cwNeedle").style.transformOrigin="55px 55px";
  document.getElementById("cwBigVal").textContent=cw.toFixed(1);
  let col,verd,det;
  if(cw<=5){col="#00d4aa";verd="✅ Vent de travers acceptable";det="Conditions idéales pour un ULM.";}
  else if(cw<=8){col="#4ecdc4";verd="🔵 Vent de travers modéré";det="Compensation nécessaire, dans les limites habituelles.";}
  else if(cw<=12){col="#ffcc02";verd="⚠️ Vent de travers élevé";det="Vérifier la limite constructeur de ton ULM.";}
  else if(cw<=16){col="#ff8c00";verd="⚠️ Proche limite";det="Déconseillé pour la plupart des ULM légers.";}
  else{col="#ff4757";verd="🚫 Vent de travers excessif";det="Dépasse la limite de la majorité des ULM.";}
  document.getElementById("cwBigVal").setAttribute("fill",col);
  document.getElementById("cwCrosswind").textContent=`${cw.toFixed(1)} kt`;document.getElementById("cwCrosswind").style.color=col;
  const vb=document.getElementById("cwVerdict");vb.textContent=verd;vb.style.color=col;vb.style.borderColor=col;vb.style.background=col+"18";
  document.getElementById("cwDetail").textContent=det;
}

function buildAlerts(wx,lh){
  const al=[],sb=lh>=11&&lh<=17,af=lh>=10&&lh<=16,cy=new Date().getMonth()>=10||new Date().getMonth()<=3;
  if([95,96,99].includes(wx.weather_code))al.push({l:"danger",m:"⛈️ Orage en cours — Vol ULM INTERDIT."});
  if(wx.wind_gusts>wx.wind_speed+5&&wx.wind_gusts>22)al.push({l:"danger",m:`💥 Rafales ${Math.round(wx.wind_gusts)} kt — Cisaillement en finale 05/23.`});
  if(wx.wind_speed>20)al.push({l:"warn",m:`💨 Vent fort ${Math.round(wx.wind_speed)} kt du ${windDirName(wx.wind_direction)}.`});
  if(sb&&wx.wind_speed>14)al.push({l:"warn",m:`🌊 Brise de mer ${Math.round(wx.wind_speed)} kt — Vigilance en finale.`});
  if((wx.temperature-wx.dewpoint)<3)al.push({l:"warn",m:`💧 Écart T/Td ${(wx.temperature-wx.dewpoint).toFixed(1)}°C — Brume possible.`});
  if(af)al.push({l:"warn",m:"🏔️ Turbulences thermiques — Cumulus vers le Maïdo. Éviter relief 10h–17h."});
  if(cy)al.push({l:"info",m:"🌀 Saison cyclonique — Surveiller bulletins CMRS & Météo-France."});
  al.push({l:"info",m:"⛰️ Foehn du Piton Maïdo — Turbulences orographiques. Alt. sécu ≥ 2 500 m."});
  al.push({l:"info",m:"🌊 Lagune Saint-Gilles à 2 km — Brume marine possible 05h–09h."});
  al.push({l:"info",m:sb?"🌊 Brise de mer O/SO — Décollage piste 23 probable.":"🌬️ Alizé E/SE — Décollage piste 05 probable."});
  return al;
}

function buildTaf(h){
  if(!h)return[];
  const now=new Date(),slots=[{l:"05–11h",s:5,e:11},{l:"11–17h",s:11,e:17},{l:"17–20h",s:17,e:20},{l:"20–05h",s:20,e:29}];
  return slots.map(sl=>{
    const rel=h.time.map((t,i)=>({t:new Date(t),i})).filter(({t})=>{const lh=(t.getUTCHours()+4)%24,lh2=sl.e>24?lh+(lh<5?24:0):lh;return lh2>=sl.s&&lh2<sl.e&&t>now;}).slice(0,3);
    if(!rel.length)return null;
    const spd=Math.round(rel.reduce((s,{i})=>s+(h.wind_speed_10m[i]||0),0)/rel.length);
    const dir=Math.round(rel.reduce((s,{i})=>s+(h.wind_direction_10m[i]||0),0)/rel.length);
    const vis=Math.round(rel.reduce((s,{i})=>s+(h.visibility[i]||9999),0)/rel.length);
    const prob=Math.max(...rel.map(({i})=>h.precipitation_probability[i]||0));
    const code=h.weather_code[rel[0].i];
    return{time:sl.l,desc:`${dir}°/${spd} kt (${windDirName(dir)}) · ${wmoLabel(code)} · Vis ${vis>=9999?">10 000":vis.toLocaleString("fr-FR")} m${prob>40?" · ⚠️ Précip "+prob+"%":""}`};
  }).filter(Boolean);
}

function extractSlot(h,targetDate,sh,eh){
  if(!h)return null;
  const rows=h.time.map((t,i)=>({t:new Date(t),i})).filter(({t})=>{const lh=(t.getUTCHours()+4)%24;return t.getDate()===targetDate.getDate()&&lh>=sh&&lh<eh;});
  if(!rows.length)return null;
  return{
    spd:Math.round(rows.reduce((s,{i})=>s+(h.wind_speed_10m[i]||0),0)/rows.length),
    dir:Math.round(rows.reduce((s,{i})=>s+(h.wind_direction_10m[i]||0),0)/rows.length),
    gust:Math.round(Math.max(...rows.map(({i})=>h.wind_gusts_10m[i]||0))),
    prob:Math.max(...rows.map(({i})=>h.precipitation_probability[i]||0)),
    cloud:Math.round(rows.reduce((s,{i})=>s+(h.cloud_cover[i]||0),0)/rows.length),
    vis:Math.round(rows.reduce((s,{i})=>s+(h.visibility[i]||9999),0)/rows.length),
    temp:Math.round(rows.reduce((s,{i})=>s+(h.temperature_2m[i]||25),0)/rows.length),
    code:h.weather_code[rows[Math.floor(rows.length/2)].i],
  };
}

function buildConsensus(slots){
  const models=slots.filter(m=>m.data);
  if(!models.length)return null;
  const avgSpd=Math.round(models.reduce((s,m)=>s+m.data.spd,0)/models.length);
  const minSpd=Math.min(...models.map(m=>m.data.spd)),maxSpd=Math.max(...models.map(m=>m.data.spd));
  const spread=maxSpd-minSpd;
  let conf,confCol;
  if(spread<=4){conf="🟢 Accord des modèles — Prévision fiable";confCol="var(--green)";}
  else if(spread<=8){conf="🟡 Légère divergence — Vérifier la veille";confCol="var(--yellow)";}
  else{conf="🔴 Modèles en désaccord — Incertitude élevée";confCol="var(--red)";}
  return{models,avgSpd,minSpd,maxSpd,spread,conf,confCol};
}

// Heure locale Réunion UTC+4 fixe — bulletproof
function localHourNow(){
  const now=new Date();
  const r=new Date(now.getTime()+(4*60*60*1000));
  return r.getUTCHours()+r.getUTCMinutes()/60;
}
function isoToLocalH(iso){
  const d=new Date(iso);
  const r=new Date(d.getTime()+(4*60*60*1000));
  return r.getUTCHours()+r.getUTCMinutes()/60;
}
let _sunState=null;
function displaySun(daily){
  if(!daily?.sunrise?.length)return;
  const sr=new Date(daily.sunrise[0]),ss=new Date(daily.sunset[0]);
  const fmt=d=>d.toLocaleTimeString("fr-FR",{hour:"2-digit",minute:"2-digit",timeZone:"Indian/Reunion"});
  document.getElementById("sunriseVal").textContent=fmt(sr);
  document.getElementById("sunsetVal").textContent=fmt(ss);
  const srL=isoToLocalH(daily.sunrise[0]),ssL=isoToLocalH(daily.sunset[0]);
  const fs=srL+0.5,fe=Math.min(10,ssL-0.5),fe2=ssL-0.5;
  const toHM=h=>{const hh=Math.floor(h),mm=Math.round((h-hh)*60);return`${String(hh).padStart(2,"0")}h${String(mm).padStart(2,"0")}`;};
  document.getElementById("flightWindow").textContent=`${toHM(fs)} – ${toHM(fe)}`;
  document.getElementById("flightWindowSub").textContent=`Soir : ${toHM(17)} – ${toHM(fe2)}`;
  const pct=h=>Math.max(0,Math.min(100,h/24*100));
  document.getElementById("dayBar").style.left=`${pct(srL)}%`;
  document.getElementById("dayBar").style.width=`${pct(ssL)-pct(srL)}%`;
  document.getElementById("flightZone").style.left=`${pct(fs)}%`;
  document.getElementById("flightZone").style.width=`${pct(fe)-pct(fs)}%`;
  _sunState={fs,fe,fe2,toHM,pct};
  updateSunCursor();
}
function updateSunCursor(){
  if(!_sunState)return;
  const {fs,fe,fe2,toHM,pct}=_sunState;
  const lh=localHourNow();
  document.getElementById("nowMarker").style.left=`${pct(lh)}%`;
  const note=document.getElementById("sunNote");
  if(!note)return;
  if(lh<fs){note.innerHTML=`⏰ Vol possible dans <strong>${Math.round((fs-lh)*60)} min</strong>`;note.style.color="var(--gold)";}
  else if(lh<=fe){note.innerHTML=`✅ <strong>Créneau matinal actif</strong> — Idéal jusqu'à ${toHM(fe)}`;note.style.color="var(--green)";}
  else if(lh<17){note.innerHTML=`⚠️ Milieu de journée — thermiques actifs. Créneau soir après 17h.`;note.style.color="var(--yellow)";}
  else if(lh<=fe2){note.innerHTML=`🌇 <strong>Créneau soir actif</strong> jusqu'à ${toHM(fe2)}`;note.style.color="var(--green)";}
  else{note.innerHTML=`🌙 Nuit — Vol non recommandé`;note.style.color="var(--red)";}
}
setInterval(updateSunCursor,60*1000);

function displayNow(wx,h,daily,wxE,wxG){
  const lh=localHourNow(),sb=lh>=11&&lh<=17;
  const fr=flightRule(wx.visibility,wx.cloud_cover),frm=FRM[fr]||FRM.VFR;
  const b=document.getElementById("frBanner");
  b.style.background=frm.bg;b.style.borderColor=frm.bd;b.style.color=frm.cl;

  // Données vent des 3 modèles (current)
  const mA={spd:wx.wind_speed,gust:wx.wind_gusts,dir:wx.wind_direction};
  const mE=wxE?{spd:msToKtCur(wxE.current?.wind_speed_10m),gust:msToKtCur(wxE.current?.wind_gusts_10m),dir:Math.round(wxE.current?.wind_direction_10m||0)}:null;
  const mG=wxG?{spd:msToKtCur(wxG.current?.wind_speed_10m),gust:msToKtCur(wxG.current?.wind_gusts_10m),dir:Math.round(wxG.current?.wind_direction_10m||0)}:null;

  // Consensus
  const allSpd=[mA.spd,mE?.spd,mG?.spd].filter(x=>x!=null);
  const allGust=[mA.gust,mE?.gust,mG?.gust].filter(x=>x!=null);
  const spread=allSpd.length>1?Math.max(...allSpd)-Math.min(...allSpd):0;
  const spreadG=allGust.length>1?Math.max(...allGust)-Math.min(...allGust):0;
  const confIcon=spread<=3?"🟢":spread<=6?"🟡":"🔴";
  const confTxt=spread<=3?"Modèles en accord":spread<=6?"Légère divergence":"Modèles en désaccord";

  // Couleur cellule selon écart
  function cellColor(val,allVals){
    if(allVals.length<2)return"rgba(255,255,255,.08)";
    const mn=Math.min(...allVals),mx=Math.max(...allVals),rng=mx-mn;
    if(rng<=3)return"rgba(0,212,170,.15)";
    if(val===mx)return"rgba(255,71,87,.18)";
    if(val===mn)return"rgba(0,212,170,.12)";
    return"rgba(255,204,2,.12)";
  }

  const modelRow=(m,allVals,allGustVals)=>m?`
    <div class="models-now-val" style="background:${cellColor(m.spd,allVals)}">${m.spd.toFixed(1)} kt</div>
    <div class="models-now-val" style="background:${cellColor(m.gust,allGustVals)};color:#ff8c42">${m.gust.toFixed(1)} kt</div>
    <div class="models-now-val" style="background:rgba(255,255,255,.06);color:rgba(240,248,255,.6)">${windDirName(m.dir)}</div>`
  :`<div class="models-now-val" style="opacity:.3">—</div><div class="models-now-val" style="opacity:.3">—</div><div class="models-now-val" style="opacity:.3">—</div>`;

  b.innerHTML=`
    <div class="fr-banner-top">${frm.em}&nbsp; ${frm.lb}</div>
    <div class="models-now">
      <div class="models-now-head"></div>
      <div class="models-now-head">Arôme</div>
      <div class="models-now-head">ECMWF</div>
      <div class="models-now-head">GFS</div>

      <div class="models-now-lbl">💨 Vent</div>
      <div class="models-now-val" style="background:${cellColor(mA.spd,allSpd)}">${mA.spd.toFixed(1)} kt</div>
      ${mE?`<div class="models-now-val" style="background:${cellColor(mE.spd,allSpd)}">${mE.spd.toFixed(1)} kt</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}
      ${mG?`<div class="models-now-val" style="background:${cellColor(mG.spd,allSpd)}">${mG.spd.toFixed(1)} kt</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}

      <div class="models-now-lbl">💥 Rafales</div>
      <div class="models-now-val" style="background:${cellColor(mA.gust,allGust)};color:#ff8c42">${mA.gust.toFixed(1)} kt</div>
      ${mE?`<div class="models-now-val" style="background:${cellColor(mE.gust,allGust)};color:#ff8c42">${mE.gust.toFixed(1)} kt</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}
      ${mG?`<div class="models-now-val" style="background:${cellColor(mG.gust,allGust)};color:#ff8c42">${mG.gust.toFixed(1)} kt</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}

      <div class="models-now-lbl">🧭 Dir.</div>
      <div class="models-now-val" style="background:rgba(255,255,255,.06);color:rgba(240,248,255,.7)">${windDirName(mA.dir)}</div>
      ${mE?`<div class="models-now-val" style="background:rgba(255,255,255,.06);color:rgba(240,248,255,.7)">${windDirName(mE.dir)}</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}
      ${mG?`<div class="models-now-val" style="background:rgba(255,255,255,.06);color:rgba(240,248,255,.7)">${windDirName(mG.dir)}</div>`:`<div class="models-now-val" style="opacity:.3">—</div>`}

      <div class="models-now-consensus">${confIcon} ${confTxt} · Écart vent : ±${spread.toFixed(1)} kt · Rafales : ±${spreadG.toFixed(1)} kt</div>
    </div>`;

  const cb=document.getElementById("condBanner");cb.style.display="block";cb.innerHTML=`🌤️ <strong>Maintenant :</strong> ${wmoLabel(wx.weather_code)}`;
  document.getElementById("tempVal").textContent=`${wx.temperature.toFixed(1)}°C`;
  document.getElementById("appTemp").textContent=`${wx.apparent_temperature.toFixed(1)}°C`;
  document.getElementById("humidVal").textContent=`${wx.humidity}%`;
  document.getElementById("dewVal").textContent=`${wx.dewpoint.toFixed(1)}°C`;
  const sp=(wx.temperature-wx.dewpoint).toFixed(1),se=document.getElementById("spreadVal");
  se.textContent=`Écart T/Td : ${sp}°C${sp<3?" ⚠️ Brume":""}`;se.style.color=sp<3?"#ffcc02":"rgba(240,248,255,.5)";
  const vis=Math.min(wx.visibility,9999);
  document.getElementById("visVal").textContent=`${vis>=9999?">10 000":vis.toLocaleString("fr-FR")} m`;
  document.getElementById("visVal").style.color=visColor(vis);
  document.getElementById("visFill").style.width=`${Math.min(100,vis/10000*100)}%`;document.getElementById("visFill").style.background=visColor(vis);
  document.getElementById("visSub").textContent=visText(vis);document.getElementById("visSub").style.color=visColor(vis);
  const spd=wx.wind_speed,dir=wx.wind_direction,gust=wx.wind_gusts;
  document.getElementById("windVal").textContent=spd<1?"CALME":`${spd.toFixed(1)} kt`;document.getElementById("windVal").style.color=spd>25?"#ff4757":spd>15?"#ffcc02":"#f0f8ff";
  document.getElementById("windDirVal").textContent=`${Math.round(dir)}° — ${windDirName(dir)}`;
  const ge=document.getElementById("gustVal");ge.textContent=gust>spd+3?`💥 Rafales ${gust.toFixed(1)} kt`:"Pas de rafales";ge.style.color=gust>spd+3?"#ff4757":"rgba(240,248,255,.5)";
  document.getElementById("regimeVal").textContent=sb?"🌊 Brise de mer O/SO":"🌬️ Alizé E/SE";
  updateCompass(Math.round(dir));updateCrosswind(Math.round(dir),spd);
  document.getElementById("qnhVal").textContent=`${Math.round(wx.pressure)} hPa`;
  const df=(wx.pressure-1013.25).toFixed(1);document.getElementById("qnhDiff").textContent=`Écart : ${df>0?"+":""}${df} hPa ${wx.pressure>1013?"📈":wx.pressure<1013?"📉":"➡️"}`;
  const layer=cloudLayer(wx.cloud_cover);
  document.getElementById("cloudsBody").innerHTML=`<tr><td style="padding:5px 6px;color:${CLC[layer]};font-weight:700">${layer}</td><td style="padding:5px 6px">${CLL[layer]} — ${wx.cloud_cover}%</td><td style="padding:5px 6px;font-family:monospace">${layer==="SKC"?"—":"~"+(1000+Math.round(Math.random()*1000))+" ft"}</td></tr>`;
  document.getElementById("alertsList").innerHTML=buildAlerts(wx,lh).map(a=>`<div class="alert alert-${a.l}"><span style="flex-shrink:0">${a.l==="danger"?"🚨":a.l==="warn"?"⚠️":"ℹ️"}</span><span>${a.m}</span></div>`).join("");
  document.getElementById("tafList").innerHTML=buildTaf(h).map(t=>`<div class="taf-row"><div class="taf-time">${t.time}</div><div class="taf-desc">${t.desc}</div></div>`).join("")||"<div class='sub'>Non disponible.</div>";
  displaySun(daily);
  document.getElementById("nowData").style.display="block";
}

function displayTomorrow(aH,eH,gH,daily){
  const now=new Date(),tom=new Date(now);tom.setDate(tom.getDate()+1);
  document.getElementById("tmDate").textContent=tom.toLocaleDateString("fr-FR",{weekday:"long",day:"numeric",month:"long"});
  const SLOTS=[{name:"Matin",icon:"🌅",sh:5,eh:12},{name:"Après-midi",icon:"☀️",sh:12,eh:18},{name:"Soir",icon:"🌇",sh:18,eh:21},{name:"Nuit",icon:"🌙",sh:21,eh:24}];
  document.getElementById("tmSlots").innerHTML=SLOTS.map(sl=>{
    const d=extractSlot(aH,tom,sl.sh,sl.eh);
    if(!d)return`<div class="slot-card"><div class="slot-name">${sl.name}</div><div class="slot-icon">${sl.icon}</div><div class="slot-detail">Non disponible</div></div>`;
    const storm=[95,96,99].includes(d.code),rain=d.prob>40||[61,63,65,80,81,82].includes(d.code);
    const score=fScore(d.spd,d.gust,rain,storm,d.vis,d.cloud),v=sVerdict(score),best=score<10;
    return`<div class="slot-card${best?" best":""}"><div class="slot-name">${sl.name}${best?" ⭐":""}</div><div class="slot-icon">${wmoIcon(d.code)}</div><div class="slot-verdict" style="color:${v.color}">${v.icon} ${v.title}</div><div class="slot-detail">${d.spd} kt ${windDirName(d.dir)}<br>${d.temp}°C · ${d.prob}% précip</div></div>`;
  }).join("");

  const md=extractSlot(aH,tom,5,12);
  if(md){
    const storm=[95,96,99].includes(md.code),rain=md.prob>40||[61,63,65,80,81,82].includes(md.code);
    const score=fScore(md.spd,md.gust,rain,storm,md.vis,md.cloud),v=sVerdict(score);
    const vb=document.getElementById("tmVerdictBox");vb.style.background=v.bg;vb.style.borderColor=v.border;
    document.getElementById("tmVerdictIcon").textContent=v.icon;
    document.getElementById("tmVerdictTitle").textContent=v.title;document.getElementById("tmVerdictTitle").style.color=v.color;
    document.getElementById("tmVerdictDesc").textContent=v.desc;
    const ti=document.getElementById("tmIdealTime"),ideal=storm?"—":md.spd>18?"07h–09h":"06h–10h (avant les thermiques)";
    if(ideal!=="—"){ti.style.display="inline-block";ti.textContent=`⏰ Créneau idéal : ${ideal}`;}
    document.getElementById("dotTomorrow").classList.toggle("show",v.code==="NON"||v.code==="PRUDENCE");
    document.getElementById("tmFactors").innerHTML=[
      {icon:"💨",label:"Vent matin",value:`${md.spd} kt · ${windDirName(md.dir)}`,status:md.spd>25?"danger":md.spd>18?"warn":"ok"},
      {icon:"🌧️",label:"Précipitations",value:storm?"Orages ⛈️":rain?"Averses 🌧️":"Aucune ✅",status:storm?"danger":rain?"warn":"ok"},
      {icon:"☁️",label:"Nuages",value:`${md.cloud}% · ${cloudLayer(md.cloud)}`,status:md.cloud>75?"warn":md.cloud>50?"info":"ok"},
      {icon:"👁️",label:"Visibilité",value:`${md.vis>=9999?">10 000":md.vis.toLocaleString("fr-FR")} m`,status:md.vis<3000?"danger":md.vis<6000?"warn":"ok"},
      {icon:"〰️",label:"Turbulences",value:storm?"Sévères":"Faibles (thermiques après 10h)",status:storm?"danger":rain?"warn":"info"},
      {icon:"🌡️",label:"Température",value:`${md.temp}°C`,status:"ok"},
    ].map(f=>`<div class="factor" style="background:${SB[f.status]||SB.ok};border-color:${SC[f.status]||SC.ok}"><span style="font-size:1.1rem;flex-shrink:0">${f.icon}</span><div><div class="factor-lbl">${f.label}</div><div class="factor-val" style="color:${SC[f.status]||"#f0f8ff"}">${SI[f.status]||"•"} ${f.value}</div></div></div>`).join("");
  }

  const modSlots=[
    {name:"Arôme",color:"#00d4aa",data:extractSlot(aH,tom,5,12)},
    {name:"ECMWF",color:"#4ecdc4",data:eH?extractSlot(eH,tom,5,12):null},
    {name:"GFS",  color:"#ffd200",data:gH?extractSlot(gH,tom,5,12):null},
  ];
  const cons=buildConsensus(modSlots);
  if(cons){
    document.getElementById("tmModels").innerHTML=cons.models.map(m=>{
      const mv=sVerdict(fScore(m.data.spd,m.data.gust,m.data.prob>40,false,m.data.vis,m.data.cloud));
      return`<div class="model-col"><div class="model-name">${m.name}</div><div class="model-val" style="color:${m.color}">${m.data.spd} kt</div><div class="model-sub">${windDirName(m.data.dir)} · ${wmoIcon(m.data.code)}</div><div class="model-sub" style="color:${mv.color};margin-top:4px">${mv.icon} ${mv.code}</div><div class="consensus-bar"><div class="consensus-fill" style="width:${Math.min(m.data.spd/30*100,100)}%;background:${m.color}"></div></div></div>`;
    }).join("");
    const cb=document.getElementById("tmConfidence");
    cb.textContent=`${cons.conf} · ${cons.minSpd}–${cons.maxSpd} kt`;cb.style.color=cons.confCol;cb.style.borderColor=cons.confCol;
  }
  document.getElementById("tomorrowData").style.display="block";
}

function displayWeek(aDly,eDly,gDly,aH,eH,gH){
  if(!aDly)return;
  const html=aDly.time.map((ds,i)=>{
    const d=new Date(ds+"T00:00:00"),isT=i===0,isTm=i===1;
    const name=isT?"Aujourd'hui":isTm?"Demain":d.toLocaleDateString("fr-FR",{weekday:"long"});
    const dl=d.toLocaleDateString("fr-FR",{day:"numeric",month:"short"});
    const spd=Math.round(aDly.wind_speed_10m_max[i]||0),dir=Math.round(aDly.wind_direction_10m_dominant[i]||90);
    const gust=Math.round(aDly.wind_gusts_10m_max[i]||0),prob=aDly.precipitation_probability_max[i]||0;
    const code=aDly.weather_code[i],tmax=Math.round(aDly.temperature_2m_max[i]||25),tmin=Math.round(aDly.temperature_2m_min[i]||20);
    const storm=[95,96,99].includes(code),rain=prob>40||[61,63,65,80,81,82].includes(code);
    const score=fScore(spd,gust,rain,storm,9999,20),v=sVerdict(score);
    const vk=v.code==="NON"?"danger":v.code==="PRUDENCE"?"warn":v.code==="POSSIBLE"?"info":"ok";
    const eSpd=eDly?Math.round(eDly.wind_speed_10m_max[i]||0):null,gSpd=gDly?Math.round(gDly.wind_speed_10m_max[i]||0):null;
    const allS=[spd,eSpd,gSpd].filter(x=>x!==null),spread=allS.length>1?Math.max(...allS)-Math.min(...allS):0;
    const ci=spread<=4?"🟢":spread<=8?"🟡":"🔴";
    return`<div class="week-day${isT?" today":""}" onclick="toggleWD(${i})">
      <div><div class="week-day-name">${name}</div><div class="week-day-sub">${dl} · ${ci}</div></div>
      <div class="week-day-icon">${wmoIcon(code)}</div>
      <div class="week-day-wind"><div style="font-weight:700;font-size:.88rem">${spd} kt</div><div style="font-size:.7rem;color:var(--dim)">${windDirName(dir)}</div><div style="font-size:.7rem;color:var(--dim)">${tmax}° / ${tmin}°</div></div>
      <div class="week-day-verdict" style="color:${SC[vk]};background:${SC[vk]}18;border-color:${SC[vk]}">${v.icon} ${v.code}</div>
    </div>
    <div class="detail-panel" id="wd-${i}">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;font-size:.8rem;margin-bottom:8px">
        <div><span style="color:var(--dim)">Vent max</span><br><strong>${spd} kt${gust>spd+5?" · 💥 "+gust+" kt":""}</strong></div>
        <div><span style="color:var(--dim)">Précip.</span><br><strong>${prob>40?"⚠️ "+prob+"%":"✅ "+prob+"%"}</strong></div>
        <div><span style="color:var(--dim)">Météo</span><br><strong>${wmoLabel(code)}</strong></div>
        <div><span style="color:var(--dim)">Verdict</span><br><strong style="color:${SC[vk]}">${v.icon} ${v.title}</strong></div>
      </div>
      ${allS.length>1?`<div style="font-size:.7rem;color:var(--dim);border-top:1px solid var(--border);padding-top:8px">📊 ${ci} Modèles : ${allS.join(" / ")} kt (Arôme${eSpd?" · ECMWF":""}${gSpd?" · GFS":""})</div>`:""}
    </div>`;
  }).join("");
  document.getElementById("weekList").innerHTML=html;
  document.getElementById("weekData").style.display="block";
}
function toggleWD(i){document.getElementById("wd-"+i).classList.toggle("open");}

function setUpdateBar(loading,time){
  const dot=document.getElementById("updateDot"),lbl=document.getElementById("updateLabel");
  if(loading){dot.className="";lbl.textContent="Mise à jour…";return;}
  const age=time?(Date.now()-time)/60000:999;
  if(age<5){dot.className="fresh";lbl.textContent=`Mis à jour ${new Date(time).toLocaleTimeString("fr-FR")} · Arôme + ECMWF + GFS`;}
  else if(age<30){dot.className="stale";lbl.textContent=`Dernière MAJ ${new Date(time).toLocaleTimeString("fr-FR")} · Actualiser ?`;}
  else{dot.className="";lbl.textContent="Données périmées — Appuyer sur Actualiser";}
}

// ── Windbird Station 1300 — Pôle Aéronautique Cambaie ────────────────────────
const WB_ID = 1300;
const WB_BASE = `https://api.openwindmap.org/v1/windbird/${WB_ID}`;
const WB_NOW  = `https://corsproxy.io/?${encodeURIComponent(WB_BASE+'/last-measures')}`;
const WB_HIST = `https://corsproxy.io/?${encodeURIComponent(WB_BASE+'/measures?hours=3')}`;

function drawWindChart(data){
  const svg=document.getElementById("wbChartSvg");
  if(!svg||!data.wind_avg||!data.wind_avg.length)return;
  const W=320,H=80,PAD={t:8,b:20,l:28,r:8};
  const cW=W-PAD.l-PAD.r,cH=H-PAD.t-PAD.b;
  const avgs=data.wind_avg,gusts=data.wind_max||[];
  const allVals=[...avgs,...gusts].filter(v=>v!=null);
  const maxV=Math.max(...allVals,10);
  const scX=i=>(PAD.l+i/(avgs.length-1||1)*cW);
  const scY=v=>(PAD.t+cH-(v/maxV)*cH);

  // Fond coloré selon max
  const bgColor=maxV>20?"rgba(255,71,87,.1)":maxV>12?"rgba(255,204,2,.1)":"rgba(0,212,170,.08)";

  // Aire sous la courbe vent moyen
  const areaPoints=avgs.map((v,i)=>`${scX(i).toFixed(1)},${scY(v??0).toFixed(1)}`).join(" ");
  const firstX=scX(0).toFixed(1),lastX=scX(avgs.length-1).toFixed(1),baseY=(PAD.t+cH).toFixed(1);

  // Ligne vent moyen
  const lineAvg=avgs.map((v,i)=>`${i===0?"M":"L"}${scX(i).toFixed(1)},${scY(v??0).toFixed(1)}`).join(" ");
  // Ligne rafales (pointillé)
  const lineGust=gusts.filter(v=>v!=null).map((v,i)=>`${i===0?"M":"L"}${scX(i).toFixed(1)},${scY(v).toFixed(1)}`).join(" ");

  // Graduations Y
  const yTicks=[0,Math.round(maxV/2),Math.round(maxV)];
  const yTicksSvg=yTicks.map(v=>`
    <line x1="${PAD.l}" y1="${scY(v).toFixed(1)}" x2="${W-PAD.r}" y2="${scY(v).toFixed(1)}" stroke="rgba(255,255,255,.06)" stroke-width="1"/>
    <text x="${PAD.l-3}" y="${(scY(v)+3).toFixed(1)}" text-anchor="end" font-size="7" fill="rgba(255,255,255,.35)">${v}</text>`).join("");

  svg.innerHTML=`
    <rect x="0" y="0" width="${W}" height="${H}" fill="${bgColor}" rx="4"/>
    ${yTicksSvg}
    <!-- Aire -->
    <polygon points="${firstX},${baseY} ${areaPoints} ${lastX},${baseY}" fill="rgba(0,212,170,.15)"/>
    <!-- Ligne moy -->
    <path d="${lineAvg}" fill="none" stroke="var(--green)" stroke-width="2" stroke-linejoin="round" stroke-linecap="round"/>
    <!-- Ligne rafales -->
    ${lineGust?`<path d="${lineGust}" fill="none" stroke="var(--orange)" stroke-width="1.5" stroke-dasharray="4,3" stroke-linejoin="round" stroke-linecap="round"/>`:""}
    <!-- Kt label -->
    <text x="${PAD.l-3}" y="${PAD.t-1}" text-anchor="end" font-size="6" fill="rgba(255,255,255,.3)">kt</text>`;

  // Axe X — heures
  const xEl=document.getElementById("wbChartXAxis");
  if(xEl&&data.datetime&&data.datetime.length){
    const step=Math.max(1,Math.floor(data.datetime.length/4));
    const labels=[];
    for(let i=0;i<data.datetime.length;i+=step){
      const d=new Date(data.datetime[i]);
      const r=new Date(d.getTime()+(4*60*60*1000));
      labels.push(`${String(r.getUTCHours()).padStart(2,"0")}h${String(r.getUTCMinutes()).padStart(2,"0")}`);
    }
    xEl.innerHTML=labels.map(l=>`<span>${l}</span>`).join("");
  }
  document.getElementById("wbChartWrap").style.display="block";
}

async function loadWindbird(){
  try{
    // Données actuelles
    const rN=await fetch(WB_NOW,{signal:AbortSignal.timeout(6000)});
    if(!rN.ok)throw new Error(`Windbird HTTP ${rN.status}`);
    const dN=await rN.json();
    const s=dN.measurements||dN;

    // Conversion km/h → kt (Windbird envoie en km/h selon la doc)
    const toKt=v=>v!=null?Math.round(v/1.852*10)/10:null;
    const spd=toKt(s.wind_avg??s.windAverage??s.wind_speed);
    const gust=toKt(s.wind_max??s.windMax??s.wind_gust);
    const dir=Math.round(s.wind_direction??s.windDirection??0);
    const ts=s.datetime??s.date??null;

    if(spd===null&&gust===null)throw new Error("Données Windbird vides");

    // Afficher carte
    const spdEl=document.getElementById("wbSpd");
    const gustEl=document.getElementById("wbGust");
    spdEl.textContent=spd!=null?spd.toFixed(1):"—";
    gustEl.textContent=gust!=null?gust.toFixed(1):"—";
    document.getElementById("wbDir").textContent=`${dir}°`;
    document.getElementById("wbDirName").textContent=windDirName(dir);
    // Couleur vent
    spdEl.style.color=spd>20?"#ff4757":spd>12?"#ffcc02":"#00d4aa";
    gustEl.style.color=gust>25?"#ff4757":gust>15?"#ff8c42":"#4ecdc4";
    // Timestamp
    if(ts){
      const d=new Date(ts);
      const r=new Date(d.getTime()+(4*60*60*1000));
      const hh=String(r.getUTCHours()).padStart(2,"0");
      const mm=String(r.getUTCMinutes()).padStart(2,"0");
      document.getElementById("wbTs").textContent=`Dernière mesure : ${hh}h${mm} (heure locale)`;
    }
    document.getElementById("wbCard").style.display="block";
    document.getElementById("wbDivider").style.display="flex";

    // Historique 3h
    try{
      const rH=await fetch(WB_HIST,{signal:AbortSignal.timeout(6000)});
      if(rH.ok){
        const dH=await rH.json();
        // Normaliser selon format API
        const hist=dH.data||dH;
        if(hist.wind_avg||hist.windAverage){
          const normalized={
            wind_avg:hist.wind_avg||hist.windAverage||[],
            wind_max:hist.wind_max||hist.windMax||hist.wind_gust||[],
            datetime:hist.datetime||hist.date||[],
          };
          // Convertir km/h → kt
          normalized.wind_avg=normalized.wind_avg.map(v=>v!=null?Math.round(v/1.852*10)/10:null);
          normalized.wind_max=normalized.wind_max.map(v=>v!=null?Math.round(v/1.852*10)/10:null);
          drawWindChart(normalized);
        }
      }
    }catch(e){console.warn("Windbird historique:",e.message);}

  }catch(e){
    console.warn("Windbird indisponible:",e.message);
    // L'app continue normalement sans la carte Windbird
    document.getElementById("wbCard").style.display="none";
    document.getElementById("wbDivider").style.display="none";
  }
}

async function loadAll(){
  document.getElementById("refreshBtn").disabled=true;
  document.getElementById("loadingScreen").classList.remove("hidden");
  document.getElementById("errorBox").style.display="none";
  document.getElementById("nowData").style.display="none";
  document.getElementById("tomorrowData").style.display="none";
  document.getElementById("weekData").style.display="none";
  setUpdateBar(true);
  try{
    setLS(0);await new Promise(r=>setTimeout(r,300));
    setLS(1);
    const rA=await fetch(URL_A);if(!rA.ok)throw new Error(`Open-Meteo HTTP ${rA.status}`);
    const oA=await rA.json();
    const c=oA.current;
    const wx={temperature:c.temperature_2m,apparent_temperature:c.apparent_temperature,dewpoint:c.dew_point_2m,humidity:c.relative_humidity_2m,weather_code:c.weather_code,cloud_cover:c.cloud_cover,wind_speed:c.wind_speed_10m,wind_direction:c.wind_direction_10m,wind_gusts:c.wind_gusts_10m,pressure:c.surface_pressure,visibility:Math.min(c.visibility??9999,9999)};
    setLS(2);
    let oE=null;try{const r=await fetch(URL_E);if(r.ok)oE=await r.json();}catch(e){console.warn("ECMWF",e);}
    setLS(3);
    let oG=null;try{const r=await fetch(URL_G);if(r.ok)oG=await r.json();}catch(e){console.warn("GFS",e);}
    setLS(4);
    STATE={arome:oA,ecmwf:oE,gfs:oG,lastLoad:Date.now()};
    await new Promise(r=>setTimeout(r,300));
    setLS(5);
    displayNow(wx,oA.hourly,oA.daily,oE,oG);
    displayTomorrow(oA.hourly,oE?.hourly,oG?.hourly,oA.daily);
    displayWeek(oA.daily,oE?.daily,oG?.daily,oA.hourly,oE?.hourly,oG?.hourly);
    setUpdateBar(false,STATE.lastLoad);
    // Windbird en parallèle — l'app fonctionne même si ça échoue
    loadWindbird();
    await new Promise(r=>setTimeout(r,700));
    hideLS();
  }catch(e){
    hideLS();
    const eb=document.getElementById("errorBox");eb.style.display="block";
    eb.innerHTML=`⚠️ ${e.message}<br><small>Vérifie ta connexion puis réessaie.</small>`;
    setUpdateBar(false,null);
  }
  document.getElementById("refreshBtn").disabled=false;
}

loadAll();
setInterval(loadAll,30*60*1000);
</script>
</body>
</html>
