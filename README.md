# Log_receipt
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>DAILY RECEIPT</title>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<style>
*{box-sizing:border-box}

:root{
  --paper:#fffdf7;
  --bg:#e8e5dd;
  --ink:#171717;
  --gray:#777;
  --line:#c9c6bd;
  --red:#a22;
}

body{
  margin:0;
  background:var(--bg);
  color:var(--ink);
  font-family:"Courier New","Noto Sans JP",monospace;
}

button,input,textarea,select{
  font:inherit;
}

button{
  cursor:pointer;
}

.hidden{
  display:none!important;
}

#loginScreen{
  min-height:100vh;
  display:flex;
  justify-content:center;
  align-items:center;
  padding:20px;
}

.loginPaper{
  width:100%;
  max-width:430px;
  background:var(--paper);
  padding:35px 28px;
  box-shadow:0 15px 40px #0002;
  text-align:center;
}

.logo{
  font-size:24px;
  font-weight:bold;
  letter-spacing:4px;
}

.sub{
  color:var(--gray);
  font-size:11px;
  margin:8px 0 25px;
}

.field{
  margin-bottom:12px;
}

input,textarea,select{
  width:100%;
  border:1px solid #aaa;
  background:#fffdf7;
  padding:12px;
  color:#111;
  outline:none;
}

textarea{
  min-height:100px;
  resize:vertical;
}

input:focus,textarea:focus,select:focus{
  border-color:#111;
}

.primary{
  width:100%;
  background:#111;
  color:#fff;
  border:0;
  padding:13px;
  font-weight:bold;
}

.secondary{
  background:transparent;
  border:1px solid #999;
  padding:9px 12px;
}

.danger{
  color:#a00;
  border:0;
  background:none;
  font-size:11px;
}

header{
  position:sticky;
  top:0;
  z-index:100;
  background:var(--paper);
  border-bottom:1px solid #ccc;
}

.nav{
  max-width:1200px;
  margin:auto;
  display:flex;
  align-items:center;
  gap:5px;
  padding:10px 15px;
  overflow-x:auto;
}

.navLogo{
  font-weight:bold;
  letter-spacing:2px;
  white-space:nowrap;
  margin-right:15px;
}

.nav button{
  border:0;
  background:none;
  padding:9px 10px;
  font-size:11px;
  white-space:nowrap;
  color:#777;
}

.nav button.active{
  color:#111;
  font-weight:bold;
  border-bottom:2px solid #111;
}

.logout{
  margin-left:auto;
}

.page{
  max-width:1150px;
  margin:auto;
  padding:30px 18px 80px;
}

.pageTitle{
  font-size:25px;
  font-weight:bold;
  margin-bottom:22px;
}

.center{
  text-align:center;
}

.small{
  font-size:10px;
  color:#777;
}

.grid{
  display:grid;
  grid-template-columns:repeat(2,minmax(0,1fr));
  gap:20px;
}

.card{
  background:var(--paper);
  padding:22px;
  box-shadow:0 6px 22px #0001;
}

.cardTitle{
  font-weight:bold;
  font-size:13px;
  margin-bottom:15px;
}

.dash{
  border-top:1px dashed #aaa;
  margin:15px 0;
}

.item{
  border-bottom:1px dashed #bbb;
  padding:12px 0;
  display:flex;
  justify-content:space-between;
  gap:15px;
}

.item:last-child{
  border-bottom:0;
}

.itemMain{
  min-width:0;
  flex:1;
}

.itemTitle{
  font-weight:bold;
  font-size:13px;
  overflow-wrap:anywhere;
}

.itemSub{
  color:#777;
  font-size:10px;
  margin-top:5px;
}

.actions{
  display:flex;
  gap:6px;
  align-items:flex-start;
  flex-shrink:0;
}

.iconBtn{
  border:0;
  background:none;
  font-size:12px;
  color:#777;
}

.check{
  border:1px solid #777;
  background:transparent;
  width:20px;
  height:20px;
  margin-right:7px;
}

.checked{
  text-decoration:line-through;
  opacity:.45;
}

.moneyBig{
  font-size:32px;
  font-weight:bold;
  text-align:right;
  margin-top:25px;
}

.total{
  display:flex;
  justify-content:space-between;
  border-top:2px solid #111;
  padding-top:12px;
  margin-top:15px;
  font-weight:bold;
}

.category{
  display:inline-block;
  border:1px solid #999;
  padding:3px 7px;
  font-size:9px;
  margin-top:5px;
}

.weekHead{
  display:grid;
  grid-template-columns:repeat(7,1fr);
  gap:3px;
  margin-bottom:5px;
}

.dayHead{
  background:#111;
  color:#fff;
  padding:9px 3px;
  text-align:center;
  font-size:10px;
}

.week{
  display:grid;
  grid-template-columns:repeat(7,1fr);
  gap:3px;
}

.day{
  min-height:230px;
  background:var(--paper);
  padding:8px;
  box-shadow:0 2px 7px #0001;
}

.dayNumber{
  font-weight:bold;
  border-bottom:1px dashed #aaa;
  padding-bottom:6px;
  margin-bottom:5px;
  font-size:12px;
}

.event{
  font-size:9px;
  padding:6px;
  margin:4px 0;
  background:#eeeae0;
  border-left:3px solid #111;
  overflow-wrap:anywhere;
}

.todoEvent{
  border-left-color:#777;
}

.formRow{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:10px;
}

.formRow3{
  display:grid;
  grid-template-columns:1fr 1fr 1fr;
  gap:10px;
}

.wishCard{
  border:1px dashed #999;
  padding:15px;
  margin-bottom:10px;
}

.photo{
  max-width:180px;
  max-height:180px;
  display:block;
  margin-top:10px;
  object-fit:cover;
}

.receipt{
  width:100%;
  max-width:550px;
  margin:auto;
  background:#fffdf7;
  padding:30px 25px;
  box-shadow:0 10px 35px #0002;
}

.receiptHeader{
  text-align:center;
}

.receiptLine{
  border-top:1px dashed #888;
  margin:17px 0;
}

.receiptRow{
  display:flex;
  justify-content:space-between;
  gap:15px;
  padding:6px 0;
  font-size:11px;
}

.receiptTotal{
  display:flex;
  justify-content:space-between;
  font-weight:bold;
  border-top:2px solid #111;
  padding-top:12px;
  margin-top:8px;
}

.empty{
  color:#999;
  text-align:center;
  padding:25px 5px;
  font-size:11px;
}

