# Ghhj
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>lutpi13 — Affiliate Mastermind</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet"/>
<style>
/* ═══════════════════════════════════════════
   RESET & ROOT VARIABLES
═══════════════════════════════════════════ */
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root{
  --g:    #00ff88;
  --c:    #00e5ff;
  --warn: #ffb300;
  --red:  #ff4444;
  --dark: #090d0b;
  --d1:   #0d1210;
  --d2:   #111815;
  --glass: rgba(0,255,136,0.05);
  --glass2:rgba(0,229,255,0.05);
  --border:rgba(0,255,136,0.15);
  --border2:rgba(0,229,255,0.18);
  --blur: blur(20px);
  --mono: 'Share Tech Mono',monospace;
  --orb:  'Orbitron',sans-serif;
  --radius:16px;
  --sidebar:280px;
}

html{scroll-behavior:smooth}
body{
  background:var(--dark);
  color:var(--g);
  font-family:var(--mono);
  min-height:100vh;
  overflow-x:hidden;
}

/* ═══════════════════════════════════════════
   MATRIX CANVAS
═══════════════════════════════════════════ */
#mc{
  position:fixed;top:0;left:0;
  width:100%;height:100%;
  z-index:0;opacity:.08;
  pointer-events:none;
}

/* SCANLINES */
body::before{
  content:'';
  position:fixed;inset:0;
  background:repeating-linear-gradient(
    0deg,transparent,transparent 2px,
    rgba(0,0,0,.04) 2px,rgba(0,0,0,.04) 4px
  );
  pointer-events:none;z-index:9999;
}

/* ═══════════════════════════════════════════
   TOP NAVBAR (GitHub-style)
═══════════════════════════════════════════ */
.topbar{
  position:fixed;top:0;left:0;right:0;
  height:60px;z-index:500;
  display:flex;align-items:center;
  padding:0 1.5rem;gap:1.5rem;
  background:rgba(9,13,11,.75);
  backdrop-filter:var(--blur);
  border-bottom:1px solid var(--border);
}

.topbar-logo{
  font-family:var(--orb);font-size:1.1rem;
  font-weight:900;color:var(--g);
  letter-spacing:2px;text-decoration:none;
  text-shadow:0 0 14px rgba(0,255,136,.5);
  flex-shrink:0;
}
.topbar-logo span{color:var(--c)}

.topbar-search{
  flex:1;max-width:320px;
  background:rgba(0,255,136,.04);
  border:1px solid var(--border);
  border-radius:8px;
  padding:6px 14px;
  color:var(--g);font-family:var(--mono);
  font-size:.78rem;letter-spacing:1px;
  outline:none;
  transition:border-color .3s,box-shadow .3s;
}
.topbar-search:focus{
  border-color:var(--c);
  box-shadow:0 0 0 3px rgba(0,229,255,.1);
}
.topbar-search::placeholder{color:rgba(0,255,136,.35)}

.topbar-nav{
  display:flex;gap:.2rem;list-style:none;
  margin-left:auto;
}
.topbar-nav a{
  padding:6px 12px;border-radius:8px;
  color:rgba(0,255,136,.65);
  text-decoration:none;font-size:.75rem;
  letter-spacing:1.5px;text-transform:uppercase;
  transition:all .25s;
}
.topbar-nav a:hover{
  background:var(--glass);color:var(--c);
  text-shadow:0 0 8px var(--c);
}

.topbar-avatar{
  width:34px;height:34px;border-radius:50%;
  background:linear-gradient(135deg,var(--g),var(--c));
  display:flex;align-items:center;justify-content:center;
  font-family:var(--orb);font-size:.7rem;font-weight:900;
  color:var(--dark);cursor:pointer;flex-shrink:0;
  box-shadow:0 0 12px rgba(0,255,136,.3);
}

/* ═══════════════════════════════════════════
   LAYOUT (GitHub: left sidebar + main + right)
═══════════════════════════════════════════ */
.layout{
  position:relative;z-index:1;
  display:grid;
  grid-template-columns:var(--sidebar) 1fr 260px;
  grid-template-rows:auto;
  min-height:100vh;
  padding-top:60px;
  max-width:1400px;
  margin:0 auto;
  gap:0;
}

/* ─── LEFT SIDEBAR ─── */
.sidebar{
  position:sticky;top:60px;
  height:calc(100vh - 60px);
  overflow-y:auto;
  padding:1.5rem 1rem;
  border-right:1px solid var(--border);
  display:flex;flex-direction:column;gap:.3rem;
  scrollbar-width:thin;
  scrollbar-color:var(--border) transparent;
}
.sidebar::-webkit-scrollbar{width:4px}
.sidebar::-webkit-scrollbar-track{background:transparent}
.sidebar::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}

