# Report
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Daily Report Dashboard</title>
<style>
*{box-sizing:border-box}
body{margin:0;padding:25px;background:#0b1020;color:#eef2ff;font-family:Arial}
main{max-width:850px;margin:auto}
.panel{background:#151d32;border:1px solid #293754;padding:20px;border-radius:15px;margin:16px 0}
.stats{display:flex;gap:12px;flex-wrap:wrap}
.stat{flex:1;min-width:120px;background:#202a44;padding:16px;border-radius:12px}
input,textarea,select{width:100%;padding:12px;margin:8px 0 14px;background:#0c1427;color:white;border:1px solid #344563;border-radius:8px}
button{padding:11px 16px;background:#8b7cff;border:0;border-radius:8px;color:white;cursor:pointer}
.item{padding:12px;border-bottom:1px solid #34405b;overflow-wrap:anywhere}
</style>
</head>
<body>
<main>
<h1>◉ Daily Report</h1>
<p>Track your daily work and productivity.</p>
<div class="stats">
<div class="stat">Total reports<h2 id="count">0</h2></div>
<div class="stat">Completed<h2 id="completed">0</h2></div>
</div>
<form class="panel" id="form">
<label>Date</label><input type="date" id="date" required>
<label>Activity</label><input id="task" placeholder="What did you do?" required>
<label>Notes</label><textarea id="notes" placeholder="Add details"></textarea>
<label>Status</label>
<select id="status"><option>Completed</option><option>In progress</option><option>Planned</option></select>
<button>Add Report</button>
</form>
<section class="panel">
<h2>Recent Reports</h2><div id="list"></div>
</section>
</main>
<script>
const $=id=>document.getElementById(id);
let rows=JSON.parse(localStorage.reports||"[]");
$("date").value=new Date().toLocaleDateString("en-CA");
function draw(){
$("count").textContent=rows.length;
$("completed").textContent=rows.filter(x=>x.status==="Completed").length;
$("list").innerHTML=rows.map((x,i)=>`
<div class="item"><b>${safe(x.task)}</b> — ${x.status}
<p>${x.date} · ${safe(x.notes||"No notes")}</p>
<button onclick="removeRow(${i})">Delete</button></div>`).join("")||"No reports yet.";
localStorage.reports=JSON.stringify(rows);
}
function safe(s){return String(s).replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]))}
function removeRow(i){rows.splice(i,1);draw()}
$("form").onsubmit=e=>{
e.preventDefault();
rows.unshift({date:$("date").value,task:$("task").value,notes:$("notes").value,status:$("status").value});
$("task").value="";$("notes").value="";draw();
};
draw();
</script>
</body>
</html>
