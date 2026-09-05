<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>MY LOG</title>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>
*{box-sizing:border-box}
:root{--bg:#f3f0e8;--paper:#fffdf8;--ink:#252525;--muted:#777;--line:#cfc9bc;--soft:#eeeae0;--accent:#222}
body{margin:0;background:var(--bg);color:var(--ink);font-family:-apple-system,BlinkMacSystemFont,"Hiragino Sans","Yu Gothic",sans-serif}
button,input,select,textarea{font:inherit}
button{cursor:pointer}
.auth-wrap{min-height:100vh;display:grid;place-items:center;padding:24px}
.auth-card{width:min(430px,100%);background:var(--paper);border:1px solid var(--line);padding:28px;box-shadow:0 8px 30px #0000000c}
.logo{font-size:30px;font-weight:800;letter-spacing:.12em}
.sub{font:12px ui-monospace,SFMono-Regular,monospace;color:var(--muted);letter-spacing:.12em}
.dash{border-top:1px dashed var(--line);margin:20px 0}
.auth-tabs{display:flex;border-bottom:1px solid var(--line);margin-bottom:18px}
.auth-tabs button{flex:1;border:0;background:none;padding:11px;color:var(--muted)}
.auth-tabs button.active{color:var(--ink);font-weight:700;border-bottom:2px solid var(--ink)}
label{display:block;font-size:12px;font-weight:700;margin:12px 0 6px}
input,select,textarea{width:100%;border:1px solid var(--line);background:#fff;padding:10px 11px;border-radius:7px;color:var(--ink)}
textarea{min-height:100px;resize:vertical}
.primary{width:100%;border:0;background:#222;color:#fff;padding:12px;border-radius:7px;font-weight:700;margin-top:16px}
.small{font-size:12px;color:var(--muted);line-height:1.6}
.auth-msg{min-height:20px;margin-top:12px;font-size:13px}
#app{min-height:100vh}
header{position:sticky;top:0;z-index:10;background:#f3f0eefa;backdrop-filter:blur(8px);border-bottom:1px solid var(--line)}
.top{max-width:1200px;margin:auto;padding:14px 18px;display:flex;justify-content:space-between;align-items:center;gap:12px}
.brand{font-weight:900;letter-spacing:.14em}.brand small{display:block;font-size:9px;font-weight:500;letter-spacing:.2em;color:var(--muted)}
.userbar{display:flex;align-items:center;gap:8px;font:11px ui-monospace,SFMono-Regular,monospace}
.ghost{border:1px solid var(--line);background:transparent;border-radius:6px;padding:7px 10px}
nav{max-width:1200px;margin:auto;display:flex;overflow:auto;border-top:1px dashed var(--line)}
nav button{white-space:nowrap;border:0;background:none;padding:10px 13px;font-size:11px;letter-spacing:.08em;color:var(--muted)}
nav button.active{color:#111;font-weight:800;border-bottom:2px solid #111}
main{max-width:1200px;margin:auto;padding:20px 16px 50px}
.page{display:none}.page.active{display:block}
.page-title{display:flex;justify-content:space-between;align-items:end;gap:12px;margin-bottom:15px}
h1{font-size:25px;margin:0}.kicker{font:10px ui-monospace,SFMono-Regular,monospace;color:var(--muted);letter-spacing:.14em}
.card{background:var(--paper);border:1px solid var(--line);padding:16px;margin-bottom:14px}
.card h2{font-size:14px;margin:0 0 12px;letter-spacing:.08em}
.btn{border:1px solid #222;background:#222;color:#fff;border-radius:6px;padding:8px 12px;font-size:12px}
.btn.light{background:transparent;color:#222;border-color:var(--line)}
.btn.danger{background:transparent;color:#8a3333;border-color:#d5aaaa}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
.stat{background:var(--paper);border:1px solid var(--line);padding:14px}
.stat .label{font:10px ui-monospace,SFMono-Regular,monospace;color:var(--muted)}
.stat .value{font-size:24px;font-weight:800;margin-top:5px}
.list{display:grid;gap:8px}
.item{border-top:1px dashed var(--line);padding:10px 0;display:flex;justify-content:space-between;gap:10px}
.item:first-child{border-top:0}
.item-main{min-width:0}.item-title{font-weight:700;font-size:14px}.item-meta{font-size:11px;color:var(--muted);margin-top:3px;line-height:1.5}
.actions{display:flex;gap:5px;flex-wrap:wrap;align-items:center}
.actions button{font-size:11px;padding:5px 7px;border:1px solid var(--line);background:transparent;border-radius:5px}
.stars{letter-spacing:1px;font-size:12px}
.done{text-decoration:line-through;color:#999}
.formgrid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.formgrid .full{grid-column:1/-1}
.empty{padding:22px 14px;text-align:center;color:#999;font-size:12px;border:1px dashed var(--line);background:#faf8f2;line-height:1.8}
.empty .empty-mark{font-size:22px;display:block;color:#aaa;margin-bottom:3px}
.empty.left{text-align:left}
.note .item-meta{text-align:left;white-space:pre-wrap}
.todo-completed{margin-top:14px;border-top:1px dashed var(--line);padding-top:14px}
.todo-completed summary{cursor:pointer;font-size:12px;color:#777;font-weight:700;list-style:none}
.todo-completed summary::-webkit-details-marker{display:none}
.todo-completed summary:before{content:"＋ ";font-weight:900}
.todo-completed[open] summary:before{content:"− "}
.month-card{margin-top:10px;padding:11px 12px;background:#fbfaf6;border:1px solid #ddd7cb}
.month-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
.month-title{font:700 11px ui-monospace,SFMono-Regular,monospace;letter-spacing:.1em;color:#777}
.month-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:2px}
.month-wd,.month-cell{min-height:30px;font:9px ui-monospace,SFMono-Regular,monospace;text-align:center;color:#888}
.month-wd{min-height:18px}
.month-cell{position:relative;padding-top:4px;color:#555;border-radius:4px}
.month-cell.today{font-weight:900;color:#111;text-decoration:underline}
.month-cell.other{opacity:.25}
.month-dots{display:flex;justify-content:center;gap:2px;margin-top:3px;min-height:5px}
.month-dot{width:4px;height:4px;border-radius:50%;display:block}
.home-receipt{max-width:620px;margin:0 auto;background:#fffdf8;border:1px solid #d8d1c5;padding:18px 16px;position:relative}
.home-receipt:before,.home-receipt:after{content:"";display:block;height:7px;background:repeating-linear-gradient(135deg,transparent 0 5px,#d8d1c5 5px 6px,transparent 6px 10px);opacity:.55}
.home-receipt .receipt-line{margin:10px 0}
.home-receipt .receipt-row{font-size:11px}
.home-receipt .receipt-stamp{font:9px ui-monospace,SFMono-Regular,monospace;color:#999;letter-spacing:.12em}
.home-receipt .receipt-today{font:700 18px ui-monospace,SFMono-Regular,monospace;letter-spacing:.08em}
.home-receipt .receipt-empty{font-size:11px;color:#999;padding:12px 0;text-align:left}
.week-controls{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
.week-grid{display:grid;grid-template-columns:repeat(7,minmax(120px,1fr));overflow:auto;border:1px solid var(--line)}
.day{min-width:120px;border-right:1px solid var(--line);min-height:240px;background:var(--paper)}
.day:last-child{border-right:0}
.day-head{padding:10px;border-bottom:1px dashed var(--line);font:11px ui-monospace,SFMono-Regular,monospace}
.day-head b{display:block;font-size:16px;font-family:inherit}
.event{margin:8px;border:1px solid var(--line);padding:8px;background:#f7f4ec;border-radius:6px}
.event.color-1{background:#f5d7d7}
.event.color-2{background:#f5dbe8}
.event.color-3{background:#dcebdc}
.event.color-4{background:#f4edc9}
.event.color-5{background:#dce8f5}
.event.color-6{background:#fff}
.event.color-7{background:#e2e2e2}
.event-time{font:10px ui-monospace,SFMono-Regular,monospace;color:#666}
.event-name{font-size:12px;font-weight:700;margin-top:2px}
.event-detail{font-size:10px;color:#777;margin-top:2px}
.dot{display:inline-block;width:7px;height:7px;border-radius:50%;background:#777;margin-right:5px}
.color-1{background:#e8bcbc}
.color-2{background:#edc6d8}
.color-3{background:#cfe2cf}
.color-4{background:#eee1a9}
.color-5{background:#c9dced}
.color-6{background:#fff}
.color-7,.color-8{background:#d9d9d9}
.todo-row{align-items:flex-start}
.check{width:18px;height:18px;accent-color:#222;margin-top:2px}
.folder{border:1px solid var(--line);padding:12px;background:#fbf9f2}
.folder-name{font-weight:800}
.note{padding:10px 0;border-top:1px dashed var(--line)}
.note:first-child{border-top:0}
.record-photo{width:70px;height:70px;object-fit:cover;border:1px solid var(--line);border-radius:5px}
.receipt{max-width:500px;margin:auto;background:#fff;padding:24px 20px;border:1px solid #d9d3c7;font-family:ui-monospace,SFMono-Regular,Menlo,monospace}
.receipt h2{text-align:center;letter-spacing:.15em}
.receipt-line{border-top:1px dashed #888;margin:13px 0}
.receipt-row{display:flex;justify-content:space-between;padding:4px 0;font-size:12px}
.barcode{height:45px;margin:18px auto 5px;width:180px;background:repeating-linear-gradient(90deg,#111 0 2px,transparent 2px 5px,#111 5px 6px,transparent 6px 9px)}
.home-date{font:12px ui-monospace,SFMono-Regular,monospace;color:#777}
.modal{position:fixed;inset:0;background:#0007;z-index:30;display:none;align-items:center;justify-content:center;padding:18px}
.modal.show{display:flex}
.modal-box{width:min(520px,100%);max-height:90vh;overflow:auto;background:var(--paper);padding:20px;border:1px solid var(--line)}
.modal-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px}
.modal-head h3{margin:0;font-size:17px}
.close{border:0;background:none;font-size:20px}
.sync{font-size:10px;color:#777}
@media(max-width:700px){
  .grid2,.stats{grid-template-columns:1fr 1fr}
  .formgrid{grid-template-columns:1fr}
  .stats .stat{padding:11px}
  .stat .value{font-size:20px}
  .top{padding:12px}
  .userbar span{max-width:90px;overflow:hidden;text-overflow:ellipsis}
}
@media print{
  header,#app>main>.page:not(#receipt),.no-print{display:none!important}
  .receipt{border:0;box-shadow:none;margin:0 auto}
  body{background:#fff}
}
</style>
</head>
<body>

<div id="authScreen" class="auth-wrap">
  <div class="auth-card">
    <div class="logo">MY LOG</div>
    <div class="sub">RECORD / PLAN / NOTE</div>
    <div class="dash"></div>

    <div class="auth-tabs">
      <button id="loginTab" class="active" onclick="switchAuth('login')">LOGIN</button>
      <button id="signupTab" onclick="switchAuth('signup')">NEW ACCOUNT</button>
    </div>

    <div id="loginForm">
      <label>ユーザー名</label>
      <input id="loginUsername" autocomplete="username" placeholder="ユーザー名">

      <label>パスワード</label>
      <input id="loginPassword" type="password" autocomplete="current-password" placeholder="パスワード">

      <button class="primary" onclick="login()">ログイン</button>
    </div>

    <div id="signupForm" style="display:none">
      <label>ユーザー名</label>
      <input id="signupUsername" autocomplete="username" placeholder="好きなユーザー名">
      <div class="small">同じユーザー名は登録できません。大文字・小文字は区別しません。</div>

      <label>パスワード</label>
      <input id="signupPassword" type="password" autocomplete="new-password" placeholder="8文字以上">

      <label>パスワード（確認）</label>
      <input id="signupPassword2" type="password" autocomplete="new-password" placeholder="もう一度入力">

      <button class="primary" onclick="signup()">アカウントを作る</button>
    </div>

    <div id="authMsg" class="auth-msg"></div>
    <div class="small">この画面ではメールアドレスを入力する必要はありません。</div>
  </div>
</div>

<div id="app" style="display:none">

<header>
  <div class="top">
    <div class="brand">
      MY LOG
      <small>RECORD / PLAN / NOTE</small>
    </div>

    <div class="userbar">
      <span id="syncStatus">SYNC</span>
      <span id="currentUsername"></span>
      <button class="ghost" onclick="migrateOldData()">旧データ移行</button>
      <button class="ghost" onclick="logout()">ログアウト</button>
    </div>
  </div>

  <nav id="nav"></nav>
</header>

<main>

<section id="home" class="page active">
  <div class="page-title">
    <div>
      <div class="kicker">PERSONAL DASHBOARD</div>
      <h1>HOME</h1>
    </div>
  </div>

  <div class="home-receipt">
    <div class="receipt-stamp">MY LOG / DAILY RECEIPT</div>

    <div class="receipt-line"></div>

    <div class="receipt-today">TODAY</div>
    <div class="home-date" id="homeDate"></div>

    <div class="receipt-line"></div>

    <div class="receipt-row">
      <span>OPEN TODO</span>
      <b id="statTodo">0</b>
    </div>

    <div class="receipt-row">
      <span>STUDY TODAY</span>
      <b id="statStudy">0m</b>
    </div>

    <div class="receipt-row">
      <span>SPENDING TODAY</span>
      <b id="statMoney">¥0</b>
    </div>

    <div class="receipt-row">
      <span>WEEK EVENTS</span>
      <b id="statEvents">0</b>
    </div>

    <div class="receipt-line"></div>

    <div class="receipt-stamp">TODAY'S SCHEDULE</div>
    <div id="homeToday"></div>

    <div class="receipt-line"></div>

    <div class="receipt-stamp">KEEP RECORDING / KEEP GOING.</div>
  </div>

  <div class="grid2" style="margin-top:14px">

    <div class="card">
      <h2>UPCOMING TODO</h2>
      <div id="homeTodos"></div>
    </div>

    <div class="card">
      <h2>RECENT RECORD</h2>
      <div id="homeRecords"></div>
    </div>

  </div>
</section>


<section id="week" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">MONDAY — SUNDAY</div>
      <h1>WEEK</h1>
    </div>

    <button class="btn" onclick="openEventModal()">＋ 予定を追加</button>
  </div>

  <div class="card">

    <div class="week-controls">
      <button class="btn light" onclick="changeWeek(-1)">← 前週</button>
      <b id="weekLabel"></b>
      <button class="btn light" onclick="changeWeek(1)">次週 →</button>
    </div>

    <div id="weekGrid" class="week-grid"></div>

    <div style="margin-top:10px;font-size:10px;color:#777">
      COLOR　
      <span class="dot color-1"></span>赤　
      <span class="dot color-2"></span>ピンク　
      <span class="dot color-3"></span>緑　
      <span class="dot color-4"></span>黄色　
      <span class="dot color-5"></span>青　
      <span class="dot color-6"></span>白　
      <span class="dot color-7"></span>グレー
    </div>

    <div id="monthCalendar" class="month-card"></div>

  </div>

  <div class="card">
    <h2>WEEK TODO</h2>
    <div id="weekTodos"></div>
  </div>
</section>


<section id="todo" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">TASK MANAGEMENT</div>
      <h1>TODO</h1>
    </div>

    <button class="btn" onclick="openTodoModal()">＋ タスクを追加</button>
  </div>

  <div class="card">
    <div id="todoList"></div>
  </div>
</section>


<section id="money" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">SPENDING LOG</div>
      <h1>MONEY</h1>
    </div>

    <button class="btn" onclick="openMoneyModal()">＋ 記録を追加</button>
  </div>

  <div class="card">
    <div class="kicker">TOTAL</div>

    <div id="moneyTotal" style="font-size:30px;font-weight:900;margin:4px 0 14px">
      ¥0
    </div>

    <div id="moneyList"></div>
  </div>
</section>


<section id="wish" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">WANT TO HAVE</div>
      <h1>WISH LIST</h1>
    </div>

    <button class="btn" onclick="openWishModal()">＋ 追加</button>
  </div>

  <div class="card">
    <div id="wishList"></div>
  </div>
</section>


<section id="memo" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">NOTE FOLDERS</div>
      <h1>MEMO</h1>
    </div>

    <button class="btn" onclick="openMemoModal()">＋ メモを追加</button>
  </div>

  <div id="memoList"></div>
</section>


<section id="record" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">STUDY LOG</div>
      <h1>RECORD</h1>
    </div>

    <button class="btn" onclick="openRecordModal()">＋ 今日の記録</button>
  </div>

  <div class="card">
    <h2>今日の勉強時間</h2>
    <div id="todayStudy" style="font-size:32px;font-weight:900">0 min</div>
  </div>

  <div class="card">
    <h2>記録一覧</h2>
    <div id="recordList"></div>
  </div>
</section>


<section id="receipt" class="page">
  <div class="page-title">
    <div>
      <div class="kicker">WEEKLY SUMMARY</div>
      <h1>RECEIPT</h1>
    </div>

    <button class="btn" onclick="window.print()">印刷 / PDF</button>
  </div>

  <div class="receipt" id="receiptBody"></div>
</section>

</main>
</div>


<div id="modal" class="modal" onclick="if(event.target===this)closeModal()">
  <div class="modal-box">

    <div class="modal-head">
      <h3 id="modalTitle"></h3>
      <button class="close" onclick="closeModal()">×</button>
    </div>

    <div id="modalBody"></div>

  </div>
</div>


<script>

/* =========================
   1. Supabase設定
   ========================= */

const SUPABASE_URL = "https://xjanjhywkozjohxultta.supabase.co";
const SUPABASE_ANON_KEY = "sb_publishable_ICkqvGWwyLdz5_rPE7YCCg_yyoqIL0v";

const {createClient} = window.supabase;
const sb = createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

let currentUser = null;
let currentUsername = "";

let events = [];
let todos = [];
let money = [];
let wishes = [];
let notes = [];
let records = [];

let weekOffset = 0;
let saveQueue = Promise.resolve();


const navItems = [
  ["home","HOME"],
  ["week","WEEK"],
  ["todo","TODO"],
  ["money","MONEY"],
  ["wish","WISH"],
  ["memo","MEMO"],
  ["record","RECORD"],
  ["receipt","RECEIPT"]
];

document.getElementById("nav").innerHTML =
  navItems.map(([id,label]) =>
    `<button id="nav-${id}" onclick="showPage('${id}')">${label}</button>`
  ).join("");


function uid(){
  return crypto.randomUUID
    ? crypto.randomUUID()
    : Date.now()+"_"+Math.random().toString(36).slice(2);
}


/* 日本時間の日付を使う */
function localDateString(d=new Date()){
  const y=d.getFullYear();
  const m=String(d.getMonth()+1).padStart(2,"0");
  const day=String(d.getDate()).padStart(2,"0");
  return `${y}-${m}-${day}`;
}

function todayStr(){
  return localDateString();
}


function esc(s=""){
  return String(s).replace(/[&<>"']/g,m=>({
    "&":"&amp;",
    "<":"&lt;",
    ">":"&gt;",
    '"':"&quot;",
    "'":"&#39;"
  }[m]));
}


function yen(n){
  return "¥"+Number(n||0).toLocaleString("ja-JP");
}


function stars(n){
  n=Number(n||0);
  return "★".repeat(n)+"☆".repeat(5-n);
}


function fmtDate(d){
  if(!d)return "";
  let x=new Date(d+"T00:00:00");
  return `${String(x.getMonth()+1).padStart(2,"0")}.${String(x.getDate()).padStart(2,"0")}`;
}


function monday(d){
  let x=new Date(d);
  x.setHours(0,0,0,0);

  let day=x.getDay();
  let diff=day===0?-6:1-day;

  x.setDate(x.getDate()+diff);
  return x;
}


function dateKey(x){
  return localDateString(x);
}


function addDays(d,n){
  let x=new Date(d);
  x.setDate(x.getDate()+n);
  return x;
}


function currentWeekDates(){
  let m=monday(new Date());
  m.setDate(m.getDate()+weekOffset*7);

  return Array.from(
    {length:7},
    (_,i)=>addDays(m,i)
  );
}


function weekText(){
  let ds=currentWeekDates();

  return `${dateKey(ds[0]).replaceAll("-",".")} - ${dateKey(ds[6]).slice(5).replace("-",".")}`;
}


function setSync(t){
  document.getElementById("syncStatus").textContent=t;
}


/* =========================
   AUTH
   ========================= */

async function hashUsername(name){
  const normalized=name.trim().toLowerCase();

  const buf=await crypto.subtle.digest(
    "SHA-256",
    new TextEncoder().encode(normalized)
  );

  return Array.from(new Uint8Array(buf))
    .map(x=>x.toString(16).padStart(2,"0"))
    .join("");
}


async function internalEmail(name){
  return "u_"+await hashUsername(name)+"@mylog.local";
}


function authMessage(t,ok=false){
  const e=document.getElementById("authMsg");

  e.textContent=t;
  e.style.color=ok?"#38633f":"#8a3333";
}


function switchAuth(mode){
  document.getElementById("loginForm").style.display =
    mode==="login" ? "block" : "none";

  document.getElementById("signupForm").style.display =
    mode==="signup" ? "block" : "none";

  document.getElementById("loginTab").classList.toggle(
    "active",
    mode==="login"
  );

  document.getElementById("signupTab").classList.toggle(
    "active",
    mode==="signup"
  );

  authMessage("");
}


function validateUsername(u){
  u=u.trim();

  if(u.length<2 || u.length>30){
    return "ユーザー名は2〜30文字にしてください。";
  }

  return "";
}


async function login(){

  const u=document.getElementById("loginUsername").value.trim();
  const p=document.getElementById("loginPassword").value;

  if(!u || !p){
    authMessage("ユーザー名とパスワードを入力してください。");
    return;
  }

  authMessage("ログイン中...",true);

  const email=await internalEmail(u);

  const {data,error}=await sb.auth.signInWithPassword({
    email,
    password:p
  });

  if(error){
    authMessage("ユーザー名またはパスワードが違います。");
    return;
  }

  await enterApp(data.session);
}


async function signup(){

  const u=document.getElementById("signupUsername").value.trim();
  const p=document.getElementById("signupPassword").value;
  const p2=document.getElementById("signupPassword2").value;

  const v=validateUsername(u);

  if(v){
    authMessage(v);
    return;
  }

  if(p.length<8){
    authMessage("パスワードは8文字以上にしてください。");
    return;
  }

  if(p!==p2){
    authMessage("パスワードが一致していません。");
    return;
  }

  authMessage("登録中...",true);

  const {
    data:avail,
    error:availErr
  }=await sb.rpc(
    "username_available",
    {p_username:u}
  );

  if(availErr){
    authMessage(
      "登録確認に失敗しました。Supabaseの設定を確認してください。"
    );
    return;
  }

  if(!avail){
    authMessage("そのユーザー名はすでに使われています。");
    return;
  }

  const email=await internalEmail(u);

  const {
    data,
    error
  }=await sb.auth.signUp({
    email,
    password:p,
    options:{
      data:{
        username:u
      }
    }
  });

  if(error){
    authMessage(
      error.message.includes("already")
        ? "そのユーザー名はすでに使われています。"
        : "アカウントを作成できませんでした。"
    );

    return;
  }

  if(!data.session){
    authMessage(
      "登録は完了しました。Supabaseで「メール確認」をOFFにしてから、もう一度登録してください。"
    );
    return;
  }

  await enterApp(data.session);
}


async function logout(){

  await sb.auth.signOut();

  currentUser=null;

  document.getElementById("app").style.display="none";
  document.getElementById("authScreen").style.display="grid";

  switchAuth("login");
}


async function enterApp(session){

  if(!session)return;

  currentUser=session.user;

  currentUsername=
    session.user.user_metadata?.username || "";

  document.getElementById("currentUsername").textContent=
    "@"+currentUsername;

  document.getElementById("authScreen").style.display="none";
  document.getElementById("app").style.display="block";

  await loadCloud();

  renderAll();
}


/* =========================
   CLOUD
   ========================= */

async function loadCloud(){

  setSync("LOADING");

  const {
    data,
    error
  }=await sb
    .from("mylog_data")
    .select("*")
    .eq("user_id",currentUser.id)
    .maybeSingle();

  if(error){

    alert("データの読み込みに失敗しました。");
    console.error(error);

    return;
  }

  if(data){

    events=Array.isArray(data.events)
      ? data.events
      : [];

    todos=Array.isArray(data.todos)
      ? data.todos
      : [];

    money=Array.isArray(data.money)
      ? data.money
      : [];

    wishes=Array.isArray(data.wishes)
      ? data.wishes
      : [];

    notes=Array.isArray(data.notes)
      ? data.notes
      : [];

    records=Array.isArray(data.records)
      ? data.records
      : [];

  }else{

    events=[];
    todos=[];
    money=[];
    wishes=[];
    notes=[];
    records=[];

    await saveAll();
  }

  setSync("SYNCED");
}


async function saveCloudNow(){

  if(!currentUser)return;

  setSync("SAVING");

  const payload={
    user_id:currentUser.id,
    events,
    todos,
    money,
    wishes,
    notes,
    records,
    updated_at:new Date().toISOString()
  };


  const {
    data:updated,
    error:updateError
  }=await sb
    .from("mylog_data")
    .update(payload)
    .eq("user_id",currentUser.id)
    .select("user_id")
    .maybeSingle();


  if(updateError){

    console.error("UPDATE ERROR",updateError);

    setSync("ERROR");

    throw new Error(
      "保存エラー: "+
      (updateError.message ||
       updateError.code ||
       "unknown")
    );
  }


  if(!updated){

    const {
      error:insertError
    }=await sb
      .from("mylog_data")
      .insert(payload);

    if(insertError){

      console.error("INSERT ERROR",insertError);

      setSync("ERROR");

      throw new Error(
        "初回保存エラー: "+
        (insertError.message ||
         insertError.code ||
         "unknown")
      );
    }
  }

  setSync("SYNCED");
}


function saveAll(){

  if(!currentUser){
    return Promise.resolve();
  }

  saveQueue=saveQueue
    .then(()=>saveCloudNow())
    .catch(e=>{

      console.error(e);

      setSync("ERROR");

    });

  return saveQueue;
}


/* =========================
   PAGE
   ========================= */

function renderAll(){

  renderHome();
  renderWeek();
  renderTodos();
  renderMoney();
  renderWishes();
  renderMemos();
  renderRecords();
  renderReceipt();

  showPage(
    document.querySelector(".page.active")?.id || "home"
  );
}


function showPage(id){

  document.querySelectorAll(".page")
    .forEach(x=>x.classList.remove("active"));

  document.getElementById(id)
    .classList.add("active");

  document.querySelectorAll("nav button")
    .forEach(x=>x.classList.remove("active"));

  document.getElementById("nav-"+id)
    ?.classList.add("active");


  if(id==="home")renderHome();
  if(id==="week")renderWeek();
  if(id==="todo")renderTodos();
  if(id==="money")renderMoney();
  if(id==="wish")renderWishes();
  if(id==="memo")renderMemos();
  if(id==="record")renderRecords();
  if(id==="receipt")renderReceipt();
}


/* =========================
   HOME
   ========================= */

function renderHome(){

  const t=todayStr();

  document.getElementById("homeDate").textContent=
    new Date().toLocaleDateString(
      "ja-JP",
      {
        year:"numeric",
        month:"long",
        day:"numeric",
        weekday:"short"
      }
    );


  const todayEvents=
    events
      .filter(e=>e.date===t)
      .sort(
        (a,b)=>
          (a.time||"99:99")
          .localeCompare(b.time||"99:99")
      );


  document.getElementById("homeToday").innerHTML=
    todayEvents.length

      ? todayEvents.map(e=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">
              <span class="dot color-${Math.min(Number(e.color)||7,7)}"></span>
              ${esc(e.name)}
            </div>

            <div class="item-meta">
              ${e.allDay?"終日":esc(e.time||"")}
              ${e.place?" / "+esc(e.place):""}
            </div>

          </div>

        </div>

      `).join("")

      : `
        <div class="receipt-empty">
          今日の予定はありません。<br>
          予定を入れるとここにレシートみたいに並びます。
        </div>
      `;


  const open=
    todos.filter(x=>!x.done).length;

  const rec=
    records.find(x=>x.date===t);

  const spend=
    money
      .filter(x=>x.date===t)
      .reduce(
        (s,x)=>s+Number(x.price||0),
        0
      );


  const week=currentWeekDates();

  const a=dateKey(week[0]);
  const b=dateKey(week[6]);


  document.getElementById("statTodo").textContent=
    open;

  document.getElementById("statStudy").textContent=
    (
      Number(rec?.hours||0)*60+
      Number(rec?.minutes||0)
    )+"m";

  document.getElementById("statMoney").textContent=
    yen(spend);

  document.getElementById("statEvents").textContent=
    events.filter(
      x=>x.date>=a&&x.date<=b
    ).length;


  const upcoming=
    todos
      .filter(x=>!x.done)
      .slice(0,5);


  document.getElementById("homeTodos").innerHTML=
    upcoming.length

      ? upcoming.map(x=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">
              ${esc(x.title)}
            </div>

            <div class="item-meta">
              ${stars(x.priority)}
              ${x.due?fmtDate(x.due):""}
            </div>

          </div>

        </div>

      `).join("")

      : `
        <div class="empty">
          未完了のTODOはありません。
        </div>
      `;


  const rr=
    [...records]
      .sort(
        (a,b)=>b.date.localeCompare(a.date)
      )
      .slice(0,3);


  document.getElementById("homeRecords").innerHTML=
    rr.length

      ? rr.map(x=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">
              ${fmtDate(x.date)}
              　
              ${Number(x.hours||0)*60+
                Number(x.minutes||0)}
              min
            </div>

            <div class="item-meta">
              ${esc(x.note||"")}
            </div>

          </div>

        </div>

      `).join("")

      : `
        <div class="empty">
          まだ記録がありません。
        </div>
      `;
}


/* =========================
   WEEK
   ========================= */

function changeWeek(n){

  weekOffset+=n;

  renderWeek();
  renderReceipt();
}


function renderWeek(){

  const ds=currentWeekDates();

  document.getElementById("weekLabel").textContent=
    weekText();


  document.getElementById("weekGrid").innerHTML=
    ds.map((d,i)=>{

      const k=dateKey(d);

      const dayEvents=
        events
          .filter(e=>e.date===k)
          .sort(
            (a,b)=>
              (a.time||"99:99")
              .localeCompare(b.time||"99:99")
          );


      const wd=[
        "SUN",
        "MON",
        "TUE",
        "WED",
        "THU",
        "FRI",
        "SAT"
      ][d.getDay()];


      return `
        <div class="day">

          <div class="day-head">
            ${wd}
            <b>${fmtDate(k)}</b>
          </div>

          ${
            dayEvents.length

              ? dayEvents.map(e=>`

                <div class="event color-${Math.min(Number(e.color)||7,7)}">

                  <div class="event-time">
                    ${e.allDay?"終日":esc(e.time||"")}
                  </div>

                  <div class="event-name">
                    ${esc(e.name)}
                  </div>

                  <div class="event-detail">
                    ${esc(e.place||"")}
                    ${esc(e.detail||"")}
                  </div>

                  <div
                    class="actions"
                    style="margin-top:5px"
                  >
                    <button onclick="editEvent('${e.id}')">
                      編集
                    </button>

                    <button onclick="deleteEvent('${e.id}')">
                      削除
                    </button>
                  </div>

                </div>

              `).join("")

              : `
                <div class="empty">
                  —
                </div>
              `
          }

        </div>
      `;

    }).join("");


  const a=dateKey(ds[0]);
  const b=dateKey(ds[6]);


  const wt=
    todos.filter(
      x=>
        x.due &&
        x.due>=a &&
        x.due<=b &&
        !x.done
    );


  document.getElementById("weekTodos").innerHTML=
    wt.length

      ? wt.map(x=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">
              ${esc(x.title)}
            </div>

            <div class="item-meta">
              ${stars(x.priority)}
              　
              ${fmtDate(x.due)}
            </div>

          </div>

        </div>

      `).join("")

      : `
        <div class="empty">

          <span class="empty-mark">✓</span>

          今週の未完了TODOはありません。<br>
          WEEKを軽く整理できています。

        </div>
      `;


  renderMonthCalendar(ds[0]);
}


/* =========================
   MONTH CALENDAR
   ========================= */

function renderMonthCalendar(baseDate){

  const base=new Date(baseDate);

  const y=base.getFullYear();
  const m=base.getMonth();

  const first=new Date(y,m,1);

  const startDay=first.getDay();

  const days=
    new Date(y,m+1,0).getDate();

  const prevDays=
    new Date(y,m,0).getDate();


  const cells=[];


  for(let i=0;i<42;i++){

    const n=i-startDay+1;

    let d;
    let other=false;


    if(n<1){

      d=new Date(
        y,
        m-1,
        prevDays+n
      );

      other=true;

    }else if(n>days){

      d=new Date(
        y,
        m+1,
        n-days
      );

      other=true;

    }else{

      d=new Date(y,m,n);

    }


    const k=dateKey(d);

    const ev=
      events.filter(
        e=>e.date===k
      );


    const dots=
      ev
        .slice(0,4)
        .map(
          e=>
            `<span class="month-dot color-${Math.min(Number(e.color)||7,7)}"></span>`
        )
        .join("");


    cells.push(`

      <div
        class="month-cell
          ${other?"other":""}
          ${k===todayStr()?"today":""}"
      >

        ${d.getDate()}

        <div class="month-dots">
          ${dots}
        </div>

      </div>

    `);
  }


  document.getElementById("monthCalendar").innerHTML=`

    <div class="month-head">

      <span class="month-title">
        MONTH / ${y}.${String(m+1).padStart(2,"0")}
      </span>

      <span style="font-size:9px;color:#aaa">
        ● 予定あり
      </span>

    </div>


    <div class="month-grid">

      ${
        [
          "SUN",
          "MON",
          "TUE",
          "WED",
          "THU",
          "FRI",
          "SAT"
        ]
        .map(
          x=>`
            <div class="month-wd">
              ${x}
            </div>
          `
        )
        .join("")
      }

      ${cells.join("")}

    </div>

  `;
}


/* =========================
   EVENT
   ========================= */

function openEventModal(id=null){

  const e=
    events.find(x=>x.id===id)
    ||
    {
      date:dateKey(currentWeekDates()[0]),
      time:"",
      name:"",
      place:"",
      detail:"",
      color:7,
      allDay:false
    };


  openModal(
    id?"予定を編集":"予定を追加",

    `

      <div class="formgrid">

        <div>
          <label>名前 *</label>
          <input
            id="f_name"
            value="${esc(e.name)}"
          >
        </div>


        <div>
          <label>日付 *</label>
          <input
            id="f_date"
            type="date"
            value="${e.date}"
          >
        </div>


        <div>
          <label>時間</label>
          <input
            id="f_time"
            type="time"
            value="${e.time||""}"
          >
        </div>


        <div>
          <label>場所</label>
          <input
            id="f_place"
            value="${esc(e.place||"")}"
          >
        </div>


        <div class="full">

          <label>
            <input
              id="f_allday"
              type="checkbox"
              ${e.allDay?"checked":""}
            >
            終日
          </label>

        </div>


        <div class="full">

          <label>詳細</label>

          <textarea id="f_detail">
${esc(e.detail||"")}
          </textarea>

        </div>


        <div class="full">

          <label>色</label>

          <select id="f_color">

            ${
              [1,2,3,4,5,6,7]
              .map(
                i=>`

                  <option
                    value="${i}"
                    ${Number(e.color)===i?"selected":""}
                  >
                    ${
                      [
                        "",
                        "赤",
                        "ピンク",
                        "緑",
                        "黄色",
                        "青",
                        "白",
                        "グレー"
                      ][i]
                    }
                  </option>

                `
              )
              .join("")
            }

          </select>

        </div>

      </div>


      <button
        class="primary"
        onclick="saveEvent('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function saveEvent(id){

  const name=
    document.getElementById("f_name")
      .value
      .trim();

  const date=
    document.getElementById("f_date")
      .value;


  if(!name || !date){

    alert("名前と日付は必須です。");

    return;
  }


  const obj={

    id:id||uid(),

    name,

    date,

    time:
      document.getElementById("f_time")
        .value,

    place:
      document.getElementById("f_place")
        .value
        .trim(),

    detail:
      document.getElementById("f_detail")
        .value
        .trim(),

    color:
      Number(
        document.getElementById("f_color")
          .value
      ),

    allDay:
      document.getElementById("f_allday")
        .checked

  };


  const i=
    events.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    events[i]=obj;

  }else{

    events.push(obj);

  }


  saveAll();

  closeModal();

  renderAll();
}


function editEvent(id){
  openEventModal(id);
}


function deleteEvent(id){

  if(
    !confirm(
      "この予定を削除しますか？"
    )
  )return;

  events=
    events.filter(
      x=>x.id!==id
    );

  saveAll();

  renderAll();
}


/* =========================
   TODO
   ========================= */

function renderTodos(){

  const openArr=
    [...todos]
      .filter(x=>!x.done)
      .sort(
        (a,b)=>
          (b.priority||0)-
          (a.priority||0)
      );


  const doneArr=
    [...todos]
      .filter(x=>x.done)
      .sort(
        (a,b)=>
          (b.completedAt||"")
          .localeCompare(
            a.completedAt||""
          )
      );


  const row=x=>`

    <div class="item todo-row">

      <div
        style="display:flex;gap:9px;min-width:0"
      >

        <input
          class="check"
          type="checkbox"
          ${x.done?"checked":""}
          onchange="toggleTodo('${x.id}')"
        >


        <div class="item-main">

          <div
            class="item-title ${x.done?"done":""}"
          >
            ${esc(x.title)}
          </div>


          <div class="item-meta">

            <span class="stars">
              ${stars(x.priority)}
            </span>

            ${x.due?fmtDate(x.due):""}

            ${x.detail
              ?" / "+esc(x.detail)
              :""
            }

          </div>

        </div>

      </div>


      <div class="actions">

        <button
          onclick="editTodo('${x.id}')"
        >
          編集
        </button>


        ${
          !x.done

            ? `
              <button
                onclick="moveTodo('${x.id}',-1)"
              >
                ↑
              </button>

              <button
                onclick="moveTodo('${x.id}',1)"
              >
                ↓
              </button>
            `

            : ""
        }


        <button
          onclick="deleteTodo('${x.id}')"
        >
          削除
        </button>

      </div>

    </div>

  `;


  const openHtml=
    openArr.length

      ? openArr.map(row).join("")

      : `
        <div class="empty">

          <span class="empty-mark">
            □
          </span>

          未完了のTODOはありません。<br>
          新しい予定や課題を追加してみよう。

        </div>
      `;


  const donePreview=
    doneArr
      .slice(0,3)
      .map(row)
      .join("");


  const moreDone=
    doneArr.length>3

      ? `

        <details class="todo-completed">

          <summary>
            それ以前の完了済みを表示
            （${doneArr.length-3}件）
          </summary>

          ${
            doneArr
              .slice(3)
              .map(row)
              .join("")
          }

        </details>

      `

      : "";


  document.getElementById("todoList").innerHTML=`

    <div>

      <div
        class="kicker"
        style="margin-bottom:5px"
      >
        OPEN / ${openArr.length}
      </div>

      ${openHtml}

    </div>


    <div class="todo-completed">

      <div
        class="kicker"
        style="margin-bottom:5px"
      >
        COMPLETED / ${doneArr.length}
      </div>

      ${
        donePreview

        ||

        `
          <div class="empty">

            <span class="empty-mark">
              ○
            </span>

            完了したTODOはここに並びます。

          </div>
        `
      }

    </div>


    ${moreDone}

  `;
}


function openTodoModal(id=null){

  const x=
    todos.find(a=>a.id===id)
    ||
    {
      title:"",
      priority:3,
      due:"",
      detail:""
    };


  openModal(
    id?"TODOを編集":"TODOを追加",

    `

      <label>
        タスク *
      </label>

      <input
        id="t_title"
        value="${esc(x.title)}"
      >


      <label>
        優先度
      </label>

      <select id="t_priority">

        ${
          [1,2,3,4,5]
          .map(
            n=>`

              <option
                value="${n}"
                ${n==x.priority?"selected":""}
              >
                ${stars(n)}
              </option>

            `
          )
          .join("")
        }

      </select>


      <label>
        期限
      </label>

      <input
        id="t_due"
        type="date"
        value="${x.due||""}"
      >


      <label>
        詳細
      </label>

      <textarea id="t_detail">
${esc(x.detail||"")}
      </textarea>


      <button
        class="primary"
        onclick="saveTodo('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function saveTodo(id){

  const title=
    document.getElementById("t_title")
      .value
      .trim();


  if(!title){

    alert(
      "タスク名を入力してください。"
    );

    return;
  }


  const old=
    todos.find(
      x=>x.id===id
    );


  const obj={

    id:id||uid(),

    title,

    priority:
      Number(
        document.getElementById("t_priority")
          .value
      ),

    due:
      document.getElementById("t_due")
        .value,

    detail:
      document.getElementById("t_detail")
        .value
        .trim(),

    done:
      old?.done||false

  };


  const i=
    todos.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    todos[i]=obj;

  }else{

    todos.push(obj);

  }


  saveAll();

  closeModal();

  renderAll();
}


function editTodo(id){
  openTodoModal(id);
}


function toggleTodo(id){

  const x=
    todos.find(
      a=>a.id===id
    );


  if(x){

    x.done=!x.done;

    x.completedAt=
      x.done
        ? new Date().toISOString()
        : "";

  }


  saveAll();

  renderAll();
}


function moveTodo(id,dir){

  const i=
    todos.findIndex(
      x=>x.id===id
    );

  const j=i+dir;


  if(
    i<0 ||
    j<0 ||
    j>=todos.length
  )return;


  [
    todos[i],
    todos[j]
  ]=[
    todos[j],
    todos[i]
  ];


  saveAll();

  renderAll();
}


function deleteTodo(id){

  if(
    !confirm(
      "このTODOを削除しますか？"
    )
  )return;


  todos=
    todos.filter(
      x=>x.id!==id
    );


  saveAll();

  renderAll();
}


/* =========================
   MONEY
   ========================= */

function renderMoney(){

  const total=
    money.reduce(
      (s,x)=>
        s+Number(x.price||0),
      0
    );


  document.getElementById("moneyTotal")
    .textContent=yen(total);


  const arr=
    [...money]
      .sort(
        (a,b)=>
          b.date.localeCompare(a.date)
      );


  document.getElementById("moneyList").innerHTML=

    arr.length

      ? arr.map(x=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">

              ${fmtDate(x.date)}
              　
              ${esc(x.genre||"その他")}
              　
              ${yen(x.price)}

            </div>

            <div class="item-meta">
              ${esc(x.detail||"")}
            </div>

          </div>


          <div class="actions">

            <button
              onclick="editMoney('${x.id}')"
            >
              編集
            </button>

            <button
              onclick="deleteMoney('${x.id}')"
            >
              削除
            </button>

          </div>

        </div>

      `).join("")

      : `

        <div class="empty">

          <span class="empty-mark">
            ¥
          </span>

          まだ支出の記録はありません。<br>
          使ったらここに残しておこう。

        </div>

      `;
}


function openMoneyModal(id=null){

  const x=
    money.find(a=>a.id===id)
    ||
    {
      date:todayStr(),
      genre:"",
      price:"",
      detail:""
    };


  openModal(
    id?"支出を編集":"支出を追加",

    `

      <label>
        日付 *
      </label>

      <input
        id="m_date"
        type="date"
        value="${x.date}"
      >


      <label>
        ジャンル
      </label>

      <input
        id="m_genre"
        value="${esc(x.genre||"")}"
        placeholder="食費 / 交通 / 文房具など"
      >


      <label>
        金額
      </label>

      <input
        id="m_price"
        type="number"
        min="0"
        value="${x.price||""}"
      >


      <label>
        詳細
      </label>

      <textarea id="m_detail">
${esc(x.detail||"")}
      </textarea>


      <button
        class="primary"
        onclick="saveMoney('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function saveMoney(id){

  const obj={

    id:id||uid(),

    date:
      document.getElementById("m_date")
        .value,

    genre:
      document.getElementById("m_genre")
        .value
        .trim(),

    price:
      Number(
        document.getElementById("m_price")
          .value||0
      ),

    detail:
      document.getElementById("m_detail")
        .value
        .trim()

  };


  if(!obj.date){

    alert(
      "日付を入力してください。"
    );

    return;
  }


  const i=
    money.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    money[i]=obj;

  }else{

    money.push(obj);

  }


  saveAll();

  closeModal();

  renderAll();
}


function editMoney(id){
  openMoneyModal(id);
}


function deleteMoney(id){

  if(
    !confirm(
      "この記録を削除しますか？"
    )
  )return;


  money=
    money.filter(
      x=>x.id!==id
    );


  saveAll();

  renderAll();
}


/* =========================
   WISH
   ========================= */

function renderWishes(){

  const arr=
    [...wishes]
      .sort(
        (a,b)=>
          (b.priority||0)-
          (a.priority||0)
      );


  document.getElementById("wishList").innerHTML=

    arr.length

      ? arr.map(x=>`

        <div class="item">

          <div class="item-main">

            <div class="item-title">

              ${esc(x.name)}
              　
              <span class="stars">
                ${stars(x.priority)}
              </span>

            </div>


            <div class="item-meta">

              ${x.price?yen(x.price):""}

              ${x.genre
                ?" / "+esc(x.genre)
                :""
              }

              ${x.detail
                ?" / "+esc(x.detail)
                :""
              }

            </div>

          </div>


          <div class="actions">

            <button
              onclick="editWish('${x.id}')"
            >
              編集
            </button>

            <button
              onclick="deleteWish('${x.id}')"
            >
              削除
            </button>

          </div>

        </div>

      `).join("")

      : `

        <div class="empty">

          <span class="empty-mark">
            ☆
          </span>

          まだWISHは空っぽです。<br>
          気になるものをひとつ、残してみよう。

        </div>

      `;
}


function openWishModal(id=null){

  const x=
    wishes.find(a=>a.id===id)
    ||
    {
      name:"",
      priority:3,
      genre:"",
      price:"",
      detail:""
    };


  openModal(
    id?"WISHを編集":"WISHを追加",

    `

      <label>
        名前 *
      </label>

      <input
        id="w_name"
        value="${esc(x.name)}"
      >


      <label>
        ほしいレベル
      </label>

      <select id="w_priority">

        ${
          [1,2,3,4,5]
          .map(
            n=>`

              <option
                value="${n}"
                ${n==x.priority?"selected":""}
              >
                ${stars(n)}
              </option>

            `
          )
          .join("")
        }

      </select>


      <label>
        ジャンル
      </label>

      <input
        id="w_genre"
        value="${esc(x.genre||"")}"
      >


      <label>
        価格
      </label>

      <input
        id="w_price"
        type="number"
        min="0"
        value="${x.price||""}"
      >


      <label>
        詳細
      </label>

      <textarea id="w_detail">
${esc(x.detail||"")}
      </textarea>


      <button
        class="primary"
        onclick="saveWish('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function saveWish(id){

  const name=
    document.getElementById("w_name")
      .value
      .trim();


  if(!name){

    alert(
      "名前を入力してください。"
    );

    return;
  }


  const obj={

    id:id||uid(),

    name,

    priority:
      Number(
        document.getElementById("w_priority")
          .value
      ),

    genre:
      document.getElementById("w_genre")
        .value
        .trim(),

    price:
      Number(
        document.getElementById("w_price")
          .value||0
      ),

    detail:
      document.getElementById("w_detail")
        .value
        .trim()

  };


  const i=
    wishes.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    wishes[i]=obj;

  }else{

    wishes.push(obj);

  }


  saveAll();

  closeModal();

  renderAll();
}


function editWish(id){
  openWishModal(id);
}


function deleteWish(id){

  if(
    !confirm(
      "このWISHを削除しますか？"
    )
  )return;


  wishes=
    wishes.filter(
      x=>x.id!==id
    );


  saveAll();

  renderAll();
}


/* =========================
   MEMO
   ========================= */

function renderMemos(){

  const groups={};


  notes.forEach(n=>{

    const f=
      n.folder ||
      "その他";

    (groups[f]??=[]).push(n);

  });


  const keys=
    Object.keys(groups)
      .sort();


  document.getElementById("memoList").innerHTML=

    keys.length

      ? keys.map(f=>`

        <div class="card">

          <h2>
            ${esc(f)}
          </h2>


          ${
            groups[f]
              .sort(
                (a,b)=>
                  (b.updatedAt||"")
                  .localeCompare(
                    a.updatedAt||""
                  )
              )
              .map(n=>`

                <div class="note">

                  <div class="item-title">
                    ${esc(n.title)}
                  </div>


                  <div class="item-meta">
                    ${esc(n.text)
                      .replace(/\n/g,"<br>")
                    }
                  </div>


                  <div
                    class="actions"
                    style="margin-top:6px"
                  >

                    <button
                      onclick="editMemo('${n.id}')"
                    >
                      編集
                    </button>

                    <button
                      onclick="deleteMemo('${n.id}')"
                    >
                      削除
                    </button>

                  </div>

                </div>

              `)
              .join("")
          }

        </div>

      `).join("")

      : `

        <div class="card">

          <div class="empty">

            <span class="empty-mark">
              ⌁
            </span>

            まだメモはありません。<br>
            思いついたことを自由に残せます。

          </div>

        </div>

      `;
}


function openMemoModal(id=null){

  const x=
    notes.find(a=>a.id===id)
    ||
    {
      folder:"その他",
      title:"",
      text:""
    };


  openModal(
    id?"メモを編集":"メモを追加",

    `

      <label>
        フォルダー
      </label>

      <input
        id="n_folder"
        value="${esc(x.folder||"その他")}"
      >


      <label>
        タイトル *
      </label>

      <input
        id="n_title"
        value="${esc(x.title)}"
      >


      <label>
        本文
      </label>

      <textarea id="n_text">
${esc(x.text||"")}
      </textarea>


      <button
        class="primary"
        onclick="saveMemo('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function saveMemo(id){

  const title=
    document.getElementById("n_title")
      .value
      .trim();


  if(!title){

    alert(
      "タイトルを入力してください。"
    );

    return;
  }


  const obj={

    id:id||uid(),

    folder:
      document.getElementById("n_folder")
        .value
        .trim() ||
      "その他",

    title,

    text:
      document.getElementById("n_text")
        .value,

    updatedAt:
      new Date().toISOString()

  };


  const i=
    notes.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    notes[i]=obj;

  }else{

    notes.push(obj);

  }


  saveAll();

  closeModal();

  renderAll();
}


function editMemo(id){
  openMemoModal(id);
}


function deleteMemo(id){

  if(
    !confirm(
      "このメモを削除しますか？"
    )
  )return;


  notes=
    notes.filter(
      x=>x.id!==id
    );


  saveAll();

  renderAll();
}


/* =========================
   RECORD
   ========================= */

function renderRecords(){

  const t=todayStr();

  const r=
    records.find(
      x=>x.date===t
    );


  document.getElementById("todayStudy")
    .textContent=
      (
        Number(r?.hours||0)*60+
        Number(r?.minutes||0)
      )+" min";


  const arr=
    [...records]
      .sort(
        (a,b)=>
          b.date.localeCompare(a.date)
      );


  document.getElementById("recordList").innerHTML=

    arr.length

      ? arr.map(x=>`

        <div class="item">

          <div
            style="display:flex;gap:10px"
          >

            ${
              x.photo
                ? `
                  <img
                    class="record-photo"
                    src="${x.photo}"
                  >
                `
                : ""
            }


            <div class="item-main">

              <div class="item-title">

                ${fmtDate(x.date)}
                　
                ${
                  Number(x.hours||0)*60+
                  Number(x.minutes||0)
                }
                min

              </div>


              <div class="item-meta">
                ${esc(x.note||"")}
              </div>

            </div>

          </div>


          <div class="actions">

            <button
              onclick="editRecord('${x.id}')"
            >
              編集
            </button>

            <button
              onclick="deleteRecord('${x.id}')"
            >
              削除
            </button>

          </div>

        </div>

      `).join("")

      : `

        <div class="empty">

          <span class="empty-mark">
            ✎
          </span>

          まだ学習履歴がありません。<br>
          今日の勉強時間を記録すると、ここに積み上がります。

        </div>

      `;
}


function openRecordModal(id=null){

  const x=
    records.find(a=>a.id===id)
    ||
    {
      date:todayStr(),
      hours:0,
      minutes:0,
      note:"",
      photo:""
    };


  openModal(
    id?"記録を編集":"勉強記録を追加",

    `

      <label>
        日付 *
      </label>

      <input
        id="r_date"
        type="date"
        value="${x.date}"
      >


      <div class="formgrid">

        <div>

          <label>
            時間
          </label>

          <input
            id="r_hours"
            type="number"
            min="0"
            value="${x.hours||0}"
          >

        </div>


        <div>

          <label>
            分
          </label>

          <input
            id="r_minutes"
            type="number"
            min="0"
            max="59"
            value="${x.minutes||0}"
          >

        </div>

      </div>


      <label>
        メモ
      </label>

      <textarea id="r_note">
${esc(x.note||"")}
      </textarea>


      <label>
        写真
      </label>

      <input
        id="r_photo"
        type="file"
        accept="image/*"
      >


      ${
        x.photo
          ? `
            <img
              class="record-photo"
              src="${x.photo}"
              style="margin-top:8px"
            >
          `
          : ""
      }


      <button
        class="primary"
        onclick="saveRecord('${id||""}')"
      >
        保存
      </button>

    `
  );
}


function compressImage(file){

  return new Promise(
    (resolve,reject)=>{

      const reader=
        new FileReader();


      reader.onload=()=>{

        const img=
          new Image();


        img.onload=()=>{

          const max=1200;

          const scale=
            Math.min(
              1,
              max/
              Math.max(
                img.width,
                img.height
              )
            );


          const c=
            document.createElement("canvas");


          c.width=
            Math.round(
              img.width*scale
            );

          c.height=
            Math.round(
              img.height*scale
            );


          c.getContext("2d")
            .drawImage(
              img,
              0,
              0,
              c.width,
              c.height
            );


          resolve(
            c.toDataURL(
              "image/jpeg",
              .72
            )
          );

        };


        img.onerror=reject;

        img.src=
          reader.result;

      };


      reader.onerror=reject;

      reader.readAsDataURL(file);

    }
  );
}


async function saveRecord(id){

  const old=
    records.find(
      x=>x.id===id
    );


  let photo=
    old?.photo ||
    "";


  const file=
    document.getElementById("r_photo")
      .files[0];


  if(file){

    photo=
      await compressImage(file);

  }


  const obj={

    id:id||uid(),

    date:
      document.getElementById("r_date")
        .value,

    hours:
      Number(
        document.getElementById("r_hours")
          .value||0
      ),

    minutes:
      Number(
        document.getElementById("r_minutes")
          .value||0
      ),

    note:
      document.getElementById("r_note")
        .value
        .trim(),

    photo

  };


  if(!obj.date){

    alert(
      "日付を入力してください。"
    );

    return;
  }


  const i=
    records.findIndex(
      x=>x.id===obj.id
    );


  if(i>=0){

    records[i]=obj;

  }else{

    records.push(obj);

  }


  await saveAll();

  closeModal();

  renderAll();
}


function editRecord(id){
  openRecordModal(id);
}


function deleteRecord(id){

  if(
    !confirm(
      "この記録を削除しますか？"
    )
  )return;


  records=
    records.filter(
      x=>x.id!==id
    );


  saveAll();

  renderAll();
}


/* =========================
   RECEIPT
   ========================= */

function renderReceipt(){

  const ds=
    currentWeekDates();


  const a=
    dateKey(ds[0]);

  const b=
    dateKey(ds[6]);


  const study=
    records
      .filter(
        x=>x.date>=a&&x.date<=b
      )
      .reduce(
        (s,x)=>
          s+
          Number(x.hours||0)*60+
          Number(x.minutes||0),
        0
      );


  const spend=
    money
      .filter(
        x=>x.date>=a&&x.date<=b
      )
      .reduce(
        (s,x)=>
          s+
          Number(x.price||0),
        0
      );


  const done=
    todos.filter(
      x=>x.done
    ).length;


  const total=
    todos.length;


  const ev=
    events.filter(
      x=>x.date>=a&&x.date<=b
    ).length;


  document.getElementById("receiptBody")
    .innerHTML=`

      <h2>MY LOG</h2>

      <div
        style="
          text-align:center;
          font-size:11px
        "
      >
        WEEK SUMMARY
      </div>


      <div class="receipt-line"></div>


      <div
        style="
          text-align:center;
          font-size:11px
        "
      >
        ${a.replaceAll("-",".")}
        -
        ${b.replaceAll("-",".")}
      </div>


      <div class="receipt-line"></div>


      <div class="receipt-row">
        <span>STUDY TIME</span>
        <b>${study} min</b>
      </div>


      <div class="receipt-row">
        <span>SPENDING</span>
        <b>${yen(spend)}</b>
      </div>


      <div class="receipt-row">
        <span>TASKS DONE</span>
        <b>${done} / ${total}</b>
      </div>


      <div class="receipt-row">
        <span>EVENTS</span>
        <b>${ev}</b>
      </div>


      <div class="receipt-line"></div>


      <div
        style="
          text-align:center;
          font-size:13px;
          font-weight:800
        "
      >
        GOOD JOB THIS WEEK!
      </div>


      <div class="barcode"></div>


      <div
        style="
          text-align:center;
          font-size:9px
        "
      >
        KEEP GOING.
      </div>

    `;
}


/* =========================
   MODAL
   ========================= */

function openModal(title,body){

  document.getElementById("modalTitle")
    .textContent=title;

  document.getElementById("modalBody")
    .innerHTML=body;

  document.getElementById("modal")
    .classList.add("show");
}


function closeModal(){

  document.getElementById("modal")
    .classList.remove("show");

  document.getElementById("modalBody")
    .innerHTML="";
}


/* =========================
   OLD DATA MIGRATION
   ========================= */

function oldArray(key){

  try{

    return JSON.parse(
      localStorage.getItem(key)||"[]"
    );

  }catch{

    return [];

  }
}


function mergeById(a,b){

  const m=new Map();

  [...a,...b].forEach(x=>{

    if(x?.id){
      m.set(x.id,x);
    }

  });

  return [...m.values()];
}


async function migrateOldData(){

  if(!currentUser)return;


  const old={

    events:
      oldArray("mylog_events"),

    todos:
      oldArray("mylog_todos"),

    money:
      oldArray("mylog_money"),

    wishes:
      oldArray("mylog_wishes"),

    notes:
      oldArray("mylog_notes"),

    records:
      oldArray("mylog_records")

  };


  const count=
    Object.values(old)
      .reduce(
        (s,a)=>s+a.length,
        0
      );


  if(!count){

    alert(
      "この端末に移行できる旧データはありません。"
    );

    return;
  }


  if(
    !confirm(
      `この端末にある旧データ ${count}件を、このアカウントへ取り込みます。\n今のクラウドデータには追加される形です。続けますか？`
    )
  )return;


  events=
    mergeById(
      events,
      old.events
    );


  todos=
    mergeById(
      todos,
      old.todos
    );


  money=
    mergeById(
      money,
      old.money
    );


  wishes=
    mergeById(
      wishes,
      old.wishes
    );


  notes=
    mergeById(
      notes,
      old.notes
    );


  records=
    mergeById(
      records,
      old.records
    );


  await saveAll();

  renderAll();


  alert(
    "旧データをクラウドへ移行しました。"
  );
}


/* =========================
   START
   ========================= */

(async function(){

  const {
    data
  }=await sb.auth.getSession();


  if(data.session){

    await enterApp(
      data.session
    );

  }

})();


sb.auth.onAuthStateChange(
  (event,session)=>{

    if(event==="SIGNED_OUT"){

      document.getElementById("app")
        .style.display="none";

      document.getElementById("authScreen")
        .style.display="grid";

    }

  }
);

</script>

</body>
</html>
