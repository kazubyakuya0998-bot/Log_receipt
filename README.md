# Log_receipt
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>DAILY RECEIPT</title>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<style>
*{box-sizing:border-box}

body{
margin:0;
background:#e9e6de;
color:#171717;
font-family:"Courier New",monospace;
}

button,input,textarea,select{
font-family:inherit;
}

button{
cursor:pointer;
}

.hidden{
display:none!important;
}

.paper{
background:#faf8f1;
box-shadow:0 10px 30px rgba(0,0,0,.12);
}

#login{
min-height:100vh;
display:flex;
align-items:center;
justify-content:center;
padding:20px;
}

.login-box{
width:100%;
max-width:400px;
padding:35px 25px;
background:#faf8f1;
box-shadow:0 10px 30px rgba(0,0,0,.12);
text-align:center;
}

.logo{
font-weight:bold;
letter-spacing:4px;
font-size:20px;
}

.small{
font-size:11px;
color:#777;
}

input,textarea,select{
width:100%;
border:1px solid #bbb;
background:#faf8f1;
padding:12px;
margin-bottom:10px;
outline:none;
}

textarea{
min-height:100px;
resize:vertical;
}

.black-button{
background:#111;
color:white;
border:0;
padding:13px;
width:100%;
font-weight:bold;
}

#app{
display:none;
}

header{
position:sticky;
top:0;
z-index:10;
background:#faf8f1;
border-bottom:1px solid #ccc;
}

.header-inner{
max-width:1200px;
margin:auto;
display:flex;
align-items:center;
gap:18px;
padding:14px 18px;
overflow-x:auto;
}

.nav-logo{
font-weight:bold;
white-space:nowrap;
margin-right:10px;
}

.nav-button{
border:0;
background:none;
white-space:nowrap;
font-size:11px;
color:#777;
}

.nav-button.active{
color:#111;
font-weight:bold;
text-decoration:underline;
text-underline-offset:5px;
}

.logout{
margin-left:auto;
}

.page{
max-width:1100px;
margin:auto;
padding:30px 18px 80px;
}

.page-title{
font-size:24px;
font-weight:bold;
margin-bottom:25px;
}

.grid{
display:grid;
grid-template-columns:repeat(2,1fr);
gap:20px;
}

.card{
background:#faf8f1;
padding:25px;
box-shadow:0 5px 20px rgba(0,0,0,.08);
position:relative;
}

