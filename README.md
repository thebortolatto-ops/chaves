[chaves (1).html](https://github.com/user-attachments/files/25936464/chaves.1.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Controle de Chaves — Manutenção</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0a0e1a;--surface:#111827;--card:#151e30;--border:#1e2d45;
  --accent:#00d4ff;--accent2:#ff6b35;--green:#00e676;
  --yellow:#ffd600;--red:#ff3d57;--text:#e8f0fe;--muted:#4a6080;
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{background:var(--bg);color:var(--text);font-family:'Syne',sans-serif;min-height:100vh}

.view{display:none!important}
.view.active{display:block!important}
#viewMobile{display:flex!important;flex-direction:column;align-items:center;justify-content:center;min-height:100vh;padding:1.5rem}
#viewMobile.active{display:flex!important}

/* LOADING */
#loadingScreen{position:fixed;inset:0;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:999;gap:1rem}
#loadingScreen.hidden{display:none}
.spinner{width:40px;height:40px;border:3px solid var(--border);border-top-color:var(--accent);border-radius:50%;animation:spin .8s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
#loadingScreen p{color:var(--muted);font-size:.85rem;font-family:'Space Mono',monospace}

/* HEADER */
header{background:var(--surface);border-bottom:1px solid var(--border);padding:1rem 1.5rem;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;flex-wrap:wrap;gap:.8rem}
.logo{display:flex;align-items:center;gap:.8rem}
.logo-icon{width:36px;height:36px;background:var(--accent);border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:1.1rem;flex-shrink:0}
.logo h1{font-size:1rem;font-weight:800}
.logo span{font-size:.7rem;color:var(--muted);font-family:'Space Mono',monospace}
.header-stats{display:flex;gap:.8rem;flex-wrap:wrap}
.stat-pill{background:var(--card);border:1px solid var(--border);border-radius:20px;padding:.35rem .9rem;font-size:.78rem;display:flex;align-items:center;gap:.5rem}
.dot{width:7px;height:7px;border-radius:50%}
.dot-green{background:var(--green);box-shadow:0 0 5px var(--green)}
.dot-red{background:var(--red);box-shadow:0 0 5px var(--red)}
.dot-yellow{background:var(--yellow);box-shadow:0 0 5px var(--yellow)}
.sync-indicator{font-size:.7rem;font-family:'Space Mono',monospace;color:var(--green);display:flex;align-items:center;gap:.4rem}
.sync-dot{width:6px;height:6px;border-radius:50%;background:var(--green);animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}

main{padding:1.5rem;max-width:1400px;margin:0 auto}
.toolbar{display:flex;gap:.6rem;margin-bottom:1.2rem;align-items:center;flex-wrap:wrap}
.search-box{background:var(--card);border:1px solid var(--border);border-radius:8px;padding:.55rem .9rem;color:var(--text);font-family:'Syne',sans-serif;font-size:.88rem;width:220px;outline:none;transition:border-color .2s}
.search-box:focus{border-color:var(--accent)}
.search-box::placeholder{color:var(--muted)}
.tb-btn{background:var(--card);border:1px solid var(--border);border-radius:8px;padding:.55rem .9rem;color:var(--muted);font-family:'Syne',sans-serif;font-size:.8rem;cursor:pointer;transition:all .2s;white-space:nowrap}
.tb-btn:hover,.tb-btn.active{border-color:var(--accent);color:var(--accent);background:rgba(0,212,255,.05)}
.tb-btn.orange:hover{border-color:var(--accent2);color:var(--accent2)}
.add-btn{background:var(--accent);color:var(--bg);border:none;border-radius:8px;padding:.55rem 1.2rem;font-family:'Syne',sans-serif;font-weight:700;font-size:.82rem;cursor:pointer;white-space:nowrap;margin-left:auto}

.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:.9rem}
.key-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:1.1rem;cursor:pointer;transition:all .2s;position:relative;overflow:hidden}
.key-card::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;border-radius:12px 12px 0 0}
.key-card.disponivel::before{background:var(--green)}
.key-card.retirada::before{background:var(--red)}
.key-card.alerta::before{background:var(--yellow);animation:blink 1.5s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.key-card:hover{border-color:var(--accent);transform:translateY(-2px);box-shadow:0 6px 20px rgba(0,0,0,.3)}
.key-number{font-family:'Space Mono',monospace;font-size:.68rem;color:var(--muted);margin-bottom:.25rem}
.key-name{font-size:.9rem;font-weight:700;margin-bottom:.7rem;line-height:1.3}
.key-status{display:inline-flex;align-items:center;gap:.35rem;font-size:.7rem;font-family:'Space Mono',monospace;padding:.2rem .55rem;border-radius:20px;font-weight:700}
.key-status.disponivel{background:rgba(0,230,118,.12);color:var(--green)}
.key-status.retirada{background:rgba(255,61,87,.12);color:var(--red)}
.key-status.alerta{background:rgba(255,214,0,.12);color:var(--yellow)}
.key-info{margin-top:.7rem;font-size:.76rem;color:var(--muted);line-height:1.5}
.key-info strong{color:var(--text)}
.key-time{margin-top:.25rem;font-family:'Space Mono',monospace;font-size:.68rem;color:var(--accent2)}

/* MODALS */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.75);backdrop-filter:blur(4px);z-index:200;align-items:center;justify-content:center;padding:1rem}
.overlay.open{display:flex}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:1.8rem;width:440px;max-width:100%;position:relative;max-height:92vh;overflow-y:auto}
.modal h2{font-size:1.1rem;font-weight:800;margin-bottom:1.3rem}
.mcls{position:absolute;top:1rem;right:1rem;background:none;border:none;color:var(--muted);font-size:1.2rem;cursor:pointer}
.fg{margin-bottom:.9rem}
.fg label{display:block;font-size:.75rem;color:var(--muted);margin-bottom:.35rem;font-family:'Space Mono',monospace}
.fg input,.fg select{width:100%;background:var(--card);border:1px solid var(--border);border-radius:8px;padding:.65rem .9rem;color:var(--text);font-family:'Syne',sans-serif;font-size:.92rem;outline:none;transition:border-color .2s;appearance:none}
.fg input:focus,.fg select:focus{border-color:var(--accent)}
.ma{display:flex;gap:.7rem;margin-top:1.3rem;flex-wrap:wrap}
.btn-ok{flex:1;background:var(--accent);color:var(--bg);border:none;border-radius:8px;padding:.75rem;font-family:'Syne',sans-serif;font-weight:700;font-size:.88rem;cursor:pointer;min-width:80px}
.btn-dev{flex:1;background:var(--green);color:var(--bg);border:none;border-radius:8px;padding:.75rem;font-family:'Syne',sans-serif;font-weight:700;font-size:.88rem;cursor:pointer;min-width:80px}
.btn-no{background:var(--card);border:1px solid var(--border);border-radius:8px;padding:.75rem 1rem;color:var(--muted);font-family:'Syne',sans-serif;font-size:.88rem;cursor:pointer}
.btn-danger{background:none;border:1px solid rgba(255,61,87,.35);border-radius:8px;padding:.75rem 1rem;color:var(--red);font-family:'Syne',sans-serif;font-size:.88rem;cursor:pointer}