.sidebar-section-label{
  font-size:.65rem;letter-spacing:3px;
  color:rgba(0,255,136,.35);
  text-transform:uppercase;
  margin:1rem 0 .3rem .5rem;
}

.sidebar-link{
  display:flex;align-items:center;gap:.75rem;
  padding:8px 10px;border-radius:10px;
  color:rgba(0,255,136,.65);
  text-decoration:none;font-size:.78rem;
  letter-spacing:.5px;
  transition:all .25s cubic-bezier(.34,1.56,.64,1);
  cursor:pointer;border:1px solid transparent;
}
.sidebar-link:hover,.sidebar-link.active{
  background:var(--glass);
  border-color:var(--border);
  color:var(--g);
  text-shadow:0 0 8px rgba(0,255,136,.3);
  transform:translateX(4px);
}
.sidebar-link .ico{font-size:1rem;flex-shrink:0}
.sidebar-link .badge{
  margin-left:auto;
  padding:1px 7px;
  background:rgba(0,229,255,.1);
  border:1px solid rgba(0,229,255,.2);
  border-radius:999px;
  font-size:.6rem;color:var(--c);
}

.sidebar-divider{
  height:1px;background:var(--border);
  margin:.5rem 0;opacity:.5;
}

/* ─── MAIN CONTENT ─── */
.main-content{
  padding:2rem 1.5rem;
  display:flex;flex-direction:column;gap:2rem;
  min-width:0;
}

/* ─── RIGHT PANEL ─── */
.right-panel{
  position:sticky;top:60px;
  height:calc(100vh - 60px);
  overflow-y:auto;
  padding:1.5rem 1rem;
  border-left:1px solid var(--border);
  display:flex;flex-direction:column;gap:1rem;
  scrollbar-width:thin;
  scrollbar-color:var(--border) transparent;
}
.right-panel::-webkit-scrollbar{width:4px}
.right-panel::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}

/* ═══════════════════════════════════════════
   GLASS CARD (Android 17 Material 3)
═══════════════════════════════════════════ */
.card{
  background:var(--glass);
  border:1px solid var(--border);
  border-radius:var(--radius);
  backdrop-filter:var(--blur);
  -webkit-backdrop-filter:var(--blur);
  transition:transform .4s cubic-bezier(.34,1.56,.64,1),
             box-shadow .4s ease,
             border-color .3s;
  overflow:hidden;
}
.card:hover{
  transform:translateY(-3px);
  box-shadow:0 8px 32px rgba(0,255,136,.08),
             0 0 0 1px rgba(0,229,255,.15);
  border-color:rgba(0,229,255,.25);
}

.card-header{
  padding:1rem 1.2rem;
  border-bottom:1px solid var(--border);
  display:flex;align-items:center;gap:.75rem;
}

.card-header h2{
  font-family:var(--orb);font-size:.85rem;
  font-weight:700;color:var(--g);
  letter-spacing:2px;
  text-transform:uppercase;
}

.card-header .dot{
  width:8px;height:8px;border-radius:50%;
  background:var(--g);
  box-shadow:0 0 8px var(--g);
  animation:pulse 2s infinite;
}

@keyframes pulse{
  0%,100%{opacity:1;transform:scale(1)}
  50%{opacity:.5;transform:scale(.8)}
}

.card-body{padding:1.2rem}

/* ═══════════════════════════════════════════
   PROFILE HEADER (GitHub repo style)
═══════════════════════════════════════════ */
.profile-header{
  display:flex;align-items:center;gap:1.5rem;
  padding:1.5rem;
  background:var(--glass);
  border:1px solid var(--border);
  border-radius:var(--radius);
  backdrop-filter:var(--blur);
}

.profile-avatar{
  width:80px;height:80px;border-radius:50%;
  background:linear-gradient(135deg,var(--g),var(--c),var(--g));
  display:flex;align-items:center;justify-content:center;
  font-family:var(--orb);font-size:1.5rem;font-weight:900;
  color:var(--dark);flex-shrink:0;
  box-shadow:0 0 24px rgba(0,255,136,.4),0 0 48px rgba(0,255,136,.15);
  animation:avatarGlow 3s ease-in-out infinite;
}
@keyframes avatarGlow{
  0%,100%{box-shadow:0 0 24px rgba(0,255,136,.4),0 0 48px rgba(0,255,136,.15)}
  50%{box-shadow:0 0 32px rgba(0,229,255,.5),0 0 64px rgba(0,229,255,.2)}
}