.card:after{
content:"";
position:absolute;
left:0;
bottom:-7px;
width:100%;
height:14px;
background:linear-gradient(135deg,transparent 5px,#faf8f1 5px) 0 0/10px 10px repeat-x;
}

.line{
border-top:1px dashed #aaa;
margin:15px 0;
}

.row{
display:flex;
justify-content:space-between;
gap:15px;
padding:10px 0;
border-bottom:1px dashed #ccc;
font-size:12px;
}

.row:last-child{
border-bottom:0;
}

.delete{
color:#b00000;
border:0;
background:none;
font-size:11px;
}

.check{
border:0;
background:none;
font-size:18px;
}

.total{
display:flex;
justify-content:space-between;
font-weight:bold;
border-top:2px solid #111;
padding-top:12px;
margin-top:15px;
}

.receipt{
max-width:500px;
margin:auto;
background:#faf8f1;
padding:30px 25px;
box-shadow:0 10px 30px rgba(0,0,0,.12);
}

.center{
text-align:center;
}

.tabs{
display:flex;
gap:8px;
margin-bottom:20px;
flex-wrap:wrap;
}

.tab{
border:1px solid #999;
background:#faf8f1;
padding:8px 12px;
font-size:11px;
}

.tab.active{
background:#111;
color:white;
}

.photo{
max-width:150px;
margin-top:10px;
}

@media(max-width:700px){
.grid{
grid-template-columns:1fr;
}

.header-inner{
gap:12px;
}

.page{
padding:20px 10px 60px;
}

.card{
padding:18px;
}
}
</style>
</head>

<body>

<!-- LOGIN -->

<section id="login">

<div class="login-box">

<div class="logo">DAILY RECEIPT</div>

<p class="small">
YOUR LIFE, ONE RECEIPT AT A TIME.
</p>

<br>

<input id="email" type="email" placeholder="EMAIL">

<input id="password" type="password" placeholder="PASSWORD">

<button class="black-button" onclick="login()">
LOGIN
</button>

<br><br>

<button
class="small"
style="border:0;background:none"
onclick="register()"
>
CREATE ACCOUNT
</button>

<p id="login-error" class="small"></p>

</div>

</section>


<!-- APP -->

<section id="app">

<header>

<div class="header-inner">

<div class="nav-logo">
DAILY RECEIPT
</div>

<button class="nav-button active" onclick="showPage('home',this)">
⌂ HOME
</button>

<button class="nav-button" onclick="showPage('schedule',this)">
▣ SCHEDULE
</button>

<button class="nav-button" onclick="showPage('todo',this)">
□ TODO
</button>

<button class="nav-button" onclick="showPage('money',this)">
¥ MONEY
</button>

<button class="nav-button" onclick="showPage('wish',this)">
♡ WISH
</button>

<button class="nav-button" onclick="showPage('memo',this)">
≡ MEMO
</button>

<button class="nav-button" onclick="showPage('study',this)">
◷ STUDY
</button>

<button class="nav-button" onclick="showPage('today',this)">
▤ TODAY
</button>

<button class="nav-button logout" onclick="logout()">
LOGOUT
</button>

</div>

</header>


<!-- HOME -->

<div id="home" class="page">

<div class="center">

<div class="small">DAILY RECEIPT</div>

<div id="home-date" class="page-title"></div>

</div>

<div class="grid">

<div class="card">

<b>UPCOMING</b>

<div id="home-schedules"></div>

</div>


<div class="card">

<b>TODAY'S TODO</b>

<div id="home-todos"></div>

</div>


<div class="card">

<b>TODAY'S SPENDING</b>

<div
id="home-money"
style="font-size:30px;font-weight:bold;text-align:right;margin-top:30px"
>
¥0
</div>

</div>


<div class="card">

<b>STUDY</b>

<div
id="home-study"
style="font-size:30px;font-weight:bold;text-align:right;margin-top:30px"
>
00:00
</div>

</div>

</div>

</div>


<!-- SCHEDULE -->

<div id="schedule" class="page hidden">

<div class="page-title">
SCHEDULE
</div>

<div class="grid">

<div class="card">

<b>ADD SCHEDULE</b>

<br><br>

<input id="schedule-title" placeholder="TITLE">

<input id="schedule-date" type="date">

<label class="small">
<input id="schedule-all-day" type="checkbox">
ALL DAY
</label>

<input id="schedule-start" type="time">

<input id="schedule-end" type="time">

<textarea
id="schedule-description"
placeholder="DETAIL / MEMO"
></textarea>

<button class="black-button" onclick="addSchedule()">
ADD SCHEDULE
</button>

</div>


<div class="card">

<b>SCHEDULE LIST</b>

<div id="schedule-list"></div>

</div>

</div>

</div>


<!-- TODO -->

<div id="todo" class="page hidden">

<div class="page-title">
TODO
</div>

<div class="grid">

<div class="card">

<b>ADD TODO</b>

<br><br>

<input id="todo-title" placeholder="TODO">

<input id="todo-date" type="date">

<label class="small">
<input id="todo-all-day" type="checkbox" checked>
ALL DAY
</label>

<input id="todo-time" type="time">

<textarea
id="todo-description"
placeholder="DETAIL / MEMO"
></textarea>

<button class="black-button" onclick="addTodo()">
ADD TODO
</button>

</div>


<div class="card">

<b>TODO LIST</b>

<div id="todo-list"></div>

</div>

</div>

</div>


<!-- MONEY -->

<div id="money" class="page hidden">

<div class="page-title">
MONEY
</div>

<div class="grid">

<div class="card">

<b>ADD EXPENSE</b>

<br><br>

<input id="money-title" placeholder="ITEM">

<input id="money-amount" type="number" placeholder="AMOUNT">

<select id="money-category">

<option>食費</option>
<option>交通</option>
<option>日用品</option>
<option>趣味</option>
<option>勉強</option>
<option>その他</option>

</select>

<input id="money-date" type="date">

<input id="money-memo" placeholder="MEMO">

<button class="black-button" onclick="addMoney()">
ADD EXPENSE
</button>

</div>


<div class="card">

<b>MONEY LIST</b>

<div id="money-list"></div>

<div class="total">

<span>TOTAL</span>

<span id="money-total">
¥0
</span>

</div>

</div>

</div>

</div>


<!-- WISH -->

<div id="wish" class="page hidden">

<div class="page-title">
WISH
</div>

<div class="grid">

<div class="card">

<b>ADD WISH</b>

<br><br>

<input id="wish-title" placeholder="ITEM">

<input id="wish-price" type="number" placeholder="PRICE">

<input id="wish-category" placeholder="CATEGORY">

<input id="wish-url" placeholder="URL">

<textarea
id="wish-memo"
placeholder="MEMO"
></textarea>

<button class="black-button" onclick="addWish()">
ADD WISH
</button>

</div>


<div class="card">

<b>WISH LIST</b>

<div id="wish-list"></div>

</div>

</div>

</div>


<!-- MEMO -->

<div id="memo" class="page hidden">

<div class="page-title">
MEMO
</div>

<div class="grid">

<div class="card">

<b>NEW MEMO</b>

<br><br>

<input id="memo-title" placeholder="TITLE">

<textarea
id="memo-content"
placeholder="WRITE SOMETHING..."
></textarea>

<button class="black-button" onclick="addMemo()">
SAVE MEMO
</button>

</div>


<div>

<div id="memo-list"></div>

</div>

</div>

</div>


<!-- STUDY -->

<div id="study" class="page hidden">

<div class="page-title">
STUDY
</div>

<div class="grid">

<div class="card">

<b>ADD STUDY LOG</b>

<br><br>

<input id="study-subject" placeholder="SUBJECT">

<input id="study-date" type="date">

<input
id="study-minutes"
type="number"
placeholder="STUDY MINUTES"
>

<textarea
id="study-memo"
placeholder="WHAT DID YOU STUDY?"
></textarea>

<input
id="study-photo"
type="file"
accept="image/*"
>

<button class="black-button" onclick="addStudy()">
SAVE STUDY
</button>

</div>


<div class="card">

<b>STUDY LOG</b>

<div id="study-list"></div>

<div class="total">

<span>TOTAL</span>

<span id="study-total">
00:00
</span>

</div>

</div>

</div>

</div>


<!-- TODAY -->

<div id="today" class="page hidden">

<div class="page-title center">
TODAY
</div>

<div class="receipt">

<div class="center">

<div class="small">
DAILY RECEIPT
</div>

<h2 id="today-date"></h2>

</div>

<div class="line"></div>

<b>SCHEDULE</b>

<div id="today-schedule"></div>

<div class="line"></div>

<b>TODO</b>

<div id="today-todo"></div>

<div class="line"></div>

<b>MONEY</b>

<div id="today-money"></div>

<div class="total">

<span>TOTAL</span>

<span id="today-total">
¥0
</span>

</div>

<div class="line"></div>

<b>STUDY</b>

<div id="today-study"></div>

<div class="line"></div>

<b>MEMO</b>

<textarea
id="today-memo"
placeholder="TODAY WAS..."
></textarea>

<div class="line"></div>

<div class="center small">

THANK YOU<br>
SEE YOU TOMORROW

</div>

<br>

<button
class="black-button"
onclick="window.print()"
>
PRINT / SAVE AS PDF
</button>

</div>

</div>

</section>


<script>

/* =========================
SUPABASE
========================= */

const SUPABASE_URL="YOUR_SUPABASE_URL";

const SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY";

const supabaseClient =
window.supabase.createClient(
SUPABASE_URL,
SUPABASE_ANON_KEY
);


/* =========================
DATE
========================= */

function today(){

return new Date()
.toISOString()
.slice(0,10);

}

document.getElementById("home-date").textContent=
today().replaceAll("-"," / ");

document.getElementById("today-date").textContent=
today();


/* =========================
AUTH
========================= */

async function login(){

const email=
document.getElementById("email").value;

const password=
document.getElementById("password").value;

const {error}=await supabaseClient.auth
.signInWithPassword({
email,
password
});

if(error){

document.getElementById("login-error")
.textContent=error.message;

return;

}

openApp();

}


async function register(){

const email=
document.getElementById("email").value;

const password=
document.getElementById("password").value;

const {error}=await supabaseClient.auth
.signUp({
email,
password
});

if(error){

document.getElementById("login-error")
.textContent=error.message;

}else{

document.getElementById("login-error")
.textContent=
"登録しました。メールを確認してください。";

}

}


async function logout(){

await supabaseClient.auth.signOut();

document.getElementById("app").style.display="none";

document.getElementById("login").style.display="flex";

}


async function checkAuth(){

const {
data:{session}
}=await supabaseClient.auth.getSession();

if(session){

openApp();

}

}


function openApp(){

document.getElementById("login")
.style.display="none";

document.getElementById("app")
.style.display="block";

loadAll();

}


/* =========================
NAVIGATION
========================= */

function showPage(page,button){

document
.querySelectorAll(".page")
.forEach(x=>x.classList.add("hidden"));

document
.getElementById(page)
.classList.remove("hidden");

document
.querySelectorAll(".nav-button")
.forEach(x=>x.classList.remove("active"));

button.classList.add("active");

loadAll();

}


/* =========================
USER
========================= */

async function user(){

const {
data:{user}
}=await supabaseClient.auth.getUser();

return user;

}


/* =========================
SCHEDULE
========================= */

async function addSchedule(){

const u=await user();

if(!u)return;

await supabaseClient
.from("schedules")
.insert({

user_id:u.id,

title:
document.getElementById("schedule-title").value,

date:
document.getElementById("schedule-date").value,

all_day:
document.getElementById("schedule-all-day").checked,

start_time:
document.getElementById("schedule-start").value||null,

end_time:
document.getElementById("schedule-end").value||null,

description:
document.getElementById("schedule-description").value

});

document.getElementById("schedule-title").value="";
document.getElementById("schedule-description").value="";

loadSchedules();

}


async function loadSchedules(){

const {data}=await supabaseClient
.from("schedules")
.select("*")
.order("date")
.order("start_time");

const box=
document.getElementById("schedule-list");

box.innerHTML="";

(data||[]).forEach(item=>{

const div=document.createElement("div");

div.className="row";

div.innerHTML=`

<div>

<b>${escapeHTML(item.title)}</b>

<div class="small">

${item.date}
/
${item.all_day?"ALL DAY":item.start_time?.slice(0,5)||""}

</div>

${item.description?
`<div class="small">${escapeHTML(item.description)}</div>`
:""}

</div>

<button class="delete"
onclick="deleteSchedule('${item.id}')">
DELETE
</button>

`;

box.appendChild(div);

});

}


async function deleteSchedule(id){

await supabaseClient
.from("schedules")
.delete()
.eq("id",id);

loadSchedules();

}


/* =========================
TODO
========================= */

async function addTodo(){

const u=await user();

if(!u)return;

await supabaseClient
.from("todos")
.insert({

user_id:u.id,

title:
document.getElementById("todo-title").value,

date:
document.getElementById("todo-date").value,

all_day:
document.getElementById("todo-all-day").checked,

time:
document.getElementById("todo-time").value||null,

description:
document.getElementById("todo-description").value

});

document.getElementById("todo-title").value="";

loadTodos();

}


async function loadTodos(){

const {data}=await supabaseClient
.from("todos")
.select("*")
.order("completed")
.order("date");

const box=
document.getElementById("todo-list");

box.innerHTML="";

(data||[]).forEach(item=>{

const div=document.createElement("div");

div.className="row";

div.innerHTML=`

<div>

<button class="check"
onclick="toggleTodo('${item.id}',${item.completed})">

${item.completed?"✓":"□"}

</button>

<b style="${item.completed?
'text-decoration:line-through;opacity:.4':''}">
${escapeHTML(item.title)}
</b>

<div class="small">

${item.date}
/
${item.all_day?
"ALL DAY":
item.time?.slice(0,5)||""}

</div>

</div>

<button class="delete"
onclick="deleteTodo('${item.id}')">
DELETE
</button>

`;

box.appendChild(div);

});

}


async function toggleTodo(id,status){

await supabaseClient
.from("todos")
.update({
completed:!status
})
.eq("id",id);

loadTodos();

}


async function deleteTodo(id){

await supabaseClient
.from("todos")
.delete()
.eq("id",id);

loadTodos();

}


/* =========================
MONEY
========================= */

async function addMoney(){

const u=await user();

if(!u)return;

await supabaseClient
.from("expenses")
.insert({

user_id:u.id,

title:
document.getElementById("money-title").value,

amount:
Number(document.getElementById("money-amount").value),

category:
document.getElementById("money-category").value,

date:
document.getElementById("money-date").value,

memo:
document.getElementById("money-memo").value

});

loadMoney();

}


async function loadMoney(){

const {data}=await supabaseClient
.from("expenses")
.select("*")
.order("date",{ascending:false});

const box=
document.getElementById("money-list");

box.innerHTML="";

let total=0;

(data||[]).forEach(item=>{

total+=item.amount;

const div=document.createElement("div");

div.className="row";

div.innerHTML=`

<div>

<b>${escapeHTML(item.title)}</b>

<div class="small">

${item.date}
/
${escapeHTML(item.category)}

</div>

</div>

<div>

¥${Number(item.amount).toLocaleString()}

<button
class="delete"
onclick="deleteMoney('${item.id}')">
×
</button>

</div>

`;

box.appendChild(div);

});

document.getElementById("money-total")
.textContent="¥"+total.toLocaleString();

}


async function deleteMoney(id){

await supabaseClient
.from("expenses")
.delete()
.eq("id",id);

loadMoney();

}


/* =========================
WISH
========================= */

async function addWish(){

const u=await user();

if(!u)return;

await supabaseClient
.from("wishes")
.insert({

user_id:u.id,

title:
document.getElementById("wish-title").value,

price:
Number(document.getElementById("wish-price").value)||null,

url:
document.getElementById("wish-url").value,

memo:
document.getElementById("wish-memo").value,

category_id:null

});

loadWish();

}


async function loadWish(){

const {data}=await supabaseClient
.from("wishes")
.select("*")
.order("created_at",{ascending:false});

const box=
document.getElementById("wish-list");

box.innerHTML="";

(data||[]).forEach(item=>{

const div=document.createElement("div");

div.className="row";

div.innerHTML=`

<div>

<button class="check"
onclick="toggleWish('${item.id}',${item.purchased})">

${item.purchased?"✓":"♡"}

</button>

<b>${escapeHTML(item.title)}</b>

<div class="small">

${item.category_id?"CATEGORY":""}

</div>

${item.memo?
`<div class="small">${escapeHTML(item.memo)}</div>`
:""}

</div>

<div>

${item.price?
"¥"+Number(item.price).toLocaleString():
""}

</div>

`;

box.appendChild(div);

});

}


async function toggleWish(id,status){

await supabaseClient
.from("wishes")
.update({
purchased:!status
})
.eq("id",id);

loadWish();

}


/* =========================
MEMO
========================= */

async function addMemo(){

const u=await user();

if(!u)return;

await supabaseClient
.from("memos")
.insert({

user_id:u.id,

title:
document.getElementById("memo-title").value,

content:
document.getElementById("memo-content").value

});

document.getElementById("memo-title").value="";
document.getElementById("memo-content").value="";

loadMemos();

}


async function loadMemos(){

const {data}=await supabaseClient
.from("memos")
.select("*")
.order("created_at",{ascending:false});

const box=
document.getElementById("memo-list");

box.innerHTML="";

(data||[]).forEach(item=>{

const div=document.createElement("div");

div.className="card";

div.style.marginBottom="20px";

div.innerHTML=`

<b>${escapeHTML(item.title)}</b>

<div class="small">

${new Date(item.created_at)
.toLocaleDateString("ja-JP")}

</div>

<div style="white-space:pre-wrap;margin-top:15px">

${escapeHTML(item.content||"")}

</div>

`;

box.appendChild(div);

});

}


/* =========================
STUDY
========================= */

async function addStudy(){

const u=await user();

if(!u)return;

const subject=
document.getElementById("study-subject").value;

const date=
document.getElementById("study-date").value;

const minutes=
Number(document.getElementById("study-minutes").value);

const memo=
document.getElementById("study-memo").value;

const {data,error}=await supabaseClient
.from("study_logs")
.insert({

user_id:u.id,
subject,
date,
minutes,
memo

})
.select()
.single();

if(error){

console.error(error);

return;

}

const file=
document.getElementById("study-photo").files[0];

if(file){

const path=
`${u.id}/${Date.now()}-${file.name}`;

const {error:uploadError}=
await supabaseClient
.storage
.from("study-photos")
.upload(path,file);

if(!uploadError){

await supabaseClient
.from("study_photos")
.insert({

user_id:u.id,

study_log_id:data.id,

storage_path:path

});

}

}

loadStudy();

}


async function loadStudy(){

const {data}=await supabaseClient
.from("study_logs")
.select("*")
.order("date",{ascending:false});

const box=
document.getElementById("study-list");

box.innerHTML="";

let total=0;

(data||[]).forEach(item=>{

total+=Number(item.minutes||0);

const div=document.createElement("div");

div.className="row";

div.innerHTML=`

<div>

<b>${escapeHTML(item.subject)}</b>

<div class="small">

${item.date}

</div>

${item.memo?
`<div class="small">${escapeHTML(item.memo)}</div>`
:""}

</div>

<b>

${formatMinutes(item.minutes)}

</b>

`;

box.appendChild(div);

});

document.getElementById("study-total")
.textContent=formatMinutes(total);

}


function formatMinutes(minutes){

minutes=Number(minutes||0);

const h=Math.floor(minutes/60);

const m=minutes%60;

return String(h).padStart(2,"0")
+":"+String(m).padStart(2,"0");

}


/* =========================
HOME
========================= */

async function loadHome(){

const d=today();

const {data:schedules}=await supabaseClient
.from("schedules")
.select("*")
.gte("date",d)
.order("date")
.order("start_time")
.limit(5);

const scheduleBox=
document.getElementById("home-schedules");

scheduleBox.innerHTML="";

(schedules||[]).forEach(item=>{

scheduleBox.innerHTML+=`

<div class="row">

<span>${item.date}</span>

<span>

${item.all_day?
"ALL DAY":
item.start_time?.slice(0,5)||""}

</span>

<b>${escapeHTML(item.title)}</b>

</div>

`;

});


const {data:todos}=await supabaseClient
.from("todos")
.select("*")
.eq("date",d)
.order("completed");

const todoBox=
document.getElementById("home-todos");

todoBox.innerHTML="";

(todos||[]).slice(0,5).forEach(item=>{

todoBox.innerHTML+=`

<div class="row">

<span>

${item.completed?"✓":"□"}

</span>

<span>

${escapeHTML(item.title)}

</span>

</div>

`;

});


const {data:money}=await supabaseClient
.from("expenses")
.select("amount")
.eq("date",d);

const totalMoney=
(money||[]).reduce(
(a,b)=>a+Number(b.amount),0
);

document.getElementById("home-money")
.textContent=
"¥"+totalMoney.toLocaleString();


const {data:study}=await supabaseClient
.from("study_logs")
.select("minutes")
.eq("date",d);

const totalStudy=
(study||[]).reduce(
(a,b)=>a+Number(b.minutes),0
);

document.getElementById("home-study")
.textContent=
formatMinutes(totalStudy);

}


/* =========================
TODAY
========================= */

async function loadToday(){

const d=today();

const {data:s}=await supabaseClient
.from("schedules")
.select("*")
.eq("date",d)
.order("start_time");

const sb=
document.getElementById("today-schedule");

sb.innerHTML="";

(s||[]).forEach(x=>{

sb.innerHTML+=`

<div class="row">

<span>
${x.all_day?
"ALL DAY":
x.start_time?.slice(0,5)||""}
</span>

<span>${escapeHTML(x.title)}</span>

</div>

`;

});


const {data:t}=await supabaseClient
.from("todos")
.select("*")
.eq("date",d);

const tb=
document.getElementById("today-todo");

tb.innerHTML="";

(t||[]).forEach(x=>{

tb.innerHTML+=`

<div class="row">

<span>${x.completed?"✓":"□"}</span>

<span>${escapeHTML(x.title)}</span>

</div>

`;

});


const {data:m}=await supabaseClient
.from("expenses")
.select("*")
.eq("date",d);

const mb=
document.getElementById("today-money");

mb.innerHTML="";

let total=0;

(m||[]).forEach(x=>{

total+=Number(x.amount);

mb.innerHTML+=`

<div class="row">

<span>${escapeHTML(x.title)}</span>

<span>
¥${Number(x.amount).toLocaleString()}
</span>

</div>

`;

});

document.getElementById("today-total")
.textContent=
"¥"+total.toLocaleString();


const {data:st}=await supabaseClient
.from("study_logs")
.select("*")
.eq("date",d);

const stb=
document.getElementById("today-study");

stb.innerHTML="";

let studyTotal=0;

(st||[]).forEach(x=>{

studyTotal+=Number(x.minutes);

stb.innerHTML+=`

<div class="row">

<span>${escapeHTML(x.subject)}</span>

<span>${x.minutes} min</span>

</div>

`;

});

stb.innerHTML+=`

<div class="total">

<span>TOTAL</span>

<span>${formatMinutes(studyTotal)}</span>

</div>

`;

}


/* =========================
ALL
========================= */

async function loadAll(){

await loadHome();

await loadSchedules();

await loadTodos();

await loadMoney();

await loadWish();

await loadMemos();

await loadStudy();

await loadToday();

}


/* =========================
SECURITY
========================= */

function escapeHTML(str){

return String(str||"")
.replace(/&/g,"&amp;")
.replace(/</g,"&lt;")
.replace(/>/g,"&gt;")
.replace(/"/g,"&quot;")
.replace(/'/g,"&#039;");

}


/* =========================
START
========================= */

checkAuth();

</script>

</body>
</html>