.modal{
  position:fixed;
  inset:0;
  background:#0008;
  display:flex;
  justify-content:center;
  align-items:center;
  padding:15px;
  z-index:300;
}

.modalBox{
  background:var(--paper);
  width:100%;
  max-width:600px;
  max-height:90vh;
  overflow:auto;
  padding:25px;
}

.modalTop{
  display:flex;
  justify-content:space-between;
  margin-bottom:20px;
}

.close{
  border:0;
  background:none;
  font-size:20px;
}

.notice{
  padding:10px;
  background:#eeeae0;
  font-size:10px;
  margin-bottom:15px;
}

@media(max-width:750px){

  .grid{
    grid-template-columns:1fr;
  }

  .formRow,
  .formRow3{
    grid-template-columns:1fr;
  }

  .week{
    overflow-x:auto;
    grid-template-columns:repeat(7,145px);
  }

  .weekHead{
    overflow-x:auto;
    grid-template-columns:repeat(7,145px);
  }

  .day{
    min-height:250px;
  }

  .page{
    padding:20px 10px 60px;
  }

}

@media print{

  body{
    background:white;
  }

  header,
  .noPrint,
  .pageTitle{
    display:none!important;
  }

  #app{
    display:block!important;
  }

  .page{
    display:none!important;
  }

  #todayPage{
    display:block!important;
    padding:0;
  }

  .receipt{
    box-shadow:none;
    max-width:none;
  }
}
</style>
</head>

<body>

<!-- LOGIN -->

<div id="loginScreen">

  <div class="loginPaper">

    <div class="logo">DAILY RECEIPT</div>

    <div class="sub">
      YOUR LIFE, ONE RECEIPT AT A TIME.
    </div>

    <input
      id="loginEmail"
      type="email"
      placeholder="EMAIL"
    >

    <br><br>

    <input
      id="loginPassword"
      type="password"
      placeholder="PASSWORD"
    >

    <br><br>

    <button class="primary" onclick="login()">
      LOGIN
    </button>

    <br><br>

    <button
      class="secondary"
      onclick="signup()"
    >
      CREATE ACCOUNT
    </button>

    <p id="loginMessage" class="small"></p>

  </div>

</div>


<!-- APP -->

<div id="app" class="hidden">

<header>

  <nav class="nav">

    <div class="navLogo">
      DAILY RECEIPT
    </div>

    <button onclick="showPage('homePage',this)" class="active">
      HOME
    </button>

    <button onclick="showPage('schedulePage',this)">
      SCHEDULE
    </button>

    <button onclick="showPage('todoPage',this)">
      TODO
    </button>

    <button onclick="showPage('moneyPage',this)">
      MONEY
    </button>

    <button onclick="showPage('wishPage',this)">
      WISH
    </button>

    <button onclick="showPage('memoPage',this)">
      MEMO
    </button>

    <button onclick="showPage('studyPage',this)">
      STUDY
    </button>

    <button onclick="showPage('todayPage',this)">
      TODAY
    </button>

    <button class="logout" onclick="logout()">
      LOGOUT
    </button>

  </nav>

</header>


<!-- HOME -->

<section id="homePage" class="page">

  <div class="center">

    <div class="small">
      DAILY RECEIPT
    </div>

    <div class="pageTitle" id="homeDate"></div>

  </div>

  <div class="grid">

    <div class="card">

      <div class="cardTitle">
        UPCOMING SCHEDULE
      </div>

      <div id="homeSchedule"></div>

    </div>


    <div class="card">

      <div class="cardTitle">
        TODAY'S TODO
      </div>

      <div id="homeTodo"></div>

    </div>


    <div class="card">

      <div class="cardTitle">
        TODAY'S MONEY
      </div>

      <div
        id="homeMoney"
        class="moneyBig"
      >
        ¥0
      </div>

    </div>


    <div class="card">

      <div class="cardTitle">
        TODAY'S STUDY
      </div>

      <div
        id="homeStudy"
        class="moneyBig"
      >
        00:00
      </div>

    </div>

  </div>

</section>


<!-- SCHEDULE -->

<section id="schedulePage" class="page hidden">

  <div class="pageTitle">
    SCHEDULE
  </div>

  <div class="card">

    <div class="formRow">

      <div>
        <label class="small">TITLE</label>
        <input id="scheduleTitle" placeholder="予定">
      </div>

      <div>
        <label class="small">DATE</label>
        <input id="scheduleDate" type="date">
      </div>

    </div>

    <div class="formRow">

      <div>
        <label class="small">START</label>
        <input id="scheduleStart" type="time">
      </div>

      <div>
        <label class="small">END</label>
        <input id="scheduleEnd" type="time">
      </div>

    </div>

    <label class="small">
      <input id="scheduleAllDay" type="checkbox">
      ALL DAY
    </label>

    <br><br>

    <textarea
      id="scheduleMemo"
      placeholder="詳細・メモ"
    ></textarea>

    <button
      class="primary"
      onclick="addSchedule()"
    >
      ADD SCHEDULE
    </button>

  </div>

  <br>

  <div class="card">

    <div class="cardTitle">
      WEEK
    </div>

    <div class="formRow">

      <button
        class="secondary"
        onclick="changeWeek(-7)"
      >
        ← PREVIOUS
      </button>

      <button
        class="secondary"
        onclick="changeWeek(7)"
      >
        NEXT →
      </button>

    </div>

    <br>

    <div id="weekArea"></div>

  </div>

  <br>

  <div class="card">

    <div class="cardTitle">
      ALL SCHEDULE
    </div>

    <div id="scheduleList"></div>

  </div>

</section>


<!-- TODO -->

<section id="todoPage" class="page hidden">

  <div class="pageTitle">
    TODO
  </div>

  <div class="card">

    <div class="formRow">

      <div>
        <label class="small">TODO</label>
        <input id="todoTitle" placeholder="やること">
      </div>

      <div>
        <label class="small">DATE</label>
        <input id="todoDate" type="date">
      </div>

    </div>

    <div class="formRow">

      <div>
        <label class="small">TIME</label>
        <input id="todoTime" type="time">
      </div>

      <div>
        <label class="small">CATEGORY</label>
        <input id="todoCategory" placeholder="学校 / 仕事 / 私生活">
      </div>

    </div>

    <label class="small">
      <input id="todoAllDay" type="checkbox" checked>
      ALL DAY
    </label>

    <br><br>

    <textarea
      id="todoMemo"
      placeholder="詳細・メモ"
    ></textarea>

    <button
      class="primary"
      onclick="addTodo()"
    >
      ADD TODO
    </button>

  </div>

  <br>

  <div class="card">

    <div class="cardTitle">
      TODO LIST
    </div>

    <div id="todoList"></div>

  </div>