.profile-info h1{
  font-family:var(--orb);font-size:1.6rem;font-weight:900;
  color:var(--g);text-shadow:0 0 20px rgba(0,255,136,.4);
  letter-spacing:3px;
}
.profile-info h1 span{color:var(--c)}
.profile-info .bio{
  font-size:.78rem;color:rgba(0,255,136,.6);
  margin-top:.4rem;line-height:1.7;letter-spacing:.5px;
}
.profile-info .tags{
  display:flex;flex-wrap:wrap;gap:.5rem;margin-top:.8rem;
}
.tag{
  padding:3px 10px;
  background:rgba(0,229,255,.08);
  border:1px solid rgba(0,229,255,.2);
  border-radius:999px;
  font-size:.65rem;letter-spacing:2px;
  color:var(--c);text-transform:uppercase;
}

/* ═══════════════════════════════════════════
   TAB NAVIGATION (GitHub-style)
═══════════════════════════════════════════ */
.tabs{
  display:flex;gap:0;
  border-bottom:1px solid var(--border);
  margin-bottom:-.5rem;
}
.tab-btn{
  padding:10px 18px;
  background:transparent;border:none;
  color:rgba(0,255,136,.5);
  font-family:var(--mono);font-size:.75rem;
  letter-spacing:1.5px;text-transform:uppercase;
  cursor:pointer;
  border-bottom:2px solid transparent;
  margin-bottom:-1px;
  transition:all .25s;display:flex;align-items:center;gap:.5rem;
}
.tab-btn:hover{color:var(--g)}
.tab-btn.active{
  color:var(--g);
  border-bottom-color:var(--g);
}
.tab-count{
  padding:1px 6px;
  background:rgba(0,255,136,.1);
  border-radius:999px;
  font-size:.6rem;color:var(--g);
}

/* ═══════════════════════════════════════════
   README / CONTENT SECTIONS
═══════════════════════════════════════════ */
.section-title{
  font-family:var(--orb);
  font-size:clamp(1rem,2.5vw,1.3rem);
  font-weight:700;color:var(--g);
  letter-spacing:3px;
  display:flex;align-items:center;gap:.75rem;
  margin-bottom:1rem;
}
.section-title::before{
  content:'#';color:var(--c);font-size:1.2em;
}
.section-title::after{
  content:'';flex:1;height:1px;
  background:linear-gradient(90deg,var(--border),transparent);
}

.text-block{
  font-size:.82rem;line-height:2;
  color:rgba(0,255,136,.7);
  margin-bottom:1rem;
}
.text-block strong{color:var(--g)}
.text-block .hl{color:var(--c)}

/* Terminal Code Block */
.code-block{
  background:rgba(0,0,0,.4);
  border:1px solid var(--border);
  border-radius:12px;
  padding:1rem 1.2rem;
  font-size:.78rem;
  line-height:1.9;
  color:rgba(0,255,136,.85);
  overflow-x:auto;
  position:relative;
}
.code-block .prompt{color:var(--c)}
.code-block .comment{color:rgba(0,255,136,.35)}
.code-block .val{color:var(--warn)}
.code-block .key{color:#c678dd}
.code-block::before{
  content:'● ● ●';
  position:absolute;top:8px;right:12px;
  font-size:.55rem;
  color:rgba(0,255,136,.2);
  letter-spacing:4px;
}

/* ═══════════════════════════════════════════
   INFO GRID / REPO CARDS
═══════════════════════════════════════════ */
.repo-grid{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(240px,1fr));
  gap:1rem;
}

.repo-card{
  background:var(--glass);
  border:1px solid var(--border);
  border-radius:12px;
  backdrop-filter:var(--blur);
  padding:1rem 1.2rem;
  transition:all .35s cubic-bezier(.34,1.56,.64,1);
  cursor:default;
}
.repo-card:hover{
  border-color:rgba(0,229,255,.35);
  transform:translateY(-4px) scale(1.01);
  box-shadow:0 8px 24px rgba(0,229,255,.1);
}
.repo-card-name{
  font-family:var(--orb);font-size:.8rem;
  font-weight:700;color:var(--c);
  letter-spacing:1.5px;margin-bottom:.5rem;
  display:flex;align-items:center;gap:.5rem;
}
.repo-card-desc{
  font-size:.72rem;color:rgba(0,255,136,.55);
  line-height:1.7;margin-bottom:.8rem;
}
.repo-card-meta{
  display:flex;gap:1rem;font-size:.65rem;
  color:rgba(0,255,136,.4);
}
.repo-card-meta .lang-dot{
  width:8px;height:8px;border-radius:50%;
  display:inline-block;margin-right:3px;
}

/* ═══════════════════════════════════════════
   STAT BARS
═══════════════════════════════════════════ */
.stat-row{
  display:flex;align-items:center;
  gap:.75rem;margin-bottom:.6rem;
}
.stat-row .label{
  font-size:.72rem;letter-spacing:1px;
  color:rgba(0,255,136,.65);
  min-width:130px;flex-shrink:0;
}
.stat-row .bar{
  flex:1;height:6px;
  background:rgba(0,255,136,.08);
  border-radius:3px;overflow:hidden;
}
.stat-row .fill{
  height:100%;
