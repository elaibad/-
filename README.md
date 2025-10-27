<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Ачивкaр — журнал жизни по ачивкам</title>
<style>
  :root{
    --bg:#0f1724; --card:#0b1220; --accent:#6ee7b7; --muted:#94a3b8; --glass: rgba(255,255,255,0.03);
    --maxw:980px;
    font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    color: #e6eef8;
  }
  body{margin:0; background:linear-gradient(180deg,#071024 0%, #08182a 60%); min-height:100vh; display:flex; align-items:flex-start; justify-content:center; padding:28px;}
  .wrap{width:100%; max-width:var(--maxw);}
  header{display:flex; align-items:center; gap:16px; margin-bottom:18px;}
  .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#60a5fa); display:flex;align-items:center;justify-content:center;font-weight:700;color:#022;box-shadow:0 6px 18px rgba(6,8,23,0.6);}
  h1{font-size:20px;margin:0}
  p.lead{margin:0;color:var(--muted); font-size:13px;}
  .grid{display:grid; grid-template-columns: 1fr 360px; gap:18px;}
  .card{background:var(--card); border-radius:12px; padding:14px; box-shadow:0 6px 18px rgba(2,6,23,0.6);}
  .controls{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px;}
  input,select,textarea,button{font:inherit}
  .form-row{display:flex;gap:8px; margin-bottom:8px;}
  .form-row input,.form-row select{flex:1;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:inherit}
  button{padding:8px 12px;border-radius:8px;border:0;background:linear-gradient(90deg,var(--accent),#60a5fa);color:#022;cursor:pointer}
  .muted{color:var(--muted); font-size:13px}
  .list{display:flex;flex-direction:column;gap:8px; max-height:56vh; overflow:auto;padding-right:6px;}
  .ach{display:flex;align-items:center;gap:10px;padding:10px;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);border:1px solid rgba(255,255,255,0.02)}
  .ach.done{opacity:0.6;text-decoration:line-through}
  .ach .left{flex:1}
  .meta{display:flex;gap:8px;align-items:center;font-size:12px;color:var(--muted)}
  .small{font-size:12px;color:var(--muted)}
  .badges{display:flex;gap:8px;flex-wrap:wrap}
  .badge{padding:6px 8px;border-radius:999px;background:rgba(255,255,255,0.03);font-size:13px}
  .progress{height:12px;background:rgba(255,255,255,0.04);border-radius:999px;overflow:hidden}
  .progress > i{display:block;height:100%;background:linear-gradient(90deg,var(--accent),#60a5fa)}
  .topline{display:flex;gap:12px;align-items:center;justify-content:space-between;margin-bottom:12px}
  .filters{display:flex;gap:8px;align-items:center}
  .actions{display:flex;gap:8px}
  .stats-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
  .stat{padding:10px;border-radius:8px;background:rgba(255,255,255,0.02);text-align:center}
  .center{display:flex;align-items:center;justify-content:center}
  @media (max-width:900px){
    .grid{grid-template-columns:1fr; }
    .logo{display:none}
  }
  /* tiny calendar */
  .calendar{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;font-size:12px}
  .cal-day{padding:6px;border-radius:6px;background:rgba(255,255,255,0.02);min-height:46px; display:flex;flex-direction:column;justify-content:space-between}
  .dot{width:8px;height:8px;border-radius:999px;background:var(--accent);margin-left:4px}
  .pill{padding:6px 8px;border-radius:999px;background:rgba(255,255,255,0.03);font-size:12px}
  /* tiny footer */
  footer{margin-top:14px;color:var(--muted);font-size:13px;text-align:center}
  .tiny{font-size:12px;color:var(--muted)}
</style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">АЧ</div>
      <div>
        <h1>Ачивкaр — журнал жизни по ачивкам</h1>
        <p class="lead">Фиксируй события, набирай очки, открывай бейджи и следи за прогрессом.</p>
      </div>
    </header>

    <div class="grid">
      <div>
        <div class="card">
          <div class="topline">
            <div class="muted">Добавить ачивку</div>
            <div class="actions">
              <button id="btnExport">Экспорт</button>
              <button id="btnImport">Импорт</button>
              <input id="fileInput" type="file" accept="application/json" style="display:none" />
            </div>
          </div>

          <div style="margin-bottom:8px" id="addForm">
            <div class="form-row">
              <input id="title" placeholder="Название (напр. 'Пробежать 5к')" />
              <select id="category">
                <option value="Фитнес">Фитнес</option>
                <option value="Работа">Работа</option>
                <option value="Учёба">Учёба</option>
                <option value="Хобби">Хобби</option>
                <option value="Отношения">Отношения</option>
                <option value="Другое">Другое</option>
              </select>
            </div>
            <div class="form-row">
              <input id="pts" type="number" placeholder="Очки (напр. 10)" min="1" value="10" />
              <input id="date" type="date" />
            </div>
            <div class="form-row">
              <input id="tags" placeholder="Тэги через запятую (например: утро,ранок)" />
            </div>
            <div class="form-row" style="align-items:flex-start">
              <textarea id="desc" placeholder="Короткое описание (необязательно)" style="flex:1;padding:8px;border-radius:8px;background:var(--glass);border:1px solid rgba(255,255,255,0.03);min-height:60px;"></textarea>
            </div>
            <div style="display:flex;gap:8px">
              <button id="add">Добавить ачивку</button>
              <button id="addQuick" style="background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)">Быстро (сегодня)</button>
            </div>
          </div>

          <hr style="border:none;height:8px" />

          <div class="controls">
            <input id="search" placeholder="Поиск по названию или тегам" style="padding:8px;border-radius:8px;background:var(--glass);border:1px solid rgba(255,255,255,0.03);flex:1" />
            <select id="filterCat">
              <option value="">Все категории</option>
              <option>Фитнес</option><option>Работа</option><option>Учёба</option><option>Хобби</option><option>Отношения</option><option>Другое</option>
            </select>
            <select id="sortBy">
              <option value="new">По дате (новые)</option>
              <option value="old">По дате (старые)</option>
              <option value="pts">По очкам</option>
            </select>
            <button id="clearDone" style="background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)">Очистить выполненные</button>
          </div>

          <div class="list" id="list"></div>
        </div>

        <div style="height:12px"></div>

        <div class="card">
          <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
            <div class="muted">Календарь (текущий месяц)</div>
            <div class="tiny" id="monthLabel"></div>
          </div>
          <div class="calendar" id="calendar"></div>
        </div>
      </div>

      <aside>
        <div class="card">
          <div style="display:flex;flex-direction:column;gap:8px">
            <div style="display:flex;align-items:center;justify-content:space-between">
              <div>
                <div class="muted">Текущие очки</div>
                <div style="font-weight:700;font-size:22px" id="score">0</div>
              </div>
              <div style="width:140px">
                <div class="muted">Прогресс к следующему бейджу</div>
                <div class="progress" style="margin-top:6px"><i id="prog" style="width:10%"></i></div>
              </div>
            </div>

            <div>
              <div class="muted" style="margin-bottom:6px">Бейджи</div>
              <div class="badges" id="badges"></div>
            </div>

            <hr style="border:none;height:8px" />

            <div>
              <div class="muted">Статистика</div>
              <div class="stats-grid" style="margin-top:8px">
                <div class="stat">
                  <div class="muted">Ачивок всего</div>
                  <div id="statTotal" style="font-weight:700;font-size:18px">0</div>
                </div>
                <div class="stat">
                  <div class="muted">Выполнено</div>
                  <div id="statDone" style="font-weight:700;font-size:18px">0</div>
                </div>
                <div class="stat">
                  <div class="muted">Текущий стрик</div>
                  <div id="statStreak" style="font-weight:700;font-size:18px">0</div>
                </div>
              </div>
            </div>

            <hr style="border:none;height:8px" />

            <div>
              <div class="muted">Экспорт / импорт</div>
              <div class="tiny" style="margin-top:6px">Экспортирует все данные в JSON. Импорт восстанавливает их (перезапишет текущие данные).</div>
            </div>

          </div>
        </div>

        <div style="height:12px"></div>

        <div class="card">
          <div class="muted" style="margin-bottom:8px">Быстрые действия</div>
          <div style="display:flex;flex-direction:column;gap:8px">
            <button id="quickRun">+ Пробежка 5к (10 пт)</button>
            <button id="quickRead">+ Читать 30 мин (8 пт)</button>
            <button id="resetBtn" style="background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)">Сбросить все (удалит всё)</button>
          </div>
        </div>

        <div style="height:12px"></div>

        <div class="card">
          <div class="muted">Советы</div>
          <ul class="tiny" style="padding-left:16px;margin:8px 0 0 0;line-height:1.5;color:var(--muted)">
            <li>Делай короткие ачивки — легче закрывать стрики.</li>
            <li>Используй тэги (утро/вечер/работа) для фильтрации.</li>
            <li>Экспортируй данные периодически для бэкапа.</li>
          </ul>
        </div>
      </aside>
    </div>

    <footer>
      <div class="tiny">Сайт локальный — всё хранится в твоём браузере. Чтобы опубликовать: загрузи `index.html` на GitHub Pages или любой хост.</div>
    </footer>
  </div>

<script>
/* Простая реализация: данные в localStorage */
const LS_KEY = 'achievkar_v1';
function uid(){ return 'id_'+Math.random().toString(36).slice(2,9); }
function todayISO(){ const d=new Date(); return d.toISOString().slice(0,10); }
const badgeRules = [
  {id:'starter', title:'Новичок', need:20, desc:'Набрать 20 очков'},
  {id:'active', title:'Активный', need:100, desc:'Набрать 100 очков'},
  {id:'veteran', title:'Ветеран', need:500, desc:'Набрать 500 очков'},
  {id:'streak7', title:'Стрик 7', need:7, kind:'streak', desc:'Выполнять хотя бы 1 ачивку 7 дней подряд'}
];

let state = { items: [], score:0, badges:[], createdAt: new Date().toISOString() };

/* load / save */
function load(){
  try{
    const raw = localStorage.getItem(LS_KEY);
    if(raw){ state = JSON.parse(raw); }
  }catch(e){ console.error(e); }
}
function save(){
  state.updatedAt = new Date().toISOString();
  localStorage.setItem(LS_KEY, JSON.stringify(state));
}

/* util */
function parseTags(str){ return str.split(',').map(s=>s.trim()).filter(Boolean); }
function formatDate(d){ if(!d) return ''; return d; }

/* core: add, toggle, remove */
function addItem({title,desc,category,tags,pts,date}){
  const it = { id: uid(), title, desc, category, tags, pts:+pts, date:date||todayISO(), done:false, createdAt:new Date().toISOString() };
  state.items.push(it); recalc(); save(); render();
}
function toggleDone(id){
  const it = state.items.find(x=>x.id===id);
  if(!it) return;
  it.done = !it.done;
  if(it.done){ state.score = (state.score||0)+ (it.pts||0); }
  else{ state.score = Math.max(0,(state.score||0) - (it.pts||0)); }
  recalcBadges();
  save(); render();
}
function removeItem(id){
  state.items = state.items.filter(x=>x.id!==id); recalc(); save(); render();
}
function clearDone(){
  state.items = state.items.filter(x=>!x.done); save(); render();
}
function recalc(){
  state.score = state.items.filter(x=>x.done).reduce((s,i)=>s+i.pts,0);
  recalcBadges();
}

/* badges */
function recalcBadges(){
  const earned = new Set(state.badges||[]);
  // points-based
  for(const r of badgeRules.filter(r=>!r.kind)){
    if((state.score||0) >= r.need) earned.add(r.id);
    else earned.delete(r.id);
  }
  // streak
  const streak = computeStreak();
  if(streak >= 7) earned.add('streak7'); else earned.delete('streak7');
  state.badges = Array.from(earned);
}

/* stats & streak */
function computeStreak(){
  // count consecutive days up to today with at least one done item
  const doneDates = new Set(state.items.filter(i=>i.done).map(i=>i.date));
  let s=0; let d=new Date();
  while(true){
    const iso = d.toISOString().slice(0,10);
    if(doneDates.has(iso)){ s++; d.setDate(d.getDate()-1); }
    else break;
  }
  return s;
}

/* export / import */
function exportJSON(){
  const blob = new Blob([JSON.stringify(state, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'achievkar_export.json'; a.click();
  URL.revokeObjectURL(url);
}
function importJSON(file){
  const reader = new FileReader();
  reader.onload = e=>{
    try{
      const data = JSON.parse(e.target.result);
      // basic validation
      if(!data.items) return alert('Неверный файл импорта');
      if(!confirm('Импорт перезапишет текущие данные. Продолжить?')) return;
      state = data; save(); render();
    }catch(err){ alert('Ошибка чтения файла'); }
  };
  reader.readAsText(file);
}

/* render */
function render(){
  // list
  const list = document.getElementById('list'); list.innerHTML='';
  const q = document.getElementById('search').value.trim().toLowerCase();
  const cat = document.getElementById('filterCat').value;
  const sort = document.getElementById('sortBy').value;

  let items = state.items.slice();
  if(q){
    items = items.filter(i => i.title.toLowerCase().includes(q) || (i.tags||[]).join(' ').toLowerCase().includes(q) || (i.desc||'').toLowerCase().includes(q));
  }
  if(cat) items = items.filter(i=>i.category===cat);
  if(sort==='new') items.sort((a,b)=>new Date(b.createdAt)-new Date(a.createdAt));
  if(sort==='old') items.sort((a,b)=>new Date(a.createdAt)-new Date(b.createdAt));
  if(sort==='pts') items.sort((a,b)=>b.pts - a.pts);

  for(const it of items){
    const el = document.createElement('div'); el.className='ach'+(it.done?' done':'');
    el.innerHTML = `
      <div style="display:flex;gap:10px;align-items:center">
        <input type="checkbox" ${it.done?'checked':''} data-id="${it.id}" />
      </div>
      <div class="left">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div style="font-weight:600">${escapeHtml(it.title)}</div>
          <div class="muted small">${escapeHtml(it.category)} · ${it.pts} пт</div>
        </div>
        <div class="meta" style="margin-top:6px">
          <div class="small">${escapeHtml(it.desc||'')}</div>
          <div style="margin-left:auto">${formatDate(it.date)}</div>
        </div>
        <div style="margin-top:6px" class="small">${(it.tags||[]).map(t=>`<span class="pill">${escapeHtml(t)}</span>`).join(' ')}</div>
      </div>
      <div style="display:flex;flex-direction:column;gap:6px;margin-left:8px">
        <button data-action="remove" data-id="${it.id}" style="background:transparent;border:0;color:var(--muted)">Удалить</button>
      </div>
    `;
    // events
    el.querySelector('input[type=checkbox]').addEventListener('change', (e)=>{ toggleDone(it.id); });
    el.querySelector('[data-action=remove]').addEventListener('click', ()=>{ if(confirm('Удалить ачивку?')) removeItem(it.id); });
    list.appendChild(el);
  }

  // stats
  document.getElementById('statTotal').innerText = state.items.length;
  document.getElementById('statDone').innerText = state.items.filter(x=>x.done).length;
  document.getElementById('statStreak').innerText = computeStreak();
  document.getElementById('score').innerText = state.score || 0;

  // badges
  const bd = document.getElementById('badges'); bd.innerHTML='';
  for(const r of badgeRules){
    const has = (state.badges||[]).includes(r.id);
    const el = document.createElement('div'); el.className='badge'; el.style.opacity = has?1:0.45;
    el.title = r.desc;
    el.innerHTML = (has? '🏆 ' : '🔒 ') + r.title + (r.need && !r.kind ? ` · ${r.need} пт` : '');
    bd.appendChild(el);
  }

  // progress to next points badge
  const points = state.score||0;
  const nextRule = badgeRules.filter(r=>!r.kind).sort((a,b)=>a.need-b.need).find(r=>points<r.need);
  const progEl = document.getElementById('prog');
  if(nextRule){
    const pct = Math.min(100, Math.round((points / nextRule.need) * 100));
    progEl.style.width = pct + '%';
    document.querySelector('.progress').title = `${points}/${nextRule.need} очков до "${nextRule.title}"`;
  } else {
    progEl.style.width = '100%';
    document.querySelector('.progress').title = `Все бейджи по очкам получены`;
  }

  // calendar
  renderCalendar();

}

function renderCalendar(){
  const cal = document.getElementById('calendar'); cal.innerHTML='';
  const now = new Date();
  const year = now.getFullYear(), month = now.getMonth();
  const first = new Date(year, month, 1);
  const startDay = first.getDay(); // 0..6 (Sun..Sat)
  const offset = (startDay+6)%7; // shift to mon start
  const daysInMonth = new Date(year, month+1, 0).getDate();
  document.getElementById('monthLabel').innerText = first.toLocaleString('ru',{month:'long', year:'numeric'});
  const doneByDay = {};
  for(const it of state.items.filter(i=>i.done)){
    doneByDay[it.date] = (doneByDay[it.date]||0)+1;
  }
  // create empty leading
  for(let i=0;i<offset;i++){ const d = document.createElement('div'); d.className='cal-day'; d.innerHTML=''; cal.appendChild(d); }
  for(let day=1; day<=daysInMonth; day++){
    const iso = `${year}-${String(month+1).padStart(2,'0')}-${String(day).padStart(2,'0')}`;
    const cnt = doneByDay[iso] || 0;
    const d = document.createElement('div'); d.className='cal-day';
    d.innerHTML = `<div style="display:flex;align-items:center;justify-content:space-between"><div>${day}</div>${cnt?'<div class="dot"></div>':''}</div>
                   <div style="display:flex;flex-direction:column;gap:4px">${cnt?'<div class="small">'+cnt+' выполнено</div>':''}</div>`;
    cal.appendChild(d);
  }
}

function escapeHtml(s){ if(!s) return ''; return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }

/* init UI handlers */
document.getElementById('add').addEventListener('click', ()=>{
  const title = document.getElementById('title').value.trim();
  if(!title) return alert('Введите название');
  const desc = document.getElementById('desc').value.trim();
  const category = document.getElementById('category').value;
  const tags = parseTags(document.getElementById('tags').value);
  const pts = Math.max(1, Number(document.getElementById('pts').value)||1);
  const date = document.getElementById('date').value || todayISO();
  addItem({title,desc,category,tags,pts,date});
  // clear quick fields
  document.getElementById('title').value=''; document.getElementById('desc').value=''; document.getElementById('tags').value='';
});
document.getElementById('addQuick').addEventListener('click', ()=>{
  const title = document.getElementById('title').value.trim() || 'Ачивка сегодня';
  const pts = Number(document.getElementById('pts').value)||10;
  addItem({title,desc:'',category:document.getElementById('category').value,tags:[],pts,date:todayISO()});
});
document.getElementById('search').addEventListener('input', render);
document.getElementById('filterCat').addEventListener('change', render);
document.getElementById('sortBy').addEventListener('change', render);
document.getElementById('clearDone').addEventListener('click', ()=>{ if(confirm('Удалить все выполненные ачивки?')) { clearDone(); }});
document.getElementById('btnExport').addEventListener('click', exportJSON);
document.getElementById('btnImport').addEventListener('click', ()=> document.getElementById('fileInput').click());
document.getElementById('fileInput').addEventListener('change', (e)=>{ const f = e.target.files[0]; if(f) importJSON(f); e.target.value=''; });
document.getElementById('quickRun').addEventListener('click', ()=> addItem({title:'Пробежать 5к',desc:'Быстрая пробежка',category:'Фитнес',tags:['утро'],pts:10,date:todayISO()}));
document.getElementById('quickRead').addEventListener('click', ()=> addItem({title:'Читать 30 мин',desc:'Время чтения',category:'Учёба',tags:['вечер'],pts:8,date:todayISO()}));
document.getElementById('resetBtn').addEventListener('click', ()=>{ if(confirm('Сбросить все данные? Это действие необратимо.')){ state={items:[],score:0,badges:[],createdAt:new Date().toISOString()}; save(); render(); } });

/* initial load */
load();
recalc();
render();

/* accessibility: handle enter in title to add */
document.getElementById('title').addEventListener('keydown', (e)=>{ if(e.key==='Enter'){ document.getElementById('add').click(); } });
</script>
</body>
</html>