</section>


<!-- MONEY -->

<section id="moneyPage" class="page hidden">

  <div class="pageTitle">
    MONEY
  </div>

  <div class="card">

    <div class="formRow">

      <div>
        <label class="small">ITEM</label>
        <input id="moneyTitle" placeholder="購入したもの">
      </div>

      <div>
        <label class="small">AMOUNT</label>
        <input id="moneyAmount" type="number" placeholder="0">
      </div>

    </div>

    <div class="formRow3">

      <div>
        <label class="small">DATE</label>
        <input id="moneyDate" type="date">
      </div>

      <div>
        <label class="small">CATEGORY</label>
        <select id="moneyCategory">
          <option>食費</option>
          <option>交通</option>
          <option>日用品</option>
          <option>趣味</option>
          <option>勉強</option>
          <option>美容</option>
          <option>その他</option>
        </select>
      </div>

      <div>
        <label class="small">MEMO</label>
        <input id="moneyMemo" placeholder="メモ">
      </div>

    </div>

    <button
      class="primary"
      onclick="addMoney()"
    >
      ADD EXPENSE
    </button>

  </div>

  <br>

  <div class="card">

    <div class="cardTitle">
      EXPENSES
    </div>

    <div id="moneyList"></div>

    <div class="total">

      <span>TOTAL</span>

      <span id="moneyTotal">
        ¥0
      </span>

    </div>

  </div>

</section>


<!-- WISH -->

<section id="wishPage" class="page hidden">

  <div class="pageTitle">
    WISH
  </div>

  <div class="card">

    <div class="formRow">

      <div>
        <label class="small">ITEM</label>
        <input id="wishTitle" placeholder="欲しいもの">
      </div>

      <div>
        <label class="small">PRICE</label>
        <input id="wishPrice" type="number" placeholder="0">
      </div>

    </div>

    <div class="formRow">

      <div>
        <label class="small">CATEGORY</label>
        <input id="wishCategory" placeholder="服 / 本 / 家電 / etc.">
      </div>

      <div>
        <label class="small">URL</label>
        <input id="wishUrl" placeholder="https://...">
      </div>

    </div>

    <textarea
      id="wishMemo"
      placeholder="メモ"
    ></textarea>

    <button
      class="primary"
      onclick="addWish()"
    >
      ADD WISH
    </button>

  </div>

  <br>

  <div id="wishList"></div>

</section>


<!-- MEMO -->

<section id="memoPage" class="page hidden">

  <div class="pageTitle">
    MEMO
  </div>

  <div class="card">

    <input
      id="memoTitle"
      placeholder="タイトル"
    >

    <br><br>

    <textarea
      id="memoContent"
      placeholder="自由にメモ..."
    ></textarea>

    <button
      class="primary"
      onclick="addMemo()"
    >
      SAVE MEMO
    </button>

  </div>

  <br>

  <div id="memoList"></div>

</section>


<!-- STUDY -->

<section id="studyPage" class="page hidden">

  <div class="pageTitle">
    STUDY
  </div>

  <div class="card">

    <div class="formRow">

      <div>
        <label class="small">SUBJECT</label>
        <input id="studySubject" placeholder="英語 / 数学 etc.">
      </div>

      <div>
        <label class="small">DATE</label>
        <input id="studyDate" type="date">
      </div>

    </div>

    <div class="formRow">

      <div>
        <label class="small">MINUTES</label>
        <input id="studyMinutes" type="number" placeholder="60">
      </div>

      <div>
        <label class="small">PHOTO</label>
        <input
          id="studyPhoto"
          type="file"
          accept="image/*"
        >
      </div>

    </div>

    <textarea
      id="studyMemo"
      placeholder="勉強した内容..."
    ></textarea>

    <button
      class="primary"
      onclick="addStudy()"
    >
      SAVE STUDY
    </button>

  </div>

  <br>

  <div class="card">

    <div class="cardTitle">
      STUDY LOG
    </div>

    <div id="studyList"></div>

    <div class="total">

      <span>TOTAL</span>

      <span id="studyTotal">
        00:00
      </span>

    </div>

  </div>

</section>


<!-- TODAY -->

<section id="todayPage" class="page hidden">

  <div class="pageTitle center">
    TODAY
  </div>

  <div class="receipt">

    <div class="receiptHeader">

      <div class="small">
        DAILY RECEIPT
      </div>

      <h2 id="receiptDate"></h2>

      <div class="small">
        DAILY SUMMARY
      </div>

    </div>

    <div class="receiptLine"></div>

    <b>SCHEDULE</b>

    <div id="receiptSchedule"></div>

    <div class="receiptLine"></div>

    <b>TODO</b>

    <div id="receiptTodo"></div>

    <div class="receiptLine"></div>

    <b>MONEY</b>

    <div id="receiptMoney"></div>

    <div class="receiptTotal">

      <span>TOTAL</span>

      <span id="receiptMoneyTotal">
        ¥0
      </span>

    </div>

    <div class="receiptLine"></div>

    <b>STUDY</b>

    <div id="receiptStudy"></div>

    <div class="receiptLine"></div>

    <b>MEMO</b>

    <textarea
      id="todayMemo"
      placeholder="今日のまとめ..."
      onchange="saveTodayMemo()"
    ></textarea>

    <div class="receiptLine"></div>

    <div class="receiptHeader small">

      THANK YOU<br><br>
      SEE YOU TOMORROW

    </div>

    <br>

    <button
      class="primary noPrint"
      onclick="window.print()"
    >
      SAVE AS PDF
    </button>

  </div>

</section>

</div>


<!-- MODAL -->

<div
  id="editModal"
  class="modal hidden"
>

  <div class="modalBox">

    <div class="modalTop">

      <b id="modalTitle">
        EDIT
      </b>

      <button
        class="close"
        onclick="closeModal()"
      >
        ×
      </button>

    </div>

    <div id="modalContent"></div>

  </div>

</div>


<script>

/* =========================================================
   SUPABASE SETTINGS
   ↓↓↓ ここだけ自分のSupabaseに変更 ↓↓↓
========================================================= */

const SUPABASE_URL =
"YOUR_SUPABASE_URL";

