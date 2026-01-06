# kasir03<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kasir RM JOYO - Rekap Per Nota + Grand Total</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:#f2f2f2;color:#000;transition:0.3s;}
body.dark{background:#121212;color:#eee;}
.page{display:none;padding:15px;}
.page.active{display:block;}
.kasir,.riwayat-container,.rekap-container,.login-container{width:90%;max-width:700px;margin:15px auto;background:#fff;padding:15px;border-radius:14px;box-shadow:0 6px 20px rgba(0,0,0,.15);}
body.dark .kasir,body.dark .riwayat-container,body.dark .rekap-container,body.dark .login-container{background:#1e1e1e;color:#eee;}
.header{text-align:center;margin-bottom:10px;}
button{width:100%;padding:10px;margin:5px 0;border:none;border-radius:10px;background:#c62828;color:white;font-size:16px;cursor:pointer;}
body.dark button{background:#b71c1c;}
input{width:100%;padding:8px;margin:5px 0;border:1px solid #ccc;border-radius:6px;font-size:14px;}
body.dark input{background:#333;color:#eee;border-color:#888;}
.kategori-title{margin:8px 0;font-weight:bold;font-size:18px;color:#333;}
body.dark .kategori-title{color:#eee;}
.menu-item{padding:6px 12px;margin-left:20px;border-bottom:1px dashed #ccc;cursor:pointer;display:flex;justify-content:space-between;align-items:center;}
.menu-item:hover{background:#fafafa;}
body.dark .menu-item:hover{background:#333;}
.menu-item span{color:#c62828;font-weight:bold;}
.order{display:flex;justify-content:space-between;font-size:14px;margin:3px 0;align-items:center;}
.order span.rp{text-align:right;display:inline-block;min-width:80px;}
.hapusOrder{margin-left:8px;color:red;cursor:pointer;font-weight:bold;}
.total{text-align:center;font-weight:bold;font-size:18px;margin-top:10px;}
#riwayat,#rekapDiv{max-height:400px;overflow:auto;border:1px dashed #ccc;padding:5px;}
body.dark #riwayat,body.dark #rekapDiv{border-color:#555;}
#inputMenu{display:flex;gap:6px;margin-bottom:10px;}
#inputMenu input,#inputMenu select{flex:2 1 120px;padding:6px;border:1px solid #ccc;border-radius:6px;font-size:14px;}
#inputMenu button{flex:1 1 60px;border:none;background:#2e7d32;color:white;cursor:pointer;border-radius:6px;font-size:14px;}
body.dark #inputMenu input, body.dark #inputMenu select, body.dark #inputMenu button{border-color:#888;background:#333;color:#eee;}
body.dark #darkModeBtn{background:#fbc02d;color:#000;}
hr{border:none;border-top:1px dashed #ccc;}
</style>
</head>
<body>

<!-- Audio -->
<audio id="suaraKlik" src="https://www.soundjay.com/buttons/sounds/button-16.mp3" preload="auto"></audio>
<audio id="suaraBayar" src="https://www.soundjay.com/buttons/sounds/button-3.mp3" preload="auto"></audio>
<audio id="suaraTambah" src="https://www.soundjay.com/buttons/sounds/button-10.mp3" preload="auto"></audio>
<audio id="suaraCetak" src="https://www.soundjay.com/buttons/sounds/button-09.mp3" preload="auto"></audio>

<!-- Halaman Login -->
<div id="pageLogin" class="page active">
  <div class="login-container">
    <div class="header"><h2>Login RM JOYO</h2></div>
    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">
    <button onclick="login()">Login</button>
    <div id="loginMsg" style="color:red;margin-top:5px;"></div>
  </div>
</div>

<!-- Halaman Kasir -->
<div id="pageKasir" class="page">
  <div class="kasir">
    <div class="header"><h2>RM JOYO - Kasir</h2><div id="waktu"></div></div>
    <div id="menuContainer"></div>
    <hr>
    <div id="list"></div>
    <button onclick="bayar()">Bayar</button>
    <div class="total">Total: <span id="total">0</span></div>
    <button onclick="showPage('pageRiwayat')">Lihat Riwayat / BOS</button>
    <button onclick="showPage('pageRekap')">Lihat Rekap Per Nota</button>
  </div>
</div>

<!-- Halaman Riwayat / BOS -->
<div id="pageRiwayat" class="page">
  <div class="riwayat-container">
    <div class="header"><h2>RM JOYO - Riwayat & BOS (Per Tanggal)</h2></div>
    <div id="inputMenu">
      <input type="text" id="namaBaru" placeholder="Nama Menu">
      <input type="number" id="hargaBaru" placeholder="Harga">
      <select id="kategoriBaru">
        <option value="Makanan">Makanan</option>
        <option value="Minuman">Minuman</option>
        <option value="Jajanan">Jajanan</option>
      </select>
      <button onclick="tambahMenuBaru()">Tambah</button>
    </div>
    <button id="darkModeBtn" onclick="toggleDark()">Dark Mode: OFF</button>
    <button id="autoPrintBtn" onclick="toggleAutoPrint()">Cetak Otomatis: OFF</button>
    <button onclick="cetakManual()">Cetak Manual</button>
    <button onclick="showPage('pageKasir')">Kembali ke Kasir</button>
    <div id="riwayat"></div>
  </div>
</div><!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Running Text Kecil di Atas</title>
<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    transition: background 0.5s;
  }

  .marquee-container {
    width: 100%;
    overflow: hidden;
    white-space: nowrap;
    padding: 0;       /* merapat ke atas */
    margin: 0;        /* merapat ke atas */
    font-size: 80%;   /* lebih kecil 80% */
    font-weight: bold;
    position: fixed;  /* tetap di atas layar */
    top: 0;           /* posisi paling atas */
    left: 0;
    z-index: 1000;    /* selalu di atas elemen lain */
  }

  .marquee-text {
    display: inline-block;
    padding-left: 100%;
    animation: scroll 15s linear infinite;
  }

  @keyframes scroll {
    0% { transform: translateX(0%); }
    100% { transform: translateX(-100%); }
  }
</style>
</head>
<body>

<div class="marquee-container" id="marqueeContainer">
  <div class="marquee-text" id="runningText">Loading...</div>
</div>

<script>
  // Nama user otomatis
  let userName = "KOES";

  function getWaktu() {
    const hour = new Date().getHours();
    if(hour >= 4 && hour < 10) return "SELAMAT PAGI";
    if(hour >= 10 && hour < 15) return "SELAMAT SIANG";
    if(hour >= 15 && hour < 18) return "SELAMAT SORE";
    return "SELAMAT MALAM";
  }

  function getColorsByHour() {
    const hour = new Date().getHours();
    if(hour >= 4 && hour < 10) return { background: "#FFFAAA", text: "#333" }; // pagi
    if(hour >= 10 && hour < 15) return { background: "#87CEEB", text: "#fff" }; // siang
    if(hour >= 15 && hour < 18) return { background: "#FFA500", text: "#000" }; // sore
    return { background: "#2C3E50", text: "#fff" }; // malam
  }

  const runningTextEl = document.getElementById("runningText");

  function updateRunningText() {
    runningTextEl.textContent = `${getWaktu()}, ${userName}... SEMANGAT SEMOGA SUKSES`;

    const colors = getColorsByHour();
    document.body.style.background = colors.background;
    runningTextEl.style.color = colors.text;
  }

  setInterval(updateRunningText, 1000);
  updateRunningText();
</script>

</body>
</html>

<!-- Halaman Rekap Per Nota + Grand Total -->
<div id="pageRekap" class="page">
  <div class="rekap-container">
    <div class="header"><h2>RM JOYO - Rekap Per Nota + Grand Total</h2></div>
    <button onclick="showPage('pageKasir')">Kembali ke Kasir</button>
    <button onclick="showPage('pageRiwayat')">Kembali ke Riwayat</button>
    <div id="rekapDiv"></div>
  </div>
</div>

<script>
// --- Data & Elemen ---
let menuData = JSON.parse(localStorage.getItem("menuRM")) || [
  {nama:"Nasi Goreng",harga:15000,kategori:"Makanan"},
  {nama:"Mie Goreng",harga:14000,kategori:"Makanan"},
  {nama:"Ayam Goreng",harga:18000,kategori:"Makanan"},
  {nama:"Teh",harga:5000,kategori:"Minuman"},
  {nama:"Kopi",harga:7000,kategori:"Minuman"},
  {nama:"Es Jeruk",harga:8000,kategori:"Minuman"},
  {nama:"Gorengan",harga:10000,kategori:"Jajanan"}
];
let pesananData=[], darkMode=JSON.parse(localStorage.getItem("darkMode"))||false, autoPrint=false;
const menuContainer=document.getElementById("menuContainer"), list=document.getElementById("list"), totalEl=document.getElementById("total"), waktuEl=document.getElementById("waktu"), riwayatDiv=document.getElementById("riwayat"), rekapDiv=document.getElementById("rekapDiv"), darkBtn=document.getElementById("darkModeBtn"), autoBtn=document.getElementById("autoPrintBtn");
const suaraKlik=document.getElementById("suaraKlik"), suaraBayar=document.getElementById("suaraBayar"), suaraTambah=document.getElementById("suaraTambah"), suaraCetak=document.getElementById("suaraCetak");

// --- Audio ---
function playAudio(audio){ audio.currentTime=0; audio.play(); }

// --- Login ---
function login(){
  const user=document.getElementById("username").value.trim();
  const pass=document.getElementById("password").value.trim();
  const loginMsg=document.getElementById("loginMsg"); loginMsg.innerText="";
  const kasirPass=["3/3/2026 000","6/6/2026 0000","9/9/2026 00000"];
  if((user==="kasir" && kasirPass.includes(pass)) || (user==="777") || (user==="bos" && pass==="2026-2027 M4t4h4r1")){
    showPage("pageKasir"); playAudio(suaraKlik);
  } else { loginMsg.innerText="Username atau Password salah!"; }
}

// --- Halaman ---
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  if(id==='pageRiwayat') loadRiwayat();
  if(id==='pageRekap') loadRekap();
}

// --- Dark Mode ---
if(darkMode){document.body.classList.add("dark"); darkBtn.innerText="Dark Mode: ON";}
function toggleDark(){darkMode=!darkMode; localStorage.setItem("darkMode",darkMode); document.body.classList.toggle("dark"); darkBtn.innerText=`Dark Mode: ${darkMode?'ON':'OFF'}`; playAudio(suaraKlik);}
function toggleAutoPrint(){autoPrint=!autoPrint; autoBtn.innerText=`Cetak Otomatis: ${autoPrint?'ON':'OFF'}`; playAudio(suaraKlik);}

// --- Waktu ---
function updateWaktu(){const d=new Date(); waktuEl.innerText = `${d.toLocaleDateString('id-ID',{weekday:'long'})}, ${d.getDate().toString().padStart(2,'0')} ${d.toLocaleDateString('id-ID',{month:'long'})} ${d.getFullYear()} | ${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`;}
setInterval(updateWaktu,1000); updateWaktu();

// --- Render Menu ---
function renderMenu(){menuContainer.innerHTML=""; ["Makanan","Minuman","Jajanan"].forEach(kat=>{const catMenus = menuData.filter(m=>m.kategori===kat); if(catMenus.length>0){const title=document.createElement("div"); title.className="kategori-title"; title.innerText=kat; menuContainer.appendChild(title); catMenus.forEach(m=>{const div=document.createElement("div"); div.className="menu-item"; div.innerHTML=`<span>${m.nama}</span><span>Rp ${m.harga.toLocaleString('id-ID')}</span>`; div.onclick=()=>{ tambah(m.nama,m.harga); playAudio(suaraKlik); }; menuContainer.appendChild(div); });}});}
renderMenu();

// --- Pesanan ---
function tambah(n,h){let item=pesananData.find(x=>x.nama===n); if(item){item.jumlah++;}else{pesananData.push({nama:n,harga:h,jumlah:1});} renderPesanan();}
function renderPesanan(){list.innerHTML=""; let total=0; pesananData.forEach((p,i)=>{const div=document.createElement("div"); div.className="order"; div.innerHTML=`<span>${p.nama} x${p.jumlah}</span><span class="rp">Rp ${(p.harga*p.jumlah).toLocaleString('id-ID')}</span><span class="hapusOrder">✖</span>`; div.querySelector(".hapusOrder").onclick=()=>{if(confirm(`Hapus pesanan ${p.nama}?`)){pesananData.splice(i,1); renderPesanan(); playAudio(suaraKlik);}}; list.appendChild(div); total+=p.harga*p.jumlah;}); totalEl.innerText=total.toLocaleString('id-ID');}

// --- Bayar ---
function bayar(){
  if(pesananData.length===0){alert("Belum ada pesanan!"); return;}
  playAudio(suaraBayar);
  let total=pesananData.reduce((a,b)=>a+b.harga*b.jumlah,0);
  const riwayat=JSON.parse(localStorage.getItem("riwayatRM"))||[];
  riwayat.push({waktu:new Date().toISOString(), total, pesanan:pesananData});
  localStorage.setItem("riwayatRM",JSON.stringify(riwayat));
  pesananData=[]; renderPesanan();
  showPage('pageRekap'); 
  loadRekap(true); // scroll ke bawah
}

// --- Tambah Menu Baru ---
function tambahMenuBaru(){const nama=document.getElementById("namaBaru").value.trim(); const harga=parseInt(document.getElementById("hargaBaru").value); const kategori=document.getElementById("kategoriBaru").value; if(nama===""||isNaN(harga)||harga<=0){alert("Isi nama dan harga dengan benar!"); return;} menuData.push({nama,harga,kategori}); localStorage.setItem("menuRM",JSON.stringify(menuData)); renderMenu(); document.getElementById("namaBaru").value=""; document.getElementById("hargaBaru").value=""; playAudio(suaraTambah); alert(`Menu "${nama}" berhasil ditambahkan ke kategori ${kategori}!`);}

// --- Riwayat per tanggal ---
function loadRiwayat(){const riwayat = JSON.parse(localStorage.getItem("riwayatRM"))||[]; riwayatDiv.innerHTML=""; if(riwayat.length===0){riwayatDiv.innerHTML="Belum ada transaksi"; return;}
const perTanggal = {}; riwayat.forEach(t=>{const tanggal=new Date(t.waktu).toLocaleDateString('id-ID'); if(!perTanggal[tanggal]) perTanggal[tanggal]=[]; perTanggal[tanggal].push(t);});
for(const [tanggal, transaksiList] of Object.entries(perTanggal)){riwayatDiv.innerHTML+=`<div style="margin-top:10px; font-weight:bold;">Tanggal: ${tanggal}</div>`; let itemTotals={}, totalHari=0; transaksiList.forEach(t=>{t.pesanan.forEach(p=>{if(!itemTotals[p.nama]) itemTotals[p.nama]={jumlah:0,subtotal:0}; itemTotals[p.nama].jumlah+=p.jumlah; itemTotals[p.nama].subtotal+=p.harga*p.jumlah; totalHari+=p.harga*p.jumlah;});}); for(const [nama,data] of Object.entries(itemTotals)){riwayatDiv.innerHTML+=`<div style="margin-left:20px;">${nama}: ${data.jumlah} porsi - Rp ${data.subtotal.toLocaleString('id-ID')}</div>`;} riwayatDiv.innerHTML+=`<b style="margin-left:20px;">Total Hari Ini: Rp ${totalHari.toLocaleString('id-ID')}</b><hr>`;}
riwayatDiv.scrollTop = riwayatDiv.scrollHeight; if(autoPrint){ playAudio(suaraCetak);}
}

// --- Rekap per nota + Grand Total ---
function loadRekap(scrollToBottom=false){
  const riwayat = JSON.parse(localStorage.getItem("riwayatRM"))||[];
  rekapDiv.innerHTML=""; if(riwayat.length===0){rekapDiv.innerHTML="Belum ada transaksi"; return;}
  let grandTotal=0;
  riwayat.forEach((t,i)=>{
    const waktu=new Date(t.waktu).toLocaleString('id-ID');
    rekapDiv.innerHTML+=`<div style="margin-bottom:10px;"><b>Nota #${i+1} - ${waktu}</b><ul>`;
    t.pesanan.forEach(p=>{
      rekapDiv.innerHTML+=`<li>${p.nama} x${p.jumlah} <span style="float:right;">Rp ${(p.harga*p.jumlah).toLocaleString('id-ID')}</span></li>`;
    });
    rekapDiv.innerHTML+=`</ul><b>Total Nota: Rp ${t.total.toLocaleString('id-ID')}</b><hr></div>`;
    grandTotal+=t.total;
  });
  rekapDiv.innerHTML+=`<div style="text-align:center; font-weight:bold; font-size:18px;">Grand Total Semua Nota: Rp ${grandTotal.toLocaleString('id-ID')}</div>`;
  if(scrollToBottom){setTimeout(()=>{rekapDiv.scrollTop = rekapDiv.scrollHeight;}, 100);}
}

// --- Cetak Manual ---
function cetakManual(){playAudio(suaraCetak); loadRiwayat();}
</script>
</body>
l
