[travel-calendar (1).html](https://github.com/user-attachments/files/27622608/travel-calendar.1.html)
# -
개인 저장소<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌸 여행 날짜 맞추기</title>
<link href="https://fonts.googleapis.com/css2?family=Nanum+Myeongjo:wght@400;700;800&family=Gothic+A1:wght@400;700;900&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:        #fff0f5;
    --bg2:       #ffe4ef;
    --surface:   #ffffff;
    --surface2:  #fff7fa;
    --border:    rgba(220,80,130,0.15);
    --border2:   rgba(220,80,130,0.35);
    --pink:      #e8457a;
    --pink-light:#f472a8;
    --pink-pale: #ffd6e7;
    --pink-deep: #c02060;
    --text:      #3a1828;
    --text-dim:  #b07090;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Gothic A1', sans-serif;
    min-height: 100vh;
    padding: 0 0 80px;
    overflow-x: hidden;
  }
  body::before {
    content: '🌸';
    font-size: 180px;
    position: fixed;
    top: -40px; right: -40px;
    opacity: 0.06;
    pointer-events: none;
    z-index: 0;
    transform: rotate(20deg);
  }
  body::after {
    content: '🌸';
    font-size: 140px;
    position: fixed;
    bottom: 40px; left: -30px;
    opacity: 0.05;
    pointer-events: none;
    z-index: 0;
    transform: rotate(-15deg);
  }

  /* HEADER */
  .header {
    background: linear-gradient(135deg, #e8457a 0%, #f472a8 55%, #ff80ab 100%);
    padding: 40px 20px 34px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .header::after {
    content: '';
    position: absolute;
    bottom: -1px; left: 0; right: 0;
    height: 26px;
    background: var(--bg);
    border-radius: 26px 26px 0 0;
  }
  .header h1 {
    font-family: 'Nanum Myeongjo', serif;
    font-size: clamp(26px, 6vw, 44px);
    font-weight: 800;
    color: #fff;
    text-shadow: 0 2px 12px rgba(0,0,0,0.15);
  }
  .header p { color: rgba(255,255,255,0.85); margin-top: 8px; font-size: 14px; }
  .header-petals {
    position: absolute; top: 10px; left: 0; right: 0;
    font-size: 20px; letter-spacing: 10px; opacity: 0.3; pointer-events: none;
  }

  /* LOADING */
  #loading { text-align: center; padding: 60px 20px; color: var(--text-dim); font-size: 15px; }
  .spinner {
    width: 36px; height: 36px;
    border: 3px solid var(--pink-pale);
    border-top-color: var(--pink);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin: 0 auto 16px;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* MAIN */
  #main { display: none; padding: 0 14px; }
  .section-wrap { max-width: 680px; margin: 0 auto; }

  /* CARD */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 22px;
    margin-bottom: 16px;
    box-shadow: 0 4px 24px rgba(220,80,130,0.07);
    position: relative;
    z-index: 1;
  }
  .card-title {
    font-family: 'Nanum Myeongjo', serif;
    font-size: 17px; font-weight: 700;
    color: var(--pink);
    margin-bottom: 16px;
    display: flex; align-items: center; gap: 8px;
  }

  /* MEMBERS */
  .member-slots {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    margin-bottom: 18px;
  }
  .member-slot {
    background: var(--surface2);
    border: 1.5px solid var(--border);
    border-radius: 12px;
    padding: 9px 12px;
    display: flex; align-items: center; gap: 9px;
    font-size: 13px;
    transition: all 0.2s;
  }
  .member-slot.me { border-color: var(--pink); background: rgba(232,69,122,0.06); }
  .member-slot.empty { opacity: 0.35; color: var(--text-dim); }
  .slot-dot {
    width: 11px; height: 11px; border-radius: 50%; flex-shrink: 0;
    box-shadow: 0 0 0 2px rgba(255,255,255,0.8);
  }
  .slot-name { flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; font-weight: 700; }
  .slot-cnt { font-size: 11px; color: var(--text-dim); }
  .me-badge {
    font-size: 10px; background: var(--pink); color: #fff;
    padding: 1px 6px; border-radius: 20px; font-weight: 900;
  }

  /* FORM */
  .join-form { display: flex; gap: 8px; }
  .join-form input {
    flex: 1;
    background: var(--surface2);
    border: 1.5px solid var(--border);
    border-radius: 12px;
    padding: 11px 16px;
    color: var(--text);
    font-family: 'Gothic A1', sans-serif;
    font-size: 14px; outline: none;
    transition: border-color 0.2s;
  }
  .join-form input:focus { border-color: var(--pink); }
  .join-form input::placeholder { color: var(--text-dim); }

  /* BUTTONS */
  .btn {
    background: linear-gradient(135deg, var(--pink), var(--pink-light));
    color: #fff; border: none;
    border-radius: 12px;
    padding: 11px 22px;
    font-family: 'Gothic A1', sans-serif;
    font-weight: 900; font-size: 14px;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.1s;
    white-space: nowrap;
    box-shadow: 0 4px 14px rgba(232,69,122,0.3);
  }
  .btn:hover { opacity: 0.88; }
  .btn:active { transform: scale(0.97); }
  .btn-ghost {
    background: transparent; color: var(--pink);
    border: 1.5px solid var(--pink); box-shadow: none;
  }
  .btn-danger {
    background: linear-gradient(135deg,#ff4d4d,#ff8a65);
    box-shadow: 0 4px 14px rgba(255,77,77,0.2);
  }
  .btn-sm { padding: 7px 14px; font-size: 12px; border-radius: 9px; }

  /* MY BAR */
  .my-bar {
    background: linear-gradient(135deg,rgba(232,69,122,0.08),rgba(244,114,168,0.08));
    border: 1.5px solid var(--border2);
    border-radius: 14px;
    padding: 12px 18px;
    display: flex; align-items: center;
    justify-content: space-between;
    gap: 12px; flex-wrap: wrap;
    margin-bottom: 16px;
    position: relative; z-index: 1;
  }
  .my-bar-left { display: flex; align-items: center; gap: 10px; font-size: 14px; font-weight: 700; }
  .my-color-dot { width: 14px; height: 14px; border-radius: 50%; flex-shrink: 0; box-shadow: 0 0 0 2px #fff; }
  .my-bar-btns { display: flex; gap: 8px; flex-wrap: wrap; }

  /* CALENDAR */
  .cal-controls {
    display: flex; align-items: center; justify-content: center;
    gap: 14px; margin-bottom: 18px;
  }
  .cal-nav {
    background: var(--surface2); border: 1.5px solid var(--border);
    color: var(--pink); width: 38px; height: 38px;
    border-radius: 12px; font-size: 18px; cursor: pointer;
    transition: all 0.2s; display: flex; align-items: center; justify-content: center;
  }
  .cal-nav:hover { background: var(--pink-pale); border-color: var(--pink); }
  .cal-title {
    font-family: 'Nanum Myeongjo', serif;
    font-size: 20px; font-weight: 800; color: var(--pink-deep);
    min-width: 150px; text-align: center;
  }
  .cal-grid { display: grid; grid-template-columns: repeat(7,1fr); gap: 5px; }
  .cal-dayname {
    text-align: center; font-size: 11px; font-weight: 900;
    color: var(--text-dim); padding: 5px 0 8px; letter-spacing: 0.5px;
  }
  .cal-dayname.sun { color: var(--pink); }
  .cal-dayname.sat { color: #9b5de5; }

  .cal-cell {
    background: var(--surface); border: 1.5px solid var(--border);
    border-radius: 12px; min-height: 66px; padding: 6px 5px;
    cursor: pointer; transition: all 0.15s;
    position: relative; user-select: none;
  }
  .cal-cell:hover:not(.empty-cell):not(.past-cell) {
    border-color: var(--pink-light);
    background: rgba(232,69,122,0.03);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(232,69,122,0.1);
  }
  .cal-cell.empty-cell { background: transparent; border-color: transparent; cursor: default; }
  .cal-cell.past-cell { opacity: 0.28; cursor: not-allowed; }
  .cal-cell.selected-me {
    border-color: var(--pink) !important;
    background: rgba(232,69,122,0.07) !important;
    box-shadow: 0 0 0 2px rgba(232,69,122,0.15) !important;
  }
  .cal-cell.today-cell .day-num {
    background: var(--pink); color: #fff; border-radius: 50%;
    width: 20px; height: 20px; display: flex; align-items: center; justify-content: center;
  }
  .day-num { font-size: 12px; font-weight: 900; margin-bottom: 3px; width: fit-content; }
  .day-num.sun { color: var(--pink); }
  .day-num.sat { color: #9b5de5; }

  .dot-row { display: flex; flex-wrap: wrap; gap: 3px; margin-top: 2px; }
  .dot { width: 9px; height: 9px; border-radius: 50%; flex-shrink: 0; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
  .count-badge {
    position: absolute; bottom: 4px; right: 5px;
    font-size: 9px; font-weight: 900; color: var(--text-dim);
  }

  /* LEGEND */
  .legend {
    display: flex; flex-wrap: wrap; gap: 8px;
    justify-content: center; margin-bottom: 16px;
    position: relative; z-index: 1;
  }
  .legend-item {
    display: flex; align-items: center; gap: 6px;
    background: var(--surface); border: 1.5px solid var(--border);
    border-radius: 20px; padding: 5px 12px;
    font-size: 12px; font-weight: 700;
    box-shadow: 0 2px 8px rgba(220,80,130,0.05);
  }
  .legend-dot { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }

  /* SUMMARY */
  .summary-list { display: flex; flex-direction: column; gap: 7px; }
  .summary-item {
    display: flex; justify-content: space-between; align-items: center;
    padding: 10px 14px;
    background: var(--surface2); border: 1px solid var(--border);
    border-radius: 12px; font-size: 13px; font-weight: 700;
  }
  .summary-rank {
    width: 22px; height: 22px; border-radius: 50%;
    background: var(--pink-pale); color: var(--pink-deep);
    font-size: 11px; font-weight: 900;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0; margin-right: 10px;
  }
  .summary-rank.top1 { background: var(--pink); color: #fff; }
  .summary-dots { display: flex; gap: 4px; align-items: center; }

  /* TOAST */
  .toast {
    position: fixed; bottom: 28px; left: 50%;
    transform: translateX(-50%) translateY(16px);
    background: linear-gradient(135deg, var(--pink), var(--pink-light));
    color: #fff; padding: 11px 24px; border-radius: 50px;
    font-weight: 900; font-size: 13px;
    opacity: 0; transition: opacity 0.3s, transform 0.3s;
    z-index: 999; pointer-events: none; white-space: nowrap;
    box-shadow: 0 6px 20px rgba(232,69,122,0.35);
  }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

  @media (max-width: 480px) {
    .cal-cell { min-height: 52px; }
    .dot { width: 7px; height: 7px; }
    .member-slots { grid-template-columns: 1fr; }
    .my-bar { flex-direction: column; align-items: flex-start; }
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-petals">🌸 🌸 🌸 🌸 🌸 🌸</div>
  <h1>🌸 여행 날짜 맞추기</h1>
  <p>가능한 날짜를 선택해서 친구들과 공유하세요</p>
</div>

<div id="loading">
  <div class="spinner"></div>
  데이터 불러오는 중...
</div>

<div id="main">
  <div class="section-wrap">

    <div class="card" id="register-section">
      <div class="card-title">
        👥 참가자
        <span style="font-size:13px;font-weight:400;color:var(--text-dim)" id="member-count">(0/10명)</span>
      </div>
      <div class="member-slots" id="member-slots"></div>
      <div id="join-area">
        <div class="join-form">
          <input type="text" id="name-input" placeholder="내 이름 입력..." maxlength="8">
          <button class="btn" onclick="joinTrip()">참가 🌸</button>
        </div>
      </div>
    </div>

    <div class="my-bar" id="my-bar" style="display:none">
      <div class="my-bar-left">
        <div class="my-color-dot" id="my-color-dot"></div>
        <span id="my-name-label"></span>
        <span style="color:var(--text-dim);font-size:12px;font-weight:400">으로 참가 중</span>
      </div>
      <div class="my-bar-btns">
        <button class="btn btn-ghost btn-sm" onclick="clearMyDates()">날짜 초기화</button>
        <button class="btn btn-danger btn-sm" onclick="leaveTrip()">나가기</button>
      </div>
    </div>

    <div class="card">
      <div class="cal-controls">
        <button class="cal-nav" onclick="prevMonth()">‹</button>
        <div class="cal-title" id="cal-title"></div>
        <button class="cal-nav" onclick="nextMonth()">›</button>
      </div>
      <div class="cal-grid" id="cal-grid"></div>
    </div>

    <div class="legend" id="legend"></div>

    <div class="card">
      <div class="card-title">🔥 인기 날짜 TOP 5</div>
      <div class="summary-list" id="summary-list"></div>
    </div>

  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const COLOR_VALS = [
  '#E8457A','#FF85A1','#C2185B','#FF4D8D','#F48FB1',
  '#AD1457','#FF80AB','#D81B60','#FCB8CE','#880E4F'
];
const MAX = 10;
const STORAGE_KEY = 'travel_cal_shared_v1';

let state = { members: [], dates: {} };
let myId = null;
let curYear, curMonth;

// ── 공유 스토리지 ──────────────────────────────
async function loadState() {
  try {
    const result = await window.storage.get(STORAGE_KEY, true);
    if (result && result.value) {
      const parsed = JSON.parse(result.value);
      if (parsed && parsed.members) state = parsed;
    }
  } catch(e) { /* 첫 접속이면 기본값 */ }
}

async function saveState() {
  try {
    await window.storage.set(STORAGE_KEY, JSON.stringify(state), true);
  } catch(e) { console.error('저장 실패', e); }
}

function getColor(idx) { return COLOR_VALS[idx % COLOR_VALS.length]; }

// ── 초기화 ──────────────────────────────────────
async function init() {
  await loadState();
  const now = new Date();
  curYear = now.getFullYear();
  curMonth = now.getMonth();

  const sid = sessionStorage.getItem('my_member_id');
  if (sid && state.members.find(m => m.id === sid)) myId = sid;

  document.getElementById('loading').style.display = 'none';
  document.getElementById('main').style.display = 'block';
  render();

  // 30초마다 자동 동기화
  setInterval(async () => {
    const snap = JSON.stringify(state);
    await loadState();
    if (JSON.stringify(state) !== snap) render();
  }, 30000);
}

// ── 렌더 ────────────────────────────────────────
function render() {
  renderMembers();
  renderCalendar();
  renderLegend();
  renderSummary();

  const me = myId ? state.members.find(m => m.id === myId) : null;
  const myBar = document.getElementById('my-bar');

  if (me) {
    myBar.style.display = 'flex';
    document.getElementById('my-name-label').textContent = me.name;
    document.getElementById('my-color-dot').style.background = getColor(me.colorIdx);
    document.getElementById('join-area').style.display = 'none';
  } else {
    myBar.style.display = 'none';
    const joinArea = document.getElementById('join-area');
    if (state.members.length >= MAX) {
      joinArea.innerHTML = '<p style="color:var(--text-dim);font-size:13px;text-align:center;padding:4px">인원이 꽉 찼습니다 🌸 (10/10)</p>';
    } else {
      joinArea.innerHTML = `<div class="join-form">
        <input type="text" id="name-input" placeholder="내 이름 입력..." maxlength="8">
        <button class="btn" onclick="joinTrip()">참가 🌸</button>
      </div>`;
      document.getElementById('name-input').addEventListener('keydown', e => { if(e.key==='Enter') joinTrip(); });
    }
  }
  document.getElementById('member-count').textContent = `(${state.members.length}/10명)`;
}

function renderMembers() {
  const container = document.getElementById('member-slots');
  container.innerHTML = '';
  for (let i = 0; i < MAX; i++) {
    const m = state.members[i];
    const div = document.createElement('div');
    if (m) {
      const isMe = m.id === myId;
      let cnt = 0;
      for (const k in state.dates) { if((state.dates[k]||[]).includes(m.id)) cnt++; }
      div.className = 'member-slot' + (isMe?' me':'');
      div.innerHTML = `
        <div class="slot-dot" style="background:${getColor(m.colorIdx)}"></div>
        <span class="slot-name">${m.name}</span>
        ${cnt>0?`<span class="slot-cnt">${cnt}일</span>`:''}
        ${isMe?'<span class="me-badge">나</span>':''}`;
    } else {
      div.className = 'member-slot empty';
      div.innerHTML = `<div class="slot-dot" style="background:${getColor(i)};opacity:0.2"></div><span class="slot-name">빈 자리</span>`;
    }
    container.appendChild(div);
  }
}

function renderCalendar() {
  const grid = document.getElementById('cal-grid');
  const title = document.getElementById('cal-title');
  const months = ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];
  title.textContent = `${curYear}년 ${months[curMonth]}`;
  grid.innerHTML = '';

  const dayNames = ['일','월','화','수','목','금','토'];
  dayNames.forEach((d,i)=>{
    const h = document.createElement('div');
    h.className = 'cal-dayname'+(i===0?' sun':i===6?' sat':'');
    h.textContent = d;
    grid.appendChild(h);
  });

  const first = new Date(curYear, curMonth, 1).getDay();
  const lastDay = new Date(curYear, curMonth+1, 0).getDate();
  const todayDate = new Date(); todayDate.setHours(0,0,0,0);
  const todayStr = `${todayDate.getFullYear()}-${String(todayDate.getMonth()+1).padStart(2,'0')}-${String(todayDate.getDate()).padStart(2,'0')}`;

  for(let i=0;i<first;i++){
    const c=document.createElement('div'); c.className='cal-cell empty-cell'; grid.appendChild(c);
  }

  for(let d=1;d<=lastDay;d++){
    const dateKey=`${curYear}-${String(curMonth+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
    const cellDate=new Date(curYear,curMonth,d);
    const dow=cellDate.getDay();
    const isPast=cellDate<todayDate;
    const isToday=dateKey===todayStr;
    const memberIds=state.dates[dateKey]||[];
    const isMe=memberIds.includes(myId);

    const cell=document.createElement('div');
    let cls='cal-cell';
    if(isPast) cls+=' past-cell';
    if(isMe) cls+=' selected-me';
    if(isToday) cls+=' today-cell';
    cell.className=cls;

    const num=document.createElement('div');
    num.className='day-num'+(dow===0?' sun':dow===6?' sat':'');
    num.textContent=d;
    cell.appendChild(num);

    if(memberIds.length>0){
      const row=document.createElement('div'); row.className='dot-row';
      memberIds.forEach(mid=>{
        const m=state.members.find(x=>x.id===mid);
        if(m){
          const dot=document.createElement('div');
          dot.className='dot'; dot.style.background=getColor(m.colorIdx); dot.title=m.name;
          row.appendChild(dot);
        }
      });
      cell.appendChild(row);
      const badge=document.createElement('div'); badge.className='count-badge';
      badge.textContent=`${memberIds.length}명`; cell.appendChild(badge);
    }

    if(!isPast) cell.onclick=()=>toggleDate(dateKey);
    grid.appendChild(cell);
  }
}

function renderLegend(){
  const l=document.getElementById('legend'); l.innerHTML='';
  state.members.forEach(m=>{
    const item=document.createElement('div'); item.className='legend-item';
    item.innerHTML=`<div class="legend-dot" style="background:${getColor(m.colorIdx)}"></div>${m.name}${m.id===myId?' <span style="color:var(--pink);font-size:11px">(나)</span>':''}`;
    l.appendChild(item);
  });
}

function renderSummary(){
  const list=document.getElementById('summary-list'); list.innerHTML='';
  const entries=Object.entries(state.dates)
    .filter(([,ids])=>ids&&ids.length>0)
    .map(([date,ids])=>({date,ids,count:ids.length}))
    .sort((a,b)=>b.count-a.count).slice(0,5);

  if(entries.length===0){
    list.innerHTML='<div style="color:var(--text-dim);font-size:13px;text-align:center;padding:12px">아직 선택된 날짜가 없어요 🌸</div>';
    return;
  }
  const wdays=['일','월','화','수','목','금','토'];
  entries.forEach((e,idx)=>{
    const item=document.createElement('div'); item.className='summary-item';
    const dt=new Date(e.date+'T00:00:00');
    const label=`${dt.getMonth()+1}월 ${dt.getDate()}일 (${wdays[dt.getDay()]})`;
    const dotsHtml=e.ids.map(mid=>{
      const m=state.members.find(x=>x.id===mid);
      return m?`<div class="dot" style="background:${getColor(m.colorIdx)}" title="${m.name}"></div>`:'';
    }).join('');
    item.innerHTML=`
      <div style="display:flex;align-items:center">
        <div class="summary-rank${idx===0?' top1':''}">${idx+1}</div>
        <span>${label} — <strong style="color:var(--pink)">${e.count}명</strong> 가능</span>
      </div>
      <div class="summary-dots">${dotsHtml}</div>`;
    list.appendChild(item);
  });
}

// ── 액션 ────────────────────────────────────────
async function toggleDate(dateKey){
  if(!myId){showToast('먼저 이름을 입력하고 참가해주세요!');return;}
  await loadState();
  if(!state.dates[dateKey]) state.dates[dateKey]=[];
  const idx=state.dates[dateKey].indexOf(myId);
  if(idx===-1){state.dates[dateKey].push(myId);showToast('날짜 선택됨 🌸');}
  else{
    state.dates[dateKey].splice(idx,1);
    if(!state.dates[dateKey].length) delete state.dates[dateKey];
  }
  await saveState(); render();
}

async function joinTrip(){
  const input=document.getElementById('name-input');
  if(!input) return;
  const name=input.value.trim();
  if(!name){showToast('이름을 입력해주세요');return;}
  await loadState();
  if(state.members.length>=MAX){showToast('인원이 꽉 찼어요 (최대 10명)');return;}
  if(state.members.find(m=>m.name===name)){showToast('이미 같은 이름이 있어요');return;}
  const id='u_'+Date.now()+'_'+Math.random().toString(36).slice(2,6);
  state.members.push({id,name,colorIdx:state.members.length});
  myId=id;
  sessionStorage.setItem('my_member_id',id);
  await saveState();
  showToast(`${name}님 환영해요! 🌸`);
  render();
}

async function clearMyDates(){
  if(!myId) return;
  await loadState();
  let cnt=0;
  for(const k in state.dates){
    const i=(state.dates[k]||[]).indexOf(myId);
    if(i!==-1){state.dates[k].splice(i,1);cnt++;if(!state.dates[k].length)delete state.dates[k];}
  }
  await saveState(); showToast(`${cnt}개 날짜 초기화됨`); render();
}

async function leaveTrip(){
  if(!myId) return;
  await loadState();
  for(const k in state.dates){
    const i=(state.dates[k]||[]).indexOf(myId);
    if(i!==-1){state.dates[k].splice(i,1);if(!state.dates[k].length)delete state.dates[k];}
  }
  state.members=state.members.filter(m=>m.id!==myId);
  state.members.forEach((m,i)=>{m.colorIdx=i;});
  myId=null;
  sessionStorage.removeItem('my_member_id');
  await saveState(); showToast('나갔어요. 또 참가할 수 있어요!'); render();
}

function prevMonth(){curMonth--;if(curMonth<0){curMonth=11;curYear--;}renderCalendar();}
function nextMonth(){curMonth++;if(curMonth>11){curMonth=0;curYear++;}renderCalendar();}

let toastTimer;
function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer=setTimeout(()=>t.classList.remove('show'),2200);
}

init();
</script>
</body>
</html>

