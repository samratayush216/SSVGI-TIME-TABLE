<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SSVGI CSE Timetable</title>

<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#eef4ff;color:#172554}
header{background:#073b82;color:white;text-align:center;padding:22px}
header h1{font-size:38px;margin:0}
header p{margin:8px}
#intro{position:fixed;inset:0;background:linear-gradient(#dceeff,#fff);z-index:10;display:flex;flex-direction:column;align-items:center;justify-content:center;transition:opacity 1s,visibility 1s}
#intro h1{font-size:65px;color:#0751ad;margin:10px}
#intro p{font-size:18px}
.doors{display:flex;gap:8px;perspective:800px}
.door{width:100px;height:150px;background:#0751ad;border:6px solid #f5bd42;border-radius:8px;animation:openDoor 2s ease-in-out forwards}
.door:first-child{transform-origin:left;animation-name:leftDoor}
.door:last-child{transform-origin:right;animation-name:rightDoor}
.robot{font-size:75px;position:absolute;top:48%;animation:welcome 2s ease-in-out}
@keyframes leftDoor{to{transform:rotateY(-110deg)}}
@keyframes rightDoor{to{transform:rotateY(110deg)}}
@keyframes welcome{0%{transform:scale(.4)}100%{transform:scale(1)}}
.hidden{opacity:0;visibility:hidden}
main{max-width:1200px;margin:20px auto;padding:12px}
.card{background:white;border-radius:12px;padding:18px;margin-bottom:20px;box-shadow:0 3px 12px #0001}
h2{color:#0751ad}
.teachers{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:10px}
.teacher{background:#e5efff;padding:13px;text-align:center;border-radius:8px;font-weight:bold}
.tablewrap{overflow-x:auto}
table{width:100%;min-width:1000px;border-collapse:collapse;background:white}
th,td{border:1px solid #b9c9df;text-align:center;padding:10px 7px;font-size:13px}
th{background:#0751ad;color:white}
tr:nth-child(even){background:#f0f6ff}
.lunch{background:#ffedb8;font-weight:bold}
footer{text-align:center;padding:20px;color:#64748b}
</style>
</head>

<body>

<div id="intro">
    <h1>SSVGI</h1>
    <p>Welcome to B.Tech CSE First Year</p>
    <div class="doors">
        <div class="door"></div>
        <div class="door"></div>
    </div>
    <div class="robot">🤖</div>
    <p>Opening Your Class Timetable...</p>
</div>

<header>
    <h1>SSVGI</h1>
    <h2 style="color:white">B.Tech CSE First Year</h2>
    <p>Weekly Class Timetable</p>
</header>

<main>
<section class="card">
<h2>Our Faculty</h2>
<div class="teachers">
<div class="teacher">A. B. Sir</div>
<div class="teacher">K. P. Sir</div>
<div class="teacher">V. K. Mam</div>
<div class="teacher">A. G. Sir</div>
<div class="teacher">A. P. Sir</div>
<div class="teacher">S. B. Sir</div>
</div>
</section>

<section class="card">
<h2>Class Timetable</h2>
<div class="tablewrap">
<table>
<thead>
<tr>
<th>Day / Section</th>
<th>9:15–10:15</th>
<th>10:15–11:15</th>
<th>11:15–12:15</th>
<th>12:15–1:15</th>
<th>Lunch<br>1:15–2:00</th>
<th>2:00–2:50</th>
<th>2:50–3:40</th>
<th>3:40–4:30</th>
</tr>
</thead>
<tbody id="rows"></tbody>
</table>
</div>
<p style="font-size:12px;color:#64748b">Sections A, B and C are shown separately. Confirm entries against the official timetable.</p>
</section>
</main>

<footer>SSVGI | B.Tech CSE First Year</footer>

<script>
const data = {
MON: {
A:["Maths (K.P)","Physics (A.B)","P.C. & T.W (V.K)","E.E (A.K)","E.D.L (Ashu S.)","E.D LAB (Ashu S.)","—","—"],
B:["P.D (A.P)","E.E (A.K)","Maths (K.P)","Physics (A.B)","Prog. Lang. (S.B)","Prog. Lang. Lab (S.B)","—","—"],
C:["Prog. Lang. (S.B)","AI & Prompt (A.G)","P.D (A.P)","Physics (A.B)","Prog. Lang. (S.B)","Physics","—","—"]
},
TUE: {
A:["Maths (K.P)","Physics (A.B)","P.D (A.P)","P.C. & T.W (V.K)","Prog. Lang. (S.B)","EE LAB (A.K)","PHY LAB (A.B)","—"],
B:["Prog. Lang. (S.B)","AI & Prompt (A.G)","E.E (A.K)","Maths (K.P)","Physics (A.B)","ED LAB (Ashu S.)","—","—"],
C:["E.E (A.K)","P.C. & T.W (V.K)","Prog. Lang. (S.B)","Physics (A.B)","Maths (K.P)","ED LAB (Ashu S.)","—","—"]
},
WED: {
A:["Prog. Lang. (S.B)","AI & Prompt (A.G)","P.C. & T.W (V.K)","Maths (K.P)","AI & Prompt (A.G)","Prog. Lang. Lab (S.B)","—","—"],
B:["E.E (A.K)","Physics (A.B)","P.C. & T.W (V.K)","Maths (K.P)","Physics (A.B)","EE LAB (A.K)","—","—"],
C:["Maths (K.P)","P.D (A.P)","Prog. Lang. (S.B)","E.E (A.K)","P.C. & T.W (V.K)","PHY LAB (A.B)","—","—"]
},
THU: {
A:["Maths (K.P)","P.C. & T.W (V.K)","AI & Prompt (A.G)","E.E (A.K)","Physics (A.B)","ED LAB (Ashu S.)","—","—"],
B:["P.C. & T.W (V.K)","Maths (K.P)","Physics (A.B)","Prog. Lang. (S.B)","Prog. Lang. Lab (S.B)","PHY LAB (A.B)","—","—"],
C:["Prog. Lang. (S.B)","Physics (A.B)","E.E (A.K)","Maths (K.P)","P.C. & T.W (V.K)","ED LAB (Ashu S.)","—","—"]
},
FRI: {
A:["P.D (A.P)","Maths (K.P)","Prog. Lang. (S.B)","Physics (A.B)","Prog. Lang. Lab (S.B)","EE LAB (A.K)","—","—"],
B:["P.C. & T.W (V.K)","Maths (K.P)","P.D (A.P)","Prog. Lang. (S.B)","Physics (A.B)","PHY LAB (A.B)","—","—"],
C:["E.E (A.K)","LIB","P.C. & T.W (V.K)","AI & Prompt (A.G)","Maths (K.P)","Prog. Lang. Lab (S.B)","—","—"]
},
SAT: {
A:["P.D (A.P)","A.I. & Prompt (A.G)","LIB","Prog. Lang. (S.B)","Maths (K.P)","P.C. & T.W (V.K)","ENG. LAP (V.K)","—"],
B:["P.C. & T.W (V.K)","P.D (A.P)","Maths (K.P)","AI & Prompt (A.G)","P.C. & T.W (V.K)","Physics (A.B)","E.E (A.K)","—"],
C:["Physics (A.B)","Prog. Lang. (S.B)","P.C. & T.W (V.K)","Physics (A.B)","E.E (A.K)","Maths (K.P)","LIB","—"]
}
};

const tbody = document.getElementById("rows");
for (const [day, sections] of Object.entries(data)) {
    for (const [section, subjects] of Object.entries(sections)) {
        const tr = document.createElement("tr");
        let html = `<th>${day} - ${section}</th>`;
        for (let i = 0; i < 4; i++) html += `<td>${subjects[i] || "—"}</td>`;
        html += `<td class="lunch">LUNCH</td>`;
        for (let i = 4; i < 7; i++) html += `<td>${subjects[i] || "—"}</td>`;
        tr.innerHTML = html;
        tbody.appendChild(tr);
    }
}

window.addEventListener("load", () => {
    setTimeout(() => {
        document.getElementById("intro").classList.add("hidden");
    }, 2500);
});
</script>

</body>
</html>
