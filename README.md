<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crystal Facilities Management — Attendance</title>
<style>
  :root{
    --deep:#0b2540;
    --teal:#0f7a72;
    --teal-light:#14a89c;
    --ice:#eef6f5;
    --panel:#ffffff;
    --in:#1a9b6c;
    --in-dark:#137a54;
    --out:#c0562f;
    --out-dark:#a1451f;
    --ink:#0b2540;
    --muted:#5c7280;
    --line:#dbe6e6;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    font-family:'Segoe UI',Roboto,Arial,sans-serif;
    color:var(--ink);
    min-height:100vh;
    background:
      radial-gradient(circle at 15% 10%, rgba(20,168,156,0.10), transparent 45%),
      radial-gradient(circle at 90% 85%, rgba(11,37,64,0.08), transparent 40%),
      repeating-linear-gradient(135deg, rgba(15,122,114,0.035) 0px, rgba(15,122,114,0.035) 2px, transparent 2px, transparent 26px),
      var(--ice);
  }
  header{
    background:linear-gradient(120deg,var(--deep),#123a5e 55%,var(--teal));
    color:#fff; padding:22px 28px;
    display:flex; align-items:center; justify-content:space-between;
    box-shadow:0 4px 18px rgba(11,37,64,0.25);
    position:relative; overflow:hidden;
  }
  header::after{
    content:""; position:absolute; right:-40px; top:-40px;
    width:180px; height:180px; border-radius:50%;
    background:radial-gradient(circle,rgba(255,255,255,0.14),transparent 70%);
  }
  .brand{display:flex; align-items:center; gap:14px; position:relative; z-index:1;}
  .crystal-mark{
    width:42px;height:42px;
    background:linear-gradient(135deg,var(--teal-light),#fff);
    clip-path:polygon(50% 0%, 90% 30%, 75% 100%, 25% 100%, 10% 30%);
    box-shadow:0 0 10px rgba(20,168,156,0.7);
    flex-shrink:0;
  }
  .brand-text .company{font-size:20px; font-weight:700; letter-spacing:.3px;}
  .brand-text .sub{font-size:12px; color:#cfe8e4; letter-spacing:.5px; text-transform:uppercase;}
  .app-title{font-size:15px; font-weight:600; opacity:.92; position:relative; z-index:1; text-align:right;}
  .app-title small{display:block; font-weight:400; font-size:12px; color:#d7ece9;}
  main{max-width:560px; margin:0 auto; padding:26px 18px 60px;}
  .card{
    background:var(--panel); border-radius:16px; padding:22px; margin-bottom:18px;
    box-shadow:0 8px 24px rgba(11,37,64,0.08); border:1px solid var(--line);
  }
  label{font-size:12px; font-weight:700; text-transform:uppercase; letter-spacing:.4px; color:var(--muted); display:block; margin-bottom:6px;}
  select, input[type=text]{
    width:100%; padding:11px 12px; font-size:15px;
    border:1.5px solid var(--line); border-radius:10px; background:#fbfdfd; color:var(--ink);
    transition:border-color .15s, box-shadow .15s;
  }
  select:focus, input:focus{outline:none; border-color:var(--teal-light); box-shadow:0 0 0 3px rgba(20,168,156,0.15);}
  .site-row{display:flex; gap:8px;}
  .site-row select{flex:1;}
  .icon-btn{
    border:1.5px solid var(--line); background:#fff; border-radius:10px; width:44px;
    display:flex; align-items:center; justify-content:center; cursor:pointer;
    transition:background .15s, transform .1s;
  }
  .icon-btn:hover{background:var(--ice);}
  .icon-btn:active{transform:scale(0.94);}
  .clock-strip{
    display:flex; justify-content:space-between; align-items:center;
    background:linear-gradient(90deg,var(--deep),var(--teal));
    color:#fff; border-radius:12px; padding:14px 18px; margin-bottom:6px;
  }
  .clock-strip .time{font-size:22px; font-weight:700; font-variant-numeric:tabular-nums;}
  .clock-strip .date{font-size:12px; opacity:.85;}
  .staff-grid{display:grid; grid-template-columns:repeat(2,1fr); gap:10px; margin-top:8px;}
  .staff-btn{
    padding:14px 10px; border-radius:12px; border:2px solid var(--line); background:#fff;
    font-size:14.5px; font-weight:600; color:var(--ink); cursor:pointer; transition:all .15s; position:relative;
  }
  .staff-btn:hover{border-color:var(--teal-light); transform:translateY(-1px);}
  .staff-btn.selected{
    border-color:var(--teal);
    background:linear-gradient(135deg,rgba(20,168,156,0.12),rgba(20,168,156,0.04));
    box-shadow:0 0 0 3px rgba(20,168,156,0.15);
  }
  .staff-btn.selected::after{
    content:"✓"; position:absolute; top:6px; right:8px; color:var(--teal); font-size:13px; font-weight:900;
  }
  .manage-row{display:flex; gap:8px; margin-top:14px;}
  .manage-row input{flex:1;}
  .small-btn{
    border:none; border-radius:10px; padding:11px 14px; font-size:13px; font-weight:700;
    cursor:pointer; color:#fff; background:var(--deep); white-space:nowrap;
    transition:transform .1s, filter .15s;
  }
  .small-btn:hover{filter:brightness(1.1);}
  .small-btn:active{transform:scale(0.96);}
  .staff-remove-list{margin-top:10px; display:flex; flex-wrap:wrap; gap:6px;}
  .chip{
    display:flex; align-items:center; gap:6px;
    background:var(--ice); border:1px solid var(--line); border-radius:999px;
    padding:5px 6px 5px 12px; font-size:12.5px; color:var(--muted);
  }
  .chip button{
    border:none; background:var(--out); color:#fff; border-radius:50%;
    width:18px; height:18px; font-size:11px; cursor:pointer; line-height:1;
    display:flex; align-items:center; justify-content:center;
  }
  .camera-wrap{
    position:relative; border-radius:14px; overflow:hidden; background:#0b1f2f;
    aspect-ratio:4/3; border:2px solid var(--line);
  }
  video, .snap-preview{width:100%; height:100%; object-fit:cover; display:block;}
  .camera-wrap .ring{ position:absolute; inset:0; border-radius:14px; box-shadow:inset 0 0 0 3px rgba(20,168,156,0.35); pointer-events:none; }
  .flash{ position:absolute; inset:0; background:#fff; opacity:0; pointer-events:none; }
  .flash.active{animation:flashpop .35s ease-out;}
  @keyframes flashpop{0%{opacity:.9;} 100%{opacity:0;}}
  .actions{display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-top:16px;}
  .action-btn{
    display:flex; flex-direction:column; align-items:center; gap:6px; padding:18px 10px;
    border:none; border-radius:14px; font-size:16px; font-weight:700; color:#fff; cursor:pointer;
    transition:transform .12s, filter .15s, box-shadow .15s; box-shadow:0 6px 16px rgba(0,0,0,0.12);
  }
  .action-btn svg{width:26px; height:26px;}
  .action-btn.in{background:linear-gradient(135deg,var(--in),var(--in-dark));}
  .action-btn.out{background:linear-gradient(135deg,var(--out),var(--out-dark));}
  .action-btn:hover{filter:brightness(1.07); transform:translateY(-2px); box-shadow:0 10px 20px rgba(0,0,0,0.16);}
  .action-btn:active{transform:scale(0.96);}
  .action-btn:disabled{opacity:.45; cursor:not-allowed; transform:none;}
  .status-banner{
    display:none; margin-top:14px; padding:12px 14px; border-radius:10px;
    font-size:14px; font-weight:600; align-items:center; gap:8px;
  }
  .status-banner.show{display:flex; animation:slidein .25s ease-out;}
  .status-banner.ok{background:rgba(26,155,108,0.12); color:var(--in-dark); border:1px solid rgba(26,155,108,0.3);}
  .status-banner.err{background:rgba(192,86,47,0.12); color:var(--out-dark); border:1px solid rgba(192,86,47,0.3);}
  .status-banner.info{background:rgba(15,122,114,0.1); color:var(--teal); border:1px solid rgba(15,122,114,0.25);}
  @keyframes slidein{from{opacity:0; transform:translateY(-6px);} to{opacity:1; transform:translateY(0);}}
  .log-section h3{font-size:14px; text-transform:uppercase; letter-spacing:.4px; color:var(--muted); margin:0 0 12px;}
  table{width:100%; border-collapse:collapse; font-size:13px;}
  th{text-align:left; color:var(--muted); font-weight:700; padding:6px 8px; border-bottom:2px solid var(--line);}
  td{padding:8px; border-bottom:1px solid var(--line); vertical-align:middle;}
  td.thumb a{color:var(--teal); font-size:12px; text-decoration:none; font-weight:700;}
  .tag{padding:3px 9px; border-radius:999px; font-size:11px; font-weight:700; color:#fff;}
  .tag.in{background:var(--in);}
  .tag.out{background:var(--out);}
  .empty-log{color:var(--muted); font-size:13px; text-align:center; padding:20px 0;}
  .spinner{
    display:inline-block; width:14px; height:14px; border:2px solid rgba(255,255,255,0.5);
    border-top-color:#fff; border-radius:50%; animation:spin .7s linear infinite; margin-right:6px;
  }
  @keyframes spin{to{transform:rotate(360deg);}}
  footer{text-align:center; font-size:11.5px; color:var(--muted); padding:10px 0 30px;}
</style>
</head>
<body>

<header>
  <div class="brand">
    <div class="crystal-mark"></div>
    <div class="brand-text">
      <div class="company">Crystal Facilities Management</div>
      <div class="sub">Crystal Services UK</div>
    </div>
  </div>
  <div class="app-title">Attendance Management App<small>Site clock in / clock out</small></div>
</header>

<main>

  <div class="card">
    <label for="siteSelect">Site</label>
    <div class="site-row">
      <select id="siteSelect"><option>Loading sites…</option></select>
      <button class="icon-btn" title="Add / remove sites" onclick="toggleSiteEditor()">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#0f7a72" stroke-width="2" stroke-linecap="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.1 2.1 0 0 1 3 3L7 19l-4 1 1-4Z"/></svg>
      </button>
    </div>
    <div id="siteEditor" style="display:none; margin-top:12px;">
      <div class="manage-row">
        <input type="text" id="newSiteName" placeholder="New site name">
        <button class="small-btn" onclick="addSite()">Add site</button>
      </div>
      <div class="staff-remove-list" id="siteChipList"></div>
    </div>
  </div>

  <div class="clock-strip">
    <div>
      <div class="time" id="liveTime">--:--:--</div>
      <div class="date" id="liveDate">-- --- ----</div>
    </div>
    <div style="text-align:right;">
      <div class="date">Selected staff</div>
      <div style="font-weight:700; font-size:15px;" id="selectedStaffLabel">None</div>
    </div>
  </div>

  <div class="card">
    <label>Select staff</label>
    <div class="staff-grid" id="staffGrid"><div class="empty-log">Loading…</div></div>

    <div class="manage-row">
      <input type="text" id="newStaffName" placeholder="Add staff name">
      <button class="small-btn" onclick="addStaff()">Add</button>
    </div>
    <div class="staff-remove-list" id="staffChipList"></div>
  </div>

  <div class="card">
    <label>Photo verification</label>
    <div class="camera-wrap" id="cameraWrap">
      <video id="video" autoplay playsinline muted></video>
      <img id="snapPreview" class="snap-preview" style="display:none;">
      <div class="ring"></div>
      <div class="flash" id="flash"></div>
    </div>

    <div class="actions">
      <button class="action-btn in" id="clockInBtn" onclick="handleClock('IN')">
        <svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"/><path d="M10 17l5-5-5-5"/><path d="M15 12H3"/></svg>
        Clock In
      </button>
      <button class="action-btn out" id="clockOutBtn" onclick="handleClock('OUT')">
        <svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><path d="M16 17l5-5-5-5"/><path d="M21 12H9"/></svg>
        Clock Out
      </button>
    </div>

    <div class="status-banner" id="statusBanner"></div>
  </div>

  <div class="card log-section">
    <h3>Today's activity — from Sheet</h3>
    <div id="logTableWrap"><div class="empty-log">Loading…</div></div>
  </div>

</main>

<footer>Crystal Facilities Management &middot; Attendance kiosk &middot; all data stored in Google Sheet, nothing kept on this device</footer>

<script>
const BASE_URL = 'https://script.google.com/macros/s/AKfycbzQl4bmku9MmyK-qqWoVkXednQf6Mtft_303c2eFHJFeHiH9BYdADmXyIDZLVgsQ7OW/exec';

const siteSelect = document.getElementById('siteSelect');
const staffGrid = document.getElementById('staffGrid');
const staffChipList = document.getElementById('staffChipList');
const siteChipList = document.getElementById('siteChipList');
const selectedStaffLabel = document.getElementById('selectedStaffLabel');
const statusBanner = document.getElementById('statusBanner');
const video = document.getElementById('video');
const snapPreview = document.getElementById('snapPreview');
const flash = document.getElementById('flash');
const clockInBtn = document.getElementById('clockInBtn');
const clockOutBtn = document.getElementById('clockOutBtn');

let config = { sites: [], staff: {} };
let selectedStaff = null;
let busy = false;

function tick(){
  const now = new Date();
  document.getElementById('liveTime').textContent = now.toLocaleTimeString('en-GB');
  document.getElementById('liveDate').textContent = now.toLocaleDateString('en-GB', { weekday:'short', day:'2-digit', month:'short', year:'numeric' });
}
tick(); setInterval(tick, 1000);

function showStatus(kind, msg){
  statusBanner.className = 'status-banner show ' + kind;
  statusBanner.innerHTML = msg;
  if(kind !== 'info') setTimeout(()=>{ statusBanner.className='status-banner'; }, 3500);
}

function callGet(action){
  return fetch(`${BASE_URL}?action=${action}`).then(r=>r.json());
}
function callPost(payload){
  return fetch(BASE_URL, {
    method:'POST',
    headers:{'Content-Type':'text/plain'},
    body: JSON.stringify(payload)
  }).then(r=>r.json());
}

function loadConfig(){
  showStatus('info','Loading sites and staff from Sheet…');
  return callGet('config').then(res=>{
    if(res.status !== 'ok') throw new Error(res.message || 'Config load failed');
    config = { sites: res.sites || [], staff: res.staff || {} };
    renderSites();
    statusBanner.className='status-banner';
  }).catch(err=>{
    showStatus('err','Could not load sites/staff from Sheet: '+err.message);
  });
}

function renderSites(){
  const prevSelected = siteSelect.value;
  siteSelect.innerHTML = '';
  if(config.sites.length === 0){
    siteSelect.innerHTML = '<option value="">No sites yet — add one</option>';
  } else {
    config.sites.forEach(s=>{
      const opt = document.createElement('option');
      opt.value = s; opt.textContent = s;
      siteSelect.appendChild(opt);
    });
    if(config.sites.includes(prevSelected)) siteSelect.value = prevSelected;
  }
  renderSiteChips();
  renderStaff();
}
function renderSiteChips(){
  siteChipList.innerHTML = '';
  config.sites.forEach(s=>{
    const chip = document.createElement('div');
    chip.className='chip';
    chip.innerHTML = `${s} <button title="Remove site">&times;</button>`;
    chip.querySelector('button').onclick = ()=>{
      if(!confirm(`Remove site "${s}" and its staff list from the Sheet?`)) return;
      showStatus('info','Removing site…');
      callPost({ action:'removeSite', site:s }).then(()=> loadConfig());
    };
    siteChipList.appendChild(chip);
  });
}
function toggleSiteEditor(){
  const el = document.getElementById('siteEditor');
  el.style.display = el.style.display === 'none' ? 'block' : 'none';
}
function addSite(){
  const input = document.getElementById('newSiteName');
  const name = input.value.trim();
  if(!name) return;
  showStatus('info','Adding site…');
  callPost({ action:'addSite', site:name }).then(res=>{
    if(res.status==='ok'){ input.value=''; loadConfig(); }
    else showStatus('err', res.message || 'Failed to add site');
  });
}

function currentSite(){ return siteSelect.value; }

function renderStaff(){
  const site = currentSite();
  staffGrid.innerHTML = '';
  const list = (site && config.staff[site]) ? config.staff[site] : [];
  if(list.length === 0){
    staffGrid.innerHTML = '<div class="empty-log">No staff added for this site yet.</div>';
  } else {
    list.forEach(name=>{
      const btn = document.createElement('button');
      btn.className = 'staff-btn' + (selectedStaff===name ? ' selected':'');
      btn.textContent = name;
      btn.onclick = ()=>{ selectedStaff = name; selectedStaffLabel.textContent = name; renderStaff(); };
      staffGrid.appendChild(btn);
    });
  }
  renderStaffChips();
}
function renderStaffChips(){
  const site = currentSite();
  staffChipList.innerHTML = '';
  const list = (site && config.staff[site]) ? config.staff[site] : [];
  list.forEach(name=>{
    const chip = document.createElement('div');
    chip.className = 'chip';
    chip.innerHTML = `${name} <button title="Remove">&times;</button>`;
    chip.querySelector('button').onclick = ()=>{
      showStatus('info','Removing staff…');
      callPost({ action:'removeStaff', site, staff:name }).then(()=>{
        if(selectedStaff===name){ selectedStaff=null; selectedStaffLabel.textContent='None'; }
        loadConfig();
      });
    };
    staffChipList.appendChild(chip);
  });
}
function addStaff(){
  const site = currentSite();
  const input = document.getElementById('newStaffName');
  const name = input.value.trim();
  if(!name || !site){ if(!site) showStatus('err','Select or add a site first.'); return; }
  showStatus('info','Adding staff…');
  callPost({ action:'addStaff', site, staff:name }).then(res=>{
    if(res.status==='ok'){ input.value=''; loadConfig(); }
    else showStatus('err', res.message || 'Failed to add staff');
  });
}

siteSelect.addEventListener('change', ()=>{ selectedStaff=null; selectedStaffLabel.textContent='None'; renderStaff(); });

navigator.mediaDevices.getUserMedia({ video: { facingMode:'user' } })
  .then(stream => { video.srcObject = stream; })
  .catch(() => { showStatus('err', 'Camera not available — check browser permissions.'); });

function captureFrame(){
  const canvas = document.createElement('canvas');
  canvas.width = video.videoWidth || 320;
  canvas.height = video.videoHeight || 240;
  canvas.getContext('2d').drawImage(video, 0, 0, canvas.width, canvas.height);
  return canvas.toDataURL('image/jpeg', 0.75);
}

function setBusy(state){
  busy = state;
  clockInBtn.disabled = state;
  clockOutBtn.disabled = state;
}

function handleClock(type){
  if(busy) return;
  const site = currentSite();
  if(!site){ showStatus('err','Select a site first.'); return; }
  if(!selectedStaff){ showStatus('err','Select a staff member first.'); return; }

  flash.classList.remove('active');
  requestAnimationFrame(()=> flash.classList.add('active'));
  const photo = captureFrame();
  snapPreview.src = photo;
  snapPreview.style.display = 'block';
  setTimeout(()=>{ snapPreview.style.display='none'; }, 900);

  const now = new Date();
  const entry = {
    action:'clock', site, staff:selectedStaff, type,
    date: now.toLocaleDateString('en-GB'),
    time: now.toLocaleTimeString('en-GB'),
    ts: now.toISOString(),
    photo
  };

  setBusy(true);
  showStatus('info', `<span class="spinner"></span> Saving to Sheet…`);
  callPost(entry).then(res=>{
    setBusy(false);
    if(res.status === 'ok'){
      showStatus('ok', `${selectedStaff} clocked ${type==='IN'?'in':'out'} at ${site} — ${entry.time}`);
      loadLogs();
    } else {
      showStatus('err', 'Could not save: ' + (res.message || 'unknown error'));
    }
  }).catch(()=>{
    setBusy(false);
    showStatus('err', 'Network error — entry was NOT saved. Please try again.');
  });
}

function loadLogs(){
  callGet('logs').then(res=>{
    const wrap = document.getElementById('logTableWrap');
    if(res.status !== 'ok'){ wrap.innerHTML = '<div class="empty-log">Could not load log.</div>'; return; }
    if(!res.logs || res.logs.length===0){ wrap.innerHTML = '<div class="empty-log">No entries yet today.</div>'; return; }
    const rows = res.logs.map(l=>`
      <tr>
        <td>${l.staff}</td>
        <td>${l.site}</td>
        <td><span class="tag ${l.type==='IN'?'in':'out'}">${l.type}</span></td>
        <td>${l.time}</td>
        <td class="thumb">${l.photoLink ? `<a href="${l.photoLink}" target="_blank">View photo</a>` : ''}</td>
      </tr>`).join('');
    wrap.innerHTML = `<table><thead><tr><th>Staff</th><th>Site</th><th>Action</th><th>Time</th><th>Photo</th></tr></thead><tbody>${rows}</tbody></table>`;
  }).catch(()=>{
    document.getElementById('logTableWrap').innerHTML = '<div class="empty-log">Could not load log.</div>';
  });
}

loadConfig();
loadLogs();
setInterval(loadLogs, 30000);
</script>
</body>
</html>