/* LOG */
.log-sec{margin-top:1.8rem;background:var(--card);border:1px solid var(--border);border-radius:12px;overflow:hidden}
.log-hdr{padding:.9rem 1.3rem;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center}
.log-hdr h3{font-size:.88rem;font-weight:700}
.log-tbl{width:100%;border-collapse:collapse}
.log-tbl th{text-align:left;padding:.55rem 1.3rem;font-size:.68rem;color:var(--muted);font-family:'Space Mono',monospace;border-bottom:1px solid var(--border)}
.log-tbl td{padding:.65rem 1.3rem;font-size:.8rem;border-bottom:1px solid rgba(30,45,69,.5)}
.log-tbl tr:last-child td{border-bottom:none}
.abadge{display:inline-block;padding:.12rem .55rem;border-radius:4px;font-size:.68rem;font-weight:700;font-family:'Space Mono',monospace}
.ab-ret{background:rgba(255,61,87,.15);color:var(--red)}
.ab-dev{background:rgba(0,230,118,.15);color:var(--green)}

/* QR */
.qr-print-wrap{background:white;border-radius:12px;padding:1.5rem;text-align:center;margin-bottom:1rem}
.qr-print-wrap canvas,.qr-print-wrap img{max-width:100%!important}

/* MOBILE */
.m-logo{text-align:center;margin-bottom:1.8rem}
.m-logo-icon{font-size:2.2rem;display:block;margin-bottom:.4rem}
.m-logo h1{font-size:1.15rem;font-weight:800}
.m-logo span{font-size:.68rem;color:var(--muted);font-family:'Space Mono',monospace}
.m-card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:1.4rem;width:100%;max-width:400px}
.m-card h2{font-size:.95rem;font-weight:700;margin-bottom:1.2rem}
.mfg{margin-bottom:.9rem}
.mfg label{display:block;font-size:.7rem;color:var(--muted);margin-bottom:.35rem;font-family:'Space Mono',monospace;letter-spacing:.04em}
.mfg input,.mfg select{width:100%;background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:.8rem .9rem;color:var(--text);font-family:'Syne',sans-serif;font-size:.95rem;outline:none;transition:border-color .2s;appearance:none;-webkit-appearance:none}
.mfg input:focus,.mfg select:focus{border-color:var(--accent)}
.kprev{display:none;background:var(--surface);border-radius:10px;padding:.85rem .9rem;margin-bottom:.9rem;border-left:3px solid var(--accent)}
.kprev.show{display:block}
.kprev-name{font-weight:700;font-size:.95rem}
.kprev-st{font-size:.72rem;margin-top:.25rem;font-family:'Space Mono',monospace}
.kprev-st.disponivel{color:var(--green)}
.kprev-st.retirada{color:var(--red)}
.kprev-st.alerta{color:var(--yellow)}
.kprev-user{font-size:.78rem;color:var(--muted);margin-top:.2rem}
.mdiv{border:none;border-top:1px solid var(--border);margin:.9rem 0}
.mbtn{width:100%;border:none;border-radius:12px;padding:.95rem;font-family:'Syne',sans-serif;font-weight:800;font-size:.95rem;cursor:pointer;margin-bottom:.6rem;transition:opacity .2s,transform .1s}
.mbtn:active{transform:scale(.97);opacity:.85}
.mbtn:disabled{opacity:.3;cursor:default;transform:none}
.mbtn-take{background:var(--red);color:#fff}
.mbtn-return{background:var(--green);color:#0a0e1a}
.msuccess{display:none;background:var(--card);border-radius:16px;padding:2rem;width:100%;max-width:400px;text-align:center}
.msuccess.show{display:block}
.msuccess .si{font-size:2.8rem;margin-bottom:.9rem}
.msuccess h2{font-size:1.1rem;font-weight:800;margin-bottom:.4rem}
.msuccess p{color:var(--muted);font-size:.82rem;line-height:1.6;white-space:pre-line}
.mbtnback{margin-top:1.3rem;background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:.75rem;color:var(--text);font-family:'Syne',sans-serif;width:100%;cursor:pointer;font-size:.88rem}

@media(max-width:640px){
  header{padding:.8rem 1rem}
  main{padding:.9rem}
  .grid{grid-template-columns:1fr 1fr}
  .log-tbl th,.log-tbl td{padding:.45rem .7rem;font-size:.7rem}
  .search-box{width:100%}
}
</style>
</head>
<body>

<!-- Loading -->
<div id="loadingScreen">
  <div class="spinner"></div>
  <p>Conectando ao banco de dados...</p>
</div>

<!-- ========== PAINEL ========== -->
<div class="view" id="viewPainel">
<header>
  <div class="logo">
    <div class="logo-icon">🔑</div>
    <div>
      <h1>Sistema de Chaves</h1>
      <span>MANUTENÇÃO HOSPITALAR</span>
    </div>
  </div>
  <div class="header-stats">
    <div class="stat-pill"><div class="dot dot-green"></div><span id="cDisp">—</span></div>
    <div class="stat-pill"><div class="dot dot-red"></div><span id="cRet">—</span></div>
    <div class="stat-pill"><div class="dot dot-yellow"></div><span id="cAlert">—</span></div>
    <div class="sync-indicator"><div class="sync-dot"></div>Tempo real</div>
  </div>
</header>

<main>
  <div class="toolbar">
    <input class="search-box" type="text" placeholder="🔍  Buscar..." id="searchInput" oninput="renderKeys()">
    <button class="tb-btn active" onclick="setFilter('todos',this)">Todas</button>
    <button class="tb-btn" onclick="setFilter('disponivel',this)">Disponíveis</button>
    <button class="tb-btn" onclick="setFilter('retirada',this)">Retiradas</button>
    <button class="tb-btn" onclick="setFilter('alerta',this)">⚠️ Alertas</button>
    <button class="tb-btn orange" onclick="openQRModal()">📱 QR Code</button>
    <button class="add-btn" onclick="openAddModal()">+ Nova Chave</button>
  </div>
  <div class="grid" id="keyGrid"></div>

  <div class="log-sec">
    <div class="log-hdr">
      <h3>📋 Histórico em tempo real</h3>
    </div>
    <table class="log-tbl">
      <thead><tr>
        <th>DATA/HORA</th><th>CHAVE</th><th>FUNCIONÁRIO</th><th>AÇÃO</th><th>MAT.</th>
      </tr></thead>
      <tbody id="logBody"></tbody>
    </table>
  </div>
</main>

<!-- Modal ação -->
<div class="overlay" id="moAction">
  <div class="modal">
    <button class="mcls" onclick="closeMo('moAction')">✕</button>
    <h2 id="moActionTitle"></h2>
    <div id="moActionBody"></div>
  </div>
</div>

<!-- Modal nova chave -->
<div class="overlay" id="moAdd">
  <div class="modal">
    <button class="mcls" onclick="closeMo('moAdd')">✕</button>
    <h2>Nova Chave</h2>
    <div class="fg"><label>NOME DO SETOR / LOCAL</label>
      <input type="text" id="newName" placeholder="Ex: UTI Adulto, Farmácia..."></div>
    <div class="fg"><label>NÚMERO DA CHAVE</label>
      <input type="text" id="newNum" placeholder="Ex: 01, A-23..."></div>
    <div class="ma">
      <button class="btn-no" onclick="closeMo('moAdd')">Cancelar</button>
      <button class="btn-ok" onclick="addKey()">Adicionar</button>
    </div>
  </div>
</div>

<!-- Modal editar -->
<div class="overlay" id="moEdit">
  <div class="modal">
    <button class="mcls" onclick="closeMo('moEdit')">✕</button>
    <h2>✏️ Editar Chave</h2>
    <div class="fg"><label>NÚMERO DA CHAVE</label>
      <input type="text" id="editNum"></div>
    <div class="fg"><label>NOME DO SETOR / LOCAL</label>
      <input type="text" id="editName"></div>
    <div class="ma">
      <button class="btn-danger" onclick="deleteKey()">🗑 Excluir</button>
      <button class="btn-no" onclick="closeMo('moEdit')">Cancelar</button>
      <button class="btn-ok" onclick="saveEdit()">Salvar</button>
    </div>
  </div>
</div>

<!-- Modal QR -->
<div class="overlay" id="moQR">
  <div class="modal" style="text-align:center;max-width:360px">
    <button class="mcls" onclick="closeMo('moQR')">✕</button>
    <h2>📱 QR Code</h2>
    <p style="font-size:.8rem;color:var(--muted);margin-bottom:1.2rem">Imprima, plastifique e cole perto do painel de chaves.</p>
    <div class="qr-print-wrap">
      <div id="qrCanvas" style="display:inline-block"></div>
      <div style="margin-top:.8rem;font-size:.72rem;font-weight:700;color:#222;font-family:'Space Mono',monospace">MANUTENÇÃO — CONTROLE DE CHAVES</div>
      <div style="font-size:.62rem;color:#666;margin-top:.2rem">Escaneie para registrar retirada ou devolução</div>
    </div>
    <div style="display:flex;gap:.7rem;justify-content:center">
      <button class="btn-ok" onclick="printQR()" style="flex:none;padding:.7rem 1.5rem">🖨️ Imprimir</button>
      <button class="btn-no" onclick="closeMo('moQR')">Fechar</button>
    </div>
  </div>
</div>
</div><!-- /painel -->

<!-- ========== MOBILE ========== -->
<div class="view" id="viewMobile">
  <div class="m-logo">
    <span class="m-logo-icon">🔑</span>
    <h1>Controle de Chaves</h1>
    <span>MANUTENÇÃO HOSPITALAR</span>
  </div>
  <div class="m-card" id="mCard">
    <h2>Registrar movimentação</h2>
    <div id="mForm"></div>
  </div>
  <div class="msuccess" id="mSuccess">
    <div class="si" id="mSIcon"></div>
    <h2 id="mSTitle"></h2>
    <p id="mSMsg"></p>
    <button class="mbtnback" onclick="mReset()">↩ Nova movimentação</button>
  </div>
</div>

<!-- Firebase -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.0/firebase-app.js";
import { getFirestore, collection, doc, onSnapshot, setDoc, addDoc, deleteDoc, query, orderBy, limit, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyBhTAv3SN5Fn71i-MmzrkxMDqVMfiiS6ss",
  authDomain: "chaveseletrica.firebaseapp.com",
  projectId: "chaveseletrica",
  storageBucket: "chaveseletrica.firebasestorage.app",
  messagingSenderId: "453163414814",
  appId: "1:453163414814:web:48edb83cc258fd3a37c50f"
};

const app = initializeApp(firebaseConfig);
const db  = getFirestore(app);

// ── state ──
let keys   = [];
let logs   = [];
let filter = 'todos';
let editId = null;

// ── routing ──
function isMobile(){return /Android|iPhone|iPad|iPod/i.test(navigator.userAgent)||window.innerWidth<768;}
function route(){
  document.getElementById('loadingScreen').classList.add('hidden');
  if(location.hash==='#mobile'||isMobile()){
    document.getElementById('viewPainel').classList.remove('active');
    document.getElementById('viewMobile').classList.add('active');
    mRender();
  } else {
    document.getElementById('viewMobile').classList.remove('active');
    document.getElementById('viewPainel').classList.add('active');
  }
}
window.addEventListener('hashchange',route);

// ── realtime listeners ──
onSnapshot(collection(db,'keys'), snap=>{
  keys=snap.docs.map(d=>({docId:d.id,...d.data()}));
  if(document.getElementById('viewPainel').classList.contains('active')) renderKeys();
  if(document.getElementById('viewMobile').classList.contains('active')) mRender();
  route();
});

onSnapshot(query(collection(db,'log'),orderBy('time','desc'),limit(40)), snap=>{
  logs=snap.docs.map(d=>d.data());
  if(document.getElementById('viewPainel').classList.contains('active')) renderLog();
});

// ── helpers ──
function getStatus(k){
  if(k.status!=='retirada') return 'disponivel';
  return (Date.now()-k.since?.toMillis?.())/3600000>8?'alerta':'retirada';
}
function fmt(ts){
  if(!ts) return '';
  const m=Math.floor((Date.now()-(ts.toMillis?.()??ts))/60000);
  if(m<60) return `há ${m}min`;
  if(m<1440) return `há ${Math.floor(m/60)}h${m%60?` ${m%60}min`:''}`;
  return `há ${Math.floor(m/1440)} dia(s)`;
}

// ── painel render ──
window.renderKeys=function(){
  const q=(document.getElementById('searchInput')||{value:''}).value.toLowerCase();
  const grid=document.getElementById('keyGrid');
  const list=keys.filter(k=>{
    const ok=k.name?.toLowerCase().includes(q)||k.id?.includes(q);
    const st=getStatus(k);
    return ok&&(filter==='todos'||st===filter);
  }).sort((a,b)=>a.id?.localeCompare(b.id,'pt',{numeric:true}));

  if(!list.length){grid.innerHTML=`<div style="grid-column:1/-1;text-align:center;padding:3rem;color:var(--muted)">🔍 Nenhuma chave encontrada</div>`;return;}
  grid.innerHTML=list.map(k=>{
    const st=getStatus(k);
    const lbl={disponivel:'● DISPONÍVEL',retirada:'● RETIRADA',alerta:'⚠ ALERTA'}[st];
    return `<div class="key-card ${st}" onclick="openAction('${k.docId}')">
      <div style="display:flex;justify-content:space-between;align-items:flex-start">
        <div class="key-number">CHV-${k.id}</div>
        <button onclick="event.stopPropagation();openEdit('${k.docId}')"
          style="background:none;border:none;color:var(--muted);cursor:pointer;font-size:.82rem;padding:0"
          onmouseover="this.style.color='var(--accent)'" onmouseout="this.style.color='var(--muted)'">✏️</button>
      </div>
      <div class="key-name">${k.name}</div>
      <div class="key-status ${st}">${lbl}</div>
      ${k.status==='retirada'?`<div class="key-info"><strong>${k.user||'—'}</strong><br>Mat: ${k.badge||'—'}</div><div class="key-time">${fmt(k.since)}</div>`:''}
    </div>`;
  }).join('');

  const c={disponivel:0,retirada:0,alerta:0};
  keys.forEach(k=>{const s=getStatus(k);c[s]=(c[s]||0)+1;});
  document.getElementById('cDisp').textContent=`${c.disponivel||0} disponíveis`;
  document.getElementById('cRet').textContent=`${c.retirada||0} retiradas`;
  document.getElementById('cAlert').textContent=`${c.alerta||0} alertas`;
}

window.renderLog=function(){
  const tb=document.getElementById('logBody');
  if(!logs.length){tb.innerHTML=`<tr><td colspan="5" style="text-align:center;color:var(--muted);padding:1.5rem">Sem movimentações</td></tr>`;return;}
  tb.innerHTML=logs.map(e=>`
    <tr>
      <td style="font-family:'Space Mono',monospace;font-size:.7rem">${e.time?.toDate?.().toLocaleString('pt-BR')||'—'}</td>
      <td><strong>${e.keyName}</strong> <span style="color:var(--muted);font-size:.7rem">CHV-${e.keyId}</span></td>
      <td>${e.user||'—'}</td>
      <td><span class="abadge ${e.action==='RETIRADA'?'ab-ret':'ab-dev'}">${e.action}</span></td>
      <td style="font-family:'Space Mono',monospace;font-size:.7rem">${e.badge||'—'}</td>
    </tr>`).join('');
}

window.setFilter=function(f,btn){
  filter=f;
  document.querySelectorAll('.tb-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  renderKeys();
}

// ── painel actions ──
window.openAction=function(docId){
  const k=keys.find(x=>x.docId===docId);
  const st=getStatus(k);
  editId=docId;
  document.getElementById('moActionTitle').textContent=`🔑 ${k.name} — CHV-${k.id}`;
  let body='';
  if(st==='disponivel'){
    body=`<div class="fg"><label>SEU NOME COMPLETO</label><input type="text" id="aUser" placeholder="Ex: João Silva"></div>
    <div class="fg"><label>MATRÍCULA</label><input type="text" id="aBadge" placeholder="Ex: 1234"></div>
    <div class="ma"><button class="btn-no" onclick="closeMo('moAction')">Cancelar</button>
    <button class="btn-ok" onclick="takeKey()">✋ Retirar</button></div>`;
  } else {
    body=`<div style="background:var(--card);border-radius:8px;padding:.9rem;margin-bottom:.9rem">
      <div style="font-size:.75rem;color:var(--muted)">RETIRADA POR</div>
      <div style="font-weight:700;margin-top:.2rem">${k.user}</div>
      <div style="font-size:.78rem;color:var(--muted);margin-top:.4rem">Mat: ${k.badge||'—'} · ${fmt(k.since)}</div>
    </div>
    <div class="fg"><label>QUEM ESTÁ DEVOLVENDO</label><input type="text" id="aUser" value="${k.user||''}"></div>
    <div class="ma"><button class="btn-no" onclick="closeMo('moAction')">Cancelar</button>
    <button class="btn-dev" onclick="returnKey()">✅ Devolver</button></div>`;
  }
  document.getElementById('moActionBody').innerHTML=body;
  document.getElementById('moAction').classList.add('open');
}

window.takeKey=async function(){
  const user=document.getElementById('aUser').value.trim();
  const badge=document.getElementById('aBadge').value.trim();
  if(!user){alert('Informe seu nome.');return;}
  const k=keys.find(x=>x.docId===editId);
  await setDoc(doc(db,'keys',editId),{...k,status:'retirada',user,badge,since:serverTimestamp()});
  await addDoc(collection(db,'log'),{time:serverTimestamp(),keyId:k.id,keyName:k.name,user,badge,action:'RETIRADA'});
  closeMo('moAction');toast(`✅ "${k.name}" registrada para ${user}`);
}

window.returnKey=async function(){
  const user=document.getElementById('aUser').value.trim();
  const k=keys.find(x=>x.docId===editId);
  await addDoc(collection(db,'log'),{time:serverTimestamp(),keyId:k.id,keyName:k.name,user:user||k.user,badge:k.badge,action:'DEVOLVIDA'});
  await setDoc(doc(db,'keys',editId),{...k,status:'disponivel',user:null,badge:null,since:null});
  closeMo('moAction');toast(`🔑 "${k.name}" devolvida!`);
}

window.openAddModal=function(){document.getElementById('moAdd').classList.add('open');}
window.addKey=async function(){
  const name=document.getElementById('newName').value.trim();
  const num=document.getElementById('newNum').value.trim();
  if(!name){alert('Informe o nome do setor.');return;}
  const id=num||String(Date.now()).slice(-4);
  await addDoc(collection(db,'keys'),{id,name,status:'disponivel',user:null,badge:null,since:null});
  closeMo('moAdd');
  document.getElementById('newName').value='';document.getElementById('newNum').value='';
  toast(`✅ "${name}" adicionada!`);
}

window.openEdit=function(docId){
  editId=docId;
  const k=keys.find(x=>x.docId===docId);
  document.getElementById('editNum').value=k.id;
  document.getElementById('editName').value=k.name;
  document.getElementById('moEdit').classList.add('open');
}
window.saveEdit=async function(){
  const name=document.getElementById('editName').value.trim();
  const num=document.getElementById('editNum').value.trim();
  if(!name){alert('Informe o nome.');return;}
  const k=keys.find(x=>x.docId===editId);
  await setDoc(doc(db,'keys',editId),{...k,name,id:num||k.id});
  closeMo('moEdit');toast(`✅ Atualizado para "${name}"`);
}
window.deleteKey=async function(){
  const k=keys.find(x=>x.docId===editId);
  if(!confirm(`Excluir "${k.name}"?`))return;
  await deleteDoc(doc(db,'keys',editId));
  closeMo('moEdit');toast(`🗑 "${k.name}" excluída.`);
}

// ── QR ──
window.openQRModal=function(){
  const c=document.getElementById('qrCanvas');
  c.innerHTML='';
  const url=window.location.href.split('#')[0]+'#mobile';
  new QRCode(c,{text:url,width:210,height:210,correctLevel:QRCode.CorrectLevel.M});
  document.getElementById('moQR').classList.add('open');
}
window.printQR=function(){
  const inner=document.querySelector('.qr-print-wrap').innerHTML;
  const w=window.open('','_blank','width=420,height=550');
  w.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8"><title>QR Code</title>
  <style>body{margin:0;display:flex;align-items:center;justify-content:center;min-height:100vh;background:#fff;font-family:sans-serif}
  .box{text-align:center;padding:2rem;border:2px dashed #ccc;border-radius:12px}</style></head>
  <body><div class="box">${inner}</div>
  <script>window.onload=()=>{window.print();setTimeout(()=>window.close(),500)}<\/script></body></html>`);
  w.document.close();
}

// ── MOBILE ──
window.mRender=function(){
  const el=document.getElementById('mForm');
  if(!keys.length){
    el.innerHTML=`<p style="text-align:center;color:var(--muted);font-size:.85rem;padding:1rem 0">Carregando chaves...</p>`;
    return;
  }
  const sorted=[...keys].sort((a,b)=>a.id?.localeCompare(b.id,'pt',{numeric:true}));
  const opts=sorted.map(k=>{
    const ic=getStatus(k)==='disponivel'?'🟢':'🔴';
    return `<option value="${k.docId}">${ic}  CHV-${k.id} — ${k.name}</option>`;
  }).join('');
  el.innerHTML=`
    <div class="mfg"><label>QUAL CHAVE?</label>
      <select id="mSel" onchange="mOnSel()">
        <option value="">— Selecione —</option>${opts}
      </select>
    </div>
    <div class="kprev" id="mPrev">
      <div class="kprev-name" id="mpName"></div>
      <div class="kprev-st"   id="mpSt"></div>
      <div class="kprev-user" id="mpUser"></div>
    </div>
    <div class="mfg"><label>SEU NOME COMPLETO</label>
      <input type="text" id="mUser" placeholder="Ex: João da Silva" autocomplete="name">
    </div>
    <div class="mfg" id="mBadgeRow"><label>MATRÍCULA (opcional)</label>
      <input type="text" id="mBadge" placeholder="Ex: 1234" inputmode="numeric">
    </div>
    <hr class="mdiv">
    <button class="mbtn mbtn-take"   id="mBtnT" onclick="mTake()"   disabled>✋ RETIRAR CHAVE</button>
    <button class="mbtn mbtn-return" id="mBtnR" onclick="mReturn()" style="display:none">✅ DEVOLVER CHAVE</button>
  `;
}

window.mOnSel=function(){
  const docId=document.getElementById('mSel').value;
  const prev=document.getElementById('mPrev');
  const bT=document.getElementById('mBtnT');
  const bR=document.getElementById('mBtnR');
  const br=document.getElementById('mBadgeRow');
  if(!docId){prev.classList.remove('show');bT.disabled=true;bT.style.display='block';bR.style.display='none';br.style.display='block';return;}
  const k=keys.find(x=>x.docId===docId);
  const st=getStatus(k);
  document.getElementById('mpName').textContent=`${k.name}  ·  CHV-${k.id}`;
  const se=document.getElementById('mpSt');
  se.textContent={disponivel:'● DISPONÍVEL',retirada:'● RETIRADA',alerta:'⚠ RETIRADA HÁ MUITO TEMPO'}[st];
  se.className=`kprev-st ${st}`;
  document.getElementById('mpUser').textContent=st!=='disponivel'?`Retirada por: ${k.user||'—'} (${fmt(k.since)})`:'';
  if(st==='disponivel'){bT.style.display='block';bT.disabled=false;bR.style.display='none';br.style.display='block';}
  else{bR.style.display='block';bT.style.display='none';br.style.display='none';}
  prev.classList.add('show');
}

window.mTake=async function(){
  const docId=document.getElementById('mSel').value;
  const user=document.getElementById('mUser').value.trim();
  const badge=document.getElementById('mBadge').value.trim();
  if(!docId){alert('Selecione a chave.');return;}
  if(!user){alert('Por favor, informe seu nome.');return;}
  const k=keys.find(x=>x.docId===docId);
  // Re-check status to avoid race condition
  if(getStatus(k)!=='disponivel'){
    alert(`⚠️ Esta chave já foi retirada por ${k.user}! Atualize a lista.`);
    mRender();return;
  }
  await setDoc(doc(db,'keys',docId),{...k,status:'retirada',user,badge,since:serverTimestamp()});
  await addDoc(collection(db,'log'),{time:serverTimestamp(),keyId:k.id,keyName:k.name,user,badge,action:'RETIRADA'});
  mShowOk('✋','Retirada registrada!',`Chave "${k.name}" registrada para ${user}.\n\nLembre-se de devolver após o uso!`);
}

window.mReturn=async function(){
  const docId=document.getElementById('mSel').value;
  const user=document.getElementById('mUser').value.trim();
  const k=keys.find(x=>x.docId===docId);
  await addDoc(collection(db,'log'),{time:serverTimestamp(),keyId:k.id,keyName:k.name,user:user||k.user,badge:k.badge,action:'DEVOLVIDA'});
  await setDoc(doc(db,'keys',docId),{...k,status:'disponivel',user:null,badge:null,since:null});
  mShowOk('✅','Devolução registrada!',`Chave "${k.name}" disponível novamente.\n\nObrigado!`);
}

window.mShowOk=function(icon,title,msg){
  document.getElementById('mCard').style.display='none';
  document.getElementById('mSIcon').textContent=icon;
  document.getElementById('mSTitle').textContent=title;
  document.getElementById('mSMsg').textContent=msg;
  document.getElementById('mSuccess').classList.add('show');
}
window.mReset=function(){
  document.getElementById('mSuccess').classList.remove('show');
  document.getElementById('mCard').style.display='block';
  mRender();
}

// ── utils ──
window.closeMo=function(id){document.getElementById(id).classList.remove('open');}
window.toast=function(msg){
  const t=document.createElement('div');
  t.style.cssText='position:fixed;bottom:1.5rem;right:1.5rem;background:#1e2d45;border:1px solid #2a3d5a;border-radius:10px;padding:.7rem 1.2rem;font-size:.85rem;z-index:999;box-shadow:0 4px 20px rgba(0,0,0,.5);max-width:280px';
  t.textContent=msg;document.body.appendChild(t);setTimeout(()=>t.remove(),3500);
}

// Init defaults if DB is empty
setTimeout(async()=>{
  if(keys.length===0){
    const DEFAULT_KEYS=["UTI Adulto","UTI Pediátrica","Centro Cirúrgico","CME","Farmácia Central","Almoxarifado","Sala de Caldeiras","Casa de Máquinas","Subestação Elétrica","Gerador 1","Gerador 2","CCIH","Arquivo Médico","Sala de TI","Radiologia","Laboratório","Banco de Sangue","Maternidade","PA Adulto","PA Pediátrico","Pronto Socorro","SAMU Base","Portaria Principal","Portaria Secundária","Vestiário Masc.","Vestiário Fem.","Sala de Reuniões","Diretoria","RH","Faturamento","Copa da UTI","Copa Cirúrgico","DML 1º Andar","DML 2º Andar","DML 3º Andar","Depósito de O₂","Central de Ar","Compressor 1","Compressor 2","Sala de Bombas","Elevador Serviço","Elevador Social","Câmara Fria","Lactário","Nutrição","Lavanderia","Expurgo","Capela","Administração"];
    for(let i=0;i<DEFAULT_KEYS.length;i++){
      await addDoc(collection(db,'keys'),{id:String(i+1).padStart(2,'0'),name:DEFAULT_KEYS[i],status:'disponivel',user:null,badge:null,since:null});
    }
  }
},3000);

setInterval(()=>{if(document.getElementById('viewPainel').classList.contains('active'))renderKeys();},60000);
</script>

<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
</body>
</html>