const SUPABASE_ANON_KEY =
"YOUR_SUPABASE_ANON_KEY";


const supabaseClient =
window.supabase.createClient(
  SUPABASE_URL,
  SUPABASE_ANON_KEY
);


/* =========================================================
   DATA
========================================================= */

let data = {

  schedules:[],
  todos:[],
  expenses:[],
  wishes:[],
  memos:[],
  studies:[],
  todayMemos:{}

};

let weekOffset = 0;


/* =========================================================
   UTIL
========================================================= */

function uid(){

  return Date.now().toString(36)
    +Math.random().toString(36).slice(2);

}


function today(){

  const d = new Date();

  return d.getFullYear()
    +"-"
    +String(d.getMonth()+1).padStart(2,"0")
    +"-"
    +String(d.getDate()).padStart(2,"0");

}


function escapeHTML(value){

  return String(value ?? "")
    .replace(/&/g,"&amp;")
    .replace(/</g,"&lt;")
    .replace(/>/g,"&gt;")
    .replace(/"/g,"&quot;")
    .replace(/'/g,"&#039;");

}


function yen(n){

  return "¥"
    +Number(n||0)
      .toLocaleString("ja-JP");

}


function minutesText(minutes){

  minutes = Number(minutes||0);

  const h =
    Math.floor(minutes/60);

  const m =
    minutes%60;

  return String(h).padStart(2,"0")
    +":"
    +String(m).padStart(2,"0");

}


function formatDate(date){

  if(!date)return "";

  const d =
    new Date(date+"T00:00:00");

  return d.getMonth()+1
    +"/"
    +d.getDate();

}


function formatJP(date){

  const d =
    new Date(date+"T00:00:00");

  const week =
    ["日","月","火","水","木","金","土"];

  return d.getFullYear()
    +" / "
    +String(d.getMonth()+1).padStart(2,"0")
    +" / "
    +String(d.getDate()).padStart(2,"0")
    +" ("
    +week[d.getDay()]
    +")";

}


function dateAdd(date,days){

  const d =
    new Date(date+"T00:00:00");

  d.setDate(d.getDate()+days);

  return d.getFullYear()
    +"-"
    +String(d.getMonth()+1).padStart(2,"0")
    +"-"
    +String(d.getDate()).padStart(2,"0");

}


/* =========================================================
   AUTH
========================================================= */

async function login(){

  const email =
    document.getElementById("loginEmail").value.trim();

  const password =
    document.getElementById("loginPassword").value;

  const message =
    document.getElementById("loginMessage");

  if(!email || !password){

    message.textContent =
      "EMAILとPASSWORDを入力してください。";

    return;

  }

  const {error} =
    await supabaseClient.auth.signInWithPassword({
      email,
      password
    });

  if(error){

    message.textContent =
      error.message;

    return;

  }

  await openApp();

}


async function signup(){

  const email =
    document.getElementById("loginEmail").value.trim();

  const password =
    document.getElementById("loginPassword").value;

  const message =
    document.getElementById("loginMessage");

  if(!email || !password){

    message.textContent =
      "EMAILとPASSWORDを入力してください。";

    return;

  }

  if(password.length < 6){

    message.textContent =
      "PASSWORDは6文字以上にしてください。";

    return;

  }

  const {error} =
    await supabaseClient.auth.signUp({
      email,
      password
    });

  if(error){

    message.textContent =
      error.message;

    return;

  }

  message.textContent =
    "登録しました。メール確認が必要な設定の場合はメールを確認してください。";

}


async function logout(){

  await supabaseClient.auth.signOut();

  document.getElementById("app")
    .classList.add("hidden");

  document.getElementById("loginScreen")
    .classList.remove("hidden");

}


async function currentUser(){

  const {
    data:{user}
  } =
    await supabaseClient.auth.getUser();

  return user;

}


/* =========================================================
   SUPABASE DATA
========================================================= */

async function loadData(){

  const user =
    await currentUser();

  if(!user)return;

  const {data:row,error} =
    await supabaseClient
      .from("user_data")
      .select("data")
      .eq("user_id",user.id)
      .maybeSingle();

  if(error){

    console.error(error);

    alert(
      "Supabaseのuser_dataテーブルを確認してください。"
    );

    return;

  }

  if(row && row.data){

    data = {
      schedules:[],
      todos:[],
      expenses:[],
      wishes:[],
      memos:[],
      studies:[],
      todayMemos:{},
      ...row.data
    };

  }else{

    data = {
      schedules:[],
      todos:[],
      expenses:[],
      wishes:[],
      memos:[],
      studies:[],
      todayMemos:{}
    };

    await saveData();

  }

}


async function saveData(){

  const user =
    await currentUser();

  if(!user)return;

  const {error} =
    await supabaseClient
      .from("user_data")
      .upsert(
        {
          user_id:user.id,
          data:data,
          updated_at:new Date().toISOString()
        },
        {
          onConflict:"user_id"
        }
      );

  if(error){

    console.error(error);

    alert(
      "データ保存に失敗しました。Supabaseの設定を確認してください。"
    );

  }

}


/* =========================================================
   APP START
========================================================= */

async function openApp(){

  document.getElementById("loginScreen")
    .classList.add("hidden");

  document.getElementById("app")
    .classList.remove("hidden");

  setDefaultDates();

  await loadData();

  renderAll();

}


async function checkSession(){

  const {
    data:{session}
  } =
    await supabaseClient.auth.getSession();

  if(session){

    await openApp();

  }

}


function setDefaultDates(){

  const d = today();

  document.getElementById("scheduleDate").value=d;
  document.getElementById("todoDate").value=d;
  document.getElementById("moneyDate").value=d;
  document.getElementById("studyDate").value=d;

  document.getElementById("homeDate").textContent=
    formatJP(d);

  document.getElementById("receiptDate").textContent=
    formatJP(d);

}


/* =========================================================
   NAVIGATION
========================================================= */

function showPage(id,button){

  document
    .querySelectorAll(".page")
    .forEach(p=>p.classList.add("hidden"));

  document
    .getElementById(id)
    .classList.remove("hidden");

  document
    .querySelectorAll(".nav button")
    .forEach(b=>b.classList.remove("active"));

  button.classList.add("active");

  renderAll();

  window.scrollTo({
    top:0,
    behavior:"smooth"
  });

}


/* =========================================================
   SCHEDULE
========================================================= */

async function addSchedule(){

  const title =
    document.getElementById("scheduleTitle").value.trim();

  const date =
    document.getElementById("scheduleDate").value;

  if(!title || !date){

    alert("タイトルと日付を入力してください。");

    return;

  }

  data.schedules.push({

    id:uid(),
    title,
    date,

    start:
      document.getElementById("scheduleStart").value,

    end:
      document.getElementById("scheduleEnd").value,

    allDay:
      document.getElementById("scheduleAllDay").checked,

    memo:
      document.getElementById("scheduleMemo").value

  });

  await saveData();

  document.getElementById("scheduleTitle").value="";
  document.getElementById("scheduleStart").value="";
  document.getElementById("scheduleEnd").value="";
  document.getElementById("scheduleMemo").value="";

  renderAll();

}


function deleteSchedule(id){

  if(!confirm("この予定を削除しますか？"))return;

  data.schedules =
    data.schedules.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


function editSchedule(id){

  const x =
    data.schedules.find(x=>x.id===id);

  if(!x)return;

  document.getElementById("modalTitle")
    .textContent="EDIT SCHEDULE";

  document.getElementById("modalContent")
    .innerHTML=`

      <input id="editScheduleTitle"
        value="${escapeHTML(x.title)}">

      <br><br>

      <input id="editScheduleDate"
        type="date"
        value="${x.date}">

      <br><br>

      <div class="formRow">

        <input id="editScheduleStart"
          type="time"
          value="${x.start||""}">

        <input id="editScheduleEnd"
          type="time"
          value="${x.end||""}">

      </div>

      <br>

      <label class="small">

        <input id="editScheduleAllDay"
          type="checkbox"
          ${x.allDay?"checked":""}>

        ALL DAY

      </label>

      <br><br>

      <textarea id="editScheduleMemo">${escapeHTML(x.memo||"")}</textarea>

      <br>

      <button class="primary"
        onclick="updateSchedule('${x.id}')">

        SAVE

      </button>
  `;

  document.getElementById("editModal")
    .classList.remove("hidden");

}


async function updateSchedule(id){

  const x =
    data.schedules.find(x=>x.id===id);

  if(!x)return;

  x.title =
    document.getElementById("editScheduleTitle").value;

  x.date =
    document.getElementById("editScheduleDate").value;

  x.start =
    document.getElementById("editScheduleStart").value;

  x.end =
    document.getElementById("editScheduleEnd").value;

  x.allDay =
    document.getElementById("editScheduleAllDay").checked;

  x.memo =
    document.getElementById("editScheduleMemo").value;

  await saveData();

  closeModal();
  renderAll();

}


/* =========================================================
   TODO
========================================================= */

async function addTodo(){

  const title =
    document.getElementById("todoTitle").value.trim();

  const date =
    document.getElementById("todoDate").value;

  if(!title || !date){

    alert("TODOと日付を入力してください。");

    return;

  }

  data.todos.push({

    id:uid(),
    title,
    date,

    time:
      document.getElementById("todoTime").value,

    allDay:
      document.getElementById("todoAllDay").checked,

    category:
      document.getElementById("todoCategory").value,

    memo:
      document.getElementById("todoMemo").value,

    done:false

  });

  await saveData();

  document.getElementById("todoTitle").value="";
  document.getElementById("todoTime").value="";
  document.getElementById("todoCategory").value="";
  document.getElementById("todoMemo").value="";

  renderAll();

}


async function toggleTodo(id){

  const x =
    data.todos.find(x=>x.id===id);

  if(!x)return;

  x.done=!x.done;

  await saveData();

  renderAll();

}


function deleteTodo(id){

  if(!confirm("このTODOを削除しますか？"))return;

  data.todos =
    data.todos.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


function editTodo(id){

  const x =
    data.todos.find(x=>x.id===id);

  if(!x)return;

  document.getElementById("modalTitle")
    .textContent="EDIT TODO";

  document.getElementById("modalContent")
    .innerHTML=`

      <input id="editTodoTitle"
        value="${escapeHTML(x.title)}">

      <br><br>

      <input id="editTodoDate"
        type="date"
        value="${x.date}">

      <br><br>

      <input id="editTodoTime"
        type="time"
        value="${x.time||""}">

      <br><br>

      <label class="small">

        <input id="editTodoAllDay"
          type="checkbox"
          ${x.allDay?"checked":""}>

        ALL DAY

      </label>

      <br><br>

      <input id="editTodoCategory"
        value="${escapeHTML(x.category||"")}"
        placeholder="CATEGORY">

      <br><br>

      <textarea id="editTodoMemo">${escapeHTML(x.memo||"")}</textarea>

      <button class="primary"
        onclick="updateTodo('${x.id}')">

        SAVE

      </button>

  `;

  document.getElementById("editModal")
    .classList.remove("hidden");

}


async function updateTodo(id){

  const x =
    data.todos.find(x=>x.id===id);

  if(!x)return;

  x.title =
    document.getElementById("editTodoTitle").value;

  x.date =
    document.getElementById("editTodoDate").value;

  x.time =
    document.getElementById("editTodoTime").value;

  x.allDay =
    document.getElementById("editTodoAllDay").checked;

  x.category =
    document.getElementById("editTodoCategory").value;

  x.memo =
    document.getElementById("editTodoMemo").value;

  await saveData();

  closeModal();
  renderAll();

}


/* =========================================================
   MONEY
========================================================= */

async function addMoney(){

  const title =
    document.getElementById("moneyTitle").value.trim();

  const amount =
    Number(document.getElementById("moneyAmount").value);

  const date =
    document.getElementById("moneyDate").value;

  if(!title || !amount || !date){

    alert("商品・金額・日付を入力してください。");

    return;

  }

  data.expenses.push({

    id:uid(),
    title,
    amount,
    date,

    category:
      document.getElementById("moneyCategory").value,

    memo:
      document.getElementById("moneyMemo").value

  });

  await saveData();

  document.getElementById("moneyTitle").value="";
  document.getElementById("moneyAmount").value="";
  document.getElementById("moneyMemo").value="";

  renderAll();

}


function deleteMoney(id){

  if(!confirm("この支出を削除しますか？"))return;

  data.expenses =
    data.expenses.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


/* =========================================================
   WISH
========================================================= */

async function addWish(){

  const title =
    document.getElementById("wishTitle").value.trim();

  if(!title){

    alert("欲しいものを入力してください。");

    return;

  }

  data.wishes.push({

    id:uid(),
    title,

    price:
      Number(document.getElementById("wishPrice").value)||0,

    category:
      document.getElementById("wishCategory").value
      ||"その他",

    url:
      document.getElementById("wishUrl").value,

    memo:
      document.getElementById("wishMemo").value,

    bought:false

  });

  await saveData();

  document.getElementById("wishTitle").value="";
  document.getElementById("wishPrice").value="";
  document.getElementById("wishCategory").value="";
  document.getElementById("wishUrl").value="";
  document.getElementById("wishMemo").value="";

  renderAll();

}


async function toggleWish(id){

  const x =
    data.wishes.find(x=>x.id===id);

  if(!x)return;

  x.bought=!x.bought;

  await saveData();
  renderAll();

}


function deleteWish(id){

  data.wishes =
    data.wishes.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


/* =========================================================
   MEMO
========================================================= */

async function addMemo(){

  const title =
    document.getElementById("memoTitle").value.trim();

  const content =
    document.getElementById("memoContent").value;

  if(!title && !content){

    alert("メモを入力してください。");

    return;

  }

  data.memos.push({

    id:uid(),
    title:title||"UNTITLED",
    content,
    createdAt:new Date().toISOString()

  });

  await saveData();

  document.getElementById("memoTitle").value="";
  document.getElementById("memoContent").value="";

  renderAll();

}


function deleteMemo(id){

  data.memos =
    data.memos.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


/* =========================================================
   STUDY
========================================================= */

async function compressImage(file){

  return new Promise((resolve,reject)=>{

    const reader =
      new FileReader();

    reader.onload=()=>{

      const img =
        new Image();

      img.onload=()=>{

        const max=900;

        let width=img.width;
        let height=img.height;

        if(width>max){

          height =
            height*max/width;

          width=max;

        }

        const canvas =
          document.createElement("canvas");

        canvas.width=width;
        canvas.height=height;

        const ctx =
          canvas.getContext("2d");

        ctx.drawImage(
          img,
          0,
          0,
          width,
          height
        );

        resolve(
          canvas.toDataURL(
            "image/jpeg",
            .72
          )
        );

      };

      img.onerror=reject;

      img.src=reader.result;

    };

    reader.onerror=reject;

    reader.readAsDataURL(file);

  });

}


async function addStudy(){

  const subject =
    document.getElementById("studySubject")
      .value.trim();

  const date =
    document.getElementById("studyDate")
      .value;

  const minutes =
    Number(
      document.getElementById("studyMinutes")
        .value
    );

  if(!subject || !date || !minutes){

    alert(
      "科目・日付・勉強時間を入力してください。"
    );

    return;

  }

  let photo="";

  const file =
    document.getElementById("studyPhoto")
      .files[0];

  if(file){

    try{

      photo =
        await compressImage(file);

    }catch(e){

      alert("写真の読み込みに失敗しました。");

    }

  }

  data.studies.push({

    id:uid(),
    subject,
    date,
    minutes,

    memo:
      document.getElementById("studyMemo").value,

    photo

  });

  await saveData();

  document.getElementById("studySubject").value="";
  document.getElementById("studyMinutes").value="";
  document.getElementById("studyMemo").value="";
  document.getElementById("studyPhoto").value="";

  renderAll();

}


function deleteStudy(id){

  if(!confirm("この勉強記録を削除しますか？"))return;

  data.studies =
    data.studies.filter(x=>x.id!==id);

  saveData();
  renderAll();

}


/* =========================================================
   TODAY MEMO
========================================================= */

async function saveTodayMemo(){

  data.todayMemos[today()] =
    document.getElementById("todayMemo").value;

  await saveData();

}


/* =========================================================
   WEEK
========================================================= */

function changeWeek(amount){

  weekOffset += amount;

  renderWeek();

}


function getMonday(date){

  const d =
    new Date(date+"T00:00:00");

  const day=d.getDay();

  const diff =
    day===0 ? -6 : 1-day;

  d.setDate(d.getDate()+diff);

  return d.getFullYear()
    +"-"
    +String(d.getMonth()+1).padStart(2,"0")
    +"-"
    +String(d.getDate()).padStart(2,"0");

}


function renderWeek(){

  const base =
    dateAdd(
      getMonday(today()),
      weekOffset
    );

  const days=[];

  for(let i=0;i<7;i++){

    days.push(
      dateAdd(base,i)
    );

  }

  const weekNames =
    ["MON","TUE","WED","THU","FRI","SAT","SUN"];

  let html=
    `<div class="weekHead">`;

  days.forEach((d,i)=>{

    html+=`
      <div class="dayHead">
        ${weekNames[i]}<br>
        ${formatDate(d)}
      </div>
    `;

  });

  html+=`</div><div class="week">`;

  days.forEach(d=>{

    const schedules =
      data.schedules
        .filter(x=>x.date===d);

    const todos =
      data.todos
        .filter(x=>x.date===d);

    html+=`

      <div class="day">

        <div class="dayNumber">
          ${formatDate(d)}
        </div>

    `;

    schedules.forEach(x=>{

      html+=`

        <div class="event">

          <b>
            ${x.allDay
              ?"ALL DAY"
              :(x.start||"")+
                (x.end?" - "+x.end:"")
            }
          </b>

          <br>

          ${escapeHTML(x.title)}

        </div>

      `;

    });

    todos.forEach(x=>{

      html+=`

        <div class="event todoEvent">

          ${x.done?"✓":"□"}
          ${escapeHTML(x.title)}

        </div>

      `;

    });

    if(!schedules.length && !todos.length){

      html+=`
        <div class="small">
          —
        </div>
      `;

    }

    html+=`</div>`;

  });

  html+=`</div>`;

  document.getElementById("weekArea")
    .innerHTML=html;

}


/* =========================================================
   RENDER HOME
========================================================= */

function renderHome(){

  const d=today();

  document.getElementById("homeDate")
    .textContent=formatJP(d);

  const schedules =
    data.schedules
      .filter(x=>x.date>=d)
      .sort((a,b)=>
        (a.date+(a.start||""))
          .localeCompare(
            b.date+(b.start||"")
          )
      )
      .slice(0,7);

  let sh="";

  schedules.forEach(x=>{

    sh+=`

      <div class="item">

        <div class="itemMain">

          <div class="itemTitle">
            ${escapeHTML(x.title)}
          </div>

          <div class="itemSub">
            ${formatDate(x.date)}
            /
            ${x.allDay
              ?"ALL DAY"
              :(x.start||"")
            }
          </div>

        </div>

      </div>

    `;

  });

  document.getElementById("homeSchedule")
    .innerHTML=
      sh||
      `<div class="empty">NO UPCOMING SCHEDULE</div>`;


  const todos =
    data.todos
      .filter(x=>x.date===d)
      .sort((a,b)=>Number(a.done)-Number(b.done));

  let th="";

  todos.slice(0,7).forEach(x=>{

    th+=`

      <div class="item">

        <div>

          <button
            class="check"
            onclick="toggleTodo('${x.id}')"
          >
            ${x.done?"✓":""}
          </button>

          <span class="${x.done?"checked":""}">
            ${escapeHTML(x.title)}
          </span>

        </div>

      </div>

    `;

  });

  document.getElementById("homeTodo")
    .innerHTML=
      th||
      `<div class="empty">NO TODO TODAY</div>`;


  const money =
    data.expenses
      .filter(x=>x.date===d)
      .reduce(
        (sum,x)=>sum+Number(x.amount),
        0
      );

  document.getElementById("homeMoney")
    .textContent=yen(money);


  const study =
    data.studies
      .filter(x=>x.date===d)
      .reduce(
        (sum,x)=>sum+Number(x.minutes),
        0
      );

  document.getElementById("homeStudy")
    .textContent=minutesText(study);

}


/* =========================================================
   RENDER SCHEDULE
========================================================= */

function renderSchedules(){

  const list =
    [...data.schedules]
      .sort((a,b)=>
        (a.date+(a.start||""))
          .localeCompare(
            b.date+(b.start||"")
          )
      );

  let html="";

  list.forEach(x=>{

    html+=`

      <div class="item">

        <div class="itemMain">

          <div class="itemTitle">
            ${escapeHTML(x.title)}
          </div>

          <div class="itemSub">

            ${formatDate(x.date)}
            /
            ${
              x.allDay
              ?"ALL DAY"
              :(x.start||"")
                +(x.end?" - "+x.end:"")
            }

          </div>

          ${
            x.memo
            ?`<div class="itemSub">
                ${escapeHTML(x.memo)}
              </div>`
            :""
          }

        </div>

        <div class="actions">

          <button
            class="iconBtn"
            onclick="editSchedule('${x.id}')"
          >
            EDIT
          </button>

          <button
            class="danger"
            onclick="deleteSchedule('${x.id}')"
          >
            DELETE
          </button>

        </div>

      </div>

    `;

  });

  document.getElementById("scheduleList")
    .innerHTML=
      html||
      `<div class="empty">NO SCHEDULE</div>`;

}


/* =========================================================
   RENDER TODO
========================================================= */

function renderTodos(){

  const list =
    [...data.todos]
      .sort((a,b)=>
        Number(a.done)-Number(b.done)
        ||
        a.date.localeCompare(b.date)
      );

  let html="";

  list.forEach(x=>{

    html+=`

      <div class="item">

        <div class="itemMain">

          <div class="itemTitle">

            <button
              class="check"
              onclick="toggleTodo('${x.id}')"
            >
              ${x.done?"✓":""}
            </button>

            <span class="${x.done?"checked":""}">
              ${escapeHTML(x.title)}
            </span>

          </div>

          <div class="itemSub">

            ${formatDate(x.date)}
            /
            ${
              x.allDay
              ?"ALL DAY"
              :(x.time||"")
            }

            ${
              x.category
              ?` / ${escapeHTML(x.category)}`
              :""
            }

          </div>

          ${
            x.memo
            ?`<div class="itemSub">
                ${escapeHTML(x.memo)}
              </div>`
            :""
          }

        </div>

        <div class="actions">

          <button
            class="iconBtn"
            onclick="editTodo('${x.id}')"
          >
            EDIT
          </button>

          <button
            class="danger"
            onclick="deleteTodo('${x.id}')"
          >
            DELETE
          </button>

        </div>

      </div>

    `;

  });

  document.getElementById("todoList")
    .innerHTML=
      html||
      `<div class="empty">NO TODO</div>`;

}


/* =========================================================
   RENDER MONEY
========================================================= */

function renderMoney(){

  const list =
    [...data.expenses]
      .sort((a,b)=>
        b.date.localeCompare(a.date)
      );

  let html="";
  let total=0;

  list.forEach(x=>{

    total+=Number(x.amount);

    html+=`

      <div class="item">

        <div class="itemMain">

          <div class="itemTitle">
            ${escapeHTML(x.title)}
          </div>

          <div class="itemSub">

            ${formatDate(x.date)}
            /
            ${escapeHTML(x.category)}

          </div>

          ${
            x.memo
            ?`<div class="itemSub">
                ${escapeHTML(x.memo)}
              </div>`
            :""
          }

        </div>

        <div>

          <b>
            ${yen(x.amount)}
          </b>

          <button
            class="danger"
            onclick="deleteMoney('${x.id}')"
          >
            ×
          </button>

        </div>

      </div>

    `;

  });

  document.getElementById("moneyList")
    .innerHTML=
      html||
      `<div class="empty">NO EXPENSE</div>`;

  document.getElementById("moneyTotal")
    .textContent=yen(total);

}


/* =========================================================
   RENDER WISH
========================================================= */

function renderWish(){

  const groups={};

  data.wishes.forEach(x=>{

    const cat=x.category||"その他";

    if(!groups[cat]){
      groups[cat]=[];
    }

    groups[cat].push(x);

  });

  let html="";

  Object.keys(groups)
    .sort()
    .forEach(cat=>{

      html+=`

        <div class="card" style="margin-bottom:20px">

          <div class="cardTitle">
            ${escapeHTML(cat)}
          </div>

      `;

      groups[cat].forEach(x=>{

        html+=`

          <div class="wishCard">

            <div class="item">

              <div class="itemMain">

                <div class="itemTitle
                  ${x.bought?"checked":""}">

                  <button
                    class="check"
                    onclick="toggleWish('${x.id}')"
                  >
                    ${x.bought?"✓":""}
                  </button>

                  ${escapeHTML(x.title)}

                </div>

                ${
                  x.price
                  ?`<div class="itemSub">
                      ${yen(x.price)}
                    </div>`
                  :""
                }

                ${
                  x.memo
                  ?`<div class="itemSub">
                      ${escapeHTML(x.memo)}
                    </div>`
                  :""
                }

                ${
                  x.url
                  ?`<div class="itemSub">
                      <a
                        href="${escapeHTML(x.url)}"
                        target="_blank"
                        rel="noopener"
                      >
                        OPEN LINK
                      </a>
                    </div>`
                  :""
                }

              </div>

              <button
                class="danger"
                onclick="deleteWish('${x.id}')"
              >
                DELETE
              </button>

            </div>

          </div>

        `;

      });

      html+=`</div>`;

    });

  document.getElementById("wishList")
    .innerHTML=
      html||
      `<div class="card empty">NO WISH</div>`;

}


/* =========================================================
   RENDER MEMO
========================================================= */

function renderMemos(){

  const list =
    [...data.memos]
      .sort((a,b)=>
        String(b.createdAt)
          .localeCompare(
            String(a.createdAt)
          )
      );

  let html="";

  list.forEach(x=>{

    html+=`

      <div class="card" style="margin-bottom:18px">

        <div class="item">

          <div class="itemMain">

            <div class="itemTitle">
              ${escapeHTML(x.title)}
            </div>

            <div class="itemSub">
              ${
                x.createdAt
                ?new Date(x.createdAt)
                  .toLocaleDateString("ja-JP")
                :""
              }
            </div>

          </div>

          <button
            class="danger"
            onclick="deleteMemo('${x.id}')"
          >
            DELETE
          </button>

        </div>

        <div
          style="white-space:pre-wrap;margin-top:15px"
        >
          ${escapeHTML(x.content)}
        </div>

      </div>

    `;

  });

  document.getElementById("memoList")
    .innerHTML=
      html||
      `<div class="empty">NO MEMO</div>`;

}


/* =========================================================
   RENDER STUDY
========================================================= */

function renderStudy(){

  const list =
    [...data.studies]
      .sort((a,b)=>
        b.date.localeCompare(a.date)
      );

  let html="";
  let total=0;

  list.forEach(x=>{

    total+=Number(x.minutes);

    html+=`

      <div class="item">

        <div class="itemMain">

          <div class="itemTitle">
            ${escapeHTML(x.subject)}
          </div>

          <div class="itemSub">

            ${formatDate(x.date)}
            /
            ${minutesText(x.minutes)}

          </div>

          ${
            x.memo
            ?`<div class="itemSub">
                ${escapeHTML(x.memo)}
              </div>`
            :""
          }

          ${
            x.photo
            ?`<img
                class="photo"
                src="${x.photo}"
                alt="study photo"
              >`
            :""
          }

        </div>

        <button
          class="danger"
          onclick="deleteStudy('${x.id}')"
        >
          DELETE
        </button>

      </div>

    `;

  });

  document.getElementById("studyList")
    .innerHTML=
      html||
      `<div class="empty">NO STUDY LOG</div>`;

  document.getElementById("studyTotal")
    .textContent=
      minutesText(total);

}


/* =========================================================
   RENDER TODAY
========================================================= */

function renderToday(){

  const d=today();

  document.getElementById("receiptDate")
    .textContent=formatJP(d);

  const schedules =
    data.schedules
      .filter(x=>x.date===d);

  let sh="";

  schedules.forEach(x=>{

    sh+=`

      <div class="receiptRow">

        <span>
          ${
            x.allDay
            ?"ALL DAY"
            :(x.start||"")
          }
        </span>

        <span>
          ${escapeHTML(x.title)}
        </span>

      </div>

    `;

  });

  document.getElementById("receiptSchedule")
    .innerHTML=
      sh||
      `<div class="small">NO SCHEDULE</div>`;


  const todos =
    data.todos
      .filter(x=>x.date===d);

  let th="";

  todos.forEach(x=>{

    th+=`

      <div class="receiptRow">

        <span>
          ${x.done?"✓":"□"}
        </span>

        <span>
          ${escapeHTML(x.title)}
        </span>

      </div>

    `;

  });

  document.getElementById("receiptTodo")
    .innerHTML=
      th||
      `<div class="small">NO TODO</div>`;


  const money =
    data.expenses
      .filter(x=>x.date===d);

  let mh="";
  let total=0;

  money.forEach(x=>{

    total+=Number(x.amount);

    mh+=`

      <div class="receiptRow">

        <span>
          ${escapeHTML(x.title)}
        </span>

        <span>
          ${yen(x.amount)}
        </span>

      </div>

    `;

  });

  document.getElementById("receiptMoney")
    .innerHTML=
      mh||
      `<div class="small">NO EXPENSE</div>`;

  document.getElementById("receiptMoneyTotal")
    .textContent=yen(total);


  const studies =
    data.studies
      .filter(x=>x.date===d);

  let sth="";
  let studyTotal=0;

  studies.forEach(x=>{

    studyTotal+=Number(x.minutes);

    sth+=`

      <div class="receiptRow">

        <span>
          ${escapeHTML(x.subject)}
        </span>

        <span>
          ${minutesText(x.minutes)}
        </span>

      </div>

    `;

  });

  sth+=`

    <div class="receiptTotal">

      <span>STUDY TOTAL</span>

      <span>
        ${minutesText(studyTotal)}
      </span>

    </div>

  `;

  document.getElementById("receiptStudy")
    .innerHTML=sth;

  document.getElementById("todayMemo")
    .value=
      data.todayMemos[d]||"";

}


/* =========================================================
   RENDER ALL
========================================================= */

function renderAll(){

  renderHome();
  renderSchedules();
  renderTodos();
  renderMoney();
  renderWish();
  renderMemos();
  renderStudy();
  renderToday();
  renderWeek();

}


/* =========================================================
   MODAL
========================================================= */

function closeModal(){

  document.getElementById("editModal")
    .classList.add("hidden");

}


/* =========================================================
   ESC KEY
========================================================= */

document.addEventListener(
  "keydown",
  e=>{

    if(e.key==="Escape"){
      closeModal();
    }

  }
);


/* =========================================================
   START
========================================================= */

checkSession();

</script>

<!--
============================================================
SUPABASE SETUP
============================================================

SupabaseのSQL Editorで、最初にこれを1回だけ実行してください。

create table public.user_data (
  user_id uuid primary key references auth.users(id) on delete cascade,
  data jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.user_data enable row level security;

create policy "Users can read own data"
on public.user_data
for select
using (auth.uid() = user_id);

create policy "Users can insert own data"
on public.user_data
for insert
with check (auth.uid() = user_id);

create policy "Users can update own data"
on public.user_data
for update
using (auth.uid() = user_id)
with check (auth.uid() = user_id);

============================================================
============================================================
-->
</body>
</html>
