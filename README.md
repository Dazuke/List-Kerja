<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Work Tracker</title>
<style>
  body {
  margin: 0;
  font-family: system-ui, sans-serif;
  color: #fff;
  background-color: #0f2027;
  overflow-x: hidden;
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  background:
    linear-gradient(
      rgba(15,32,39,0.75),
      rgba(15,32,39,0.75)
    ),
    url("https://cdn.discordapp.com/attachments/1450360196894425220/1467266781193175121/images_2.jpg?ex=697fc22d&is=697e70ad&hm=5832f81542bac1150278469f5d8e1d6796e2e6294783fd4549f1a84ff0db8bcd&");
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  z-index: -1;
}

  .app {
    max-width: 420px;
    margin: auto;
    padding: 16px;
  }

  h1 {
    text-align: center;
    margin-bottom: 8px;
  }

  .input-box, .list, .summary {
    background: rgba(0,0,0,0.5);
    border-radius: 12px;
    padding: 12px;
    margin-bottom: 12px;
  }

  select, button {
    width: 100%;
    padding: 10px;
    border-radius: 8px;
    border: none;
    margin-top: 8px;
    font-size: 14px;
  }

  button {
    background: #4cafef;
    color: #000;
    font-weight: bold;
  }

  .item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 6px;
    font-size: 13px;
  }

  .item button {
    width: auto;
    padding: 4px 8px;
    background: #ff4d4d;
    color: #fff;
    font-size: 12px;
  }

  .settings {
    text-align: right;
    font-size: 20px;
    cursor: pointer;
  }

  .modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.7);
    display: none;
    align-items: center;
    justify-content: center;
  }

  .modal-box {
    background: #111;
    padding: 16px;
    border-radius: 12px;
    width: 80%;
    max-width: 300px;
    text-align: center;
  }

  .modal-box h3 {
    margin-bottom: 12px;
  }

  .btn-green {
    background: #4caf50;
    color: #000;
    margin-bottom: 8px;
  }

  .btn-red {
    background: #e53935;
    color: #fff;
  }

  footer {
    text-align: center;
    font-size: 11px;
    opacity: 0.7;
    margin-top: 16px;
  }
</style>
</head>
<body>

<div class="app">
  <div class="settings" onclick="openModal()">⚙️</div>
  <h1>Work Tracker</h1>

  <div class="input-box">
    <label>Jenis Hari</label>
    <select id="type">
      <option value="125">Biasa</option>
      <option value="175">Lembur</option>
    </select>
    <button onclick="addDay()">Tambah</button>
  </div>

  <div class="summary">
    <div>Total Hari: <span id="totalDay">0</span></div>
    <div>Total Uang: <span id="totalMoney">0</span></div>
  </div>

  <div class="list" id="list"></div>

  <footer>author: dika</footer>
</div>

<div class="modal" id="modal">
  <div class="modal-box">
    <h3>Yakin anda ingin rekap?</h3>
    <button class="btn-green" onclick="rekap()">yes baby!!!</button>
    <button class="btn-red" onclick="closeModal()">not today bro</button>
  </div>
</div>

<script>
let data = JSON.parse(localStorage.getItem("workData")) || [];

function today() {
  const d = new Date();
  return d.toLocaleDateString("id-ID");
}

function addDay() {
  const value = parseInt(document.getElementById("type").value);
  data.push({ date: today(), value });
  save();
}

function remove(index) {
  data.splice(index, 1);
  save();
}

function save() {
  localStorage.setItem("workData", JSON.stringify(data));
  render();
}

function render() {
  const list = document.getElementById("list");
  list.innerHTML = "";
  let total = 0;

  data.forEach((d, i) => {
    total += d.value;
    list.innerHTML += `
      <div class="item">
        <span>${d.date} - ${d.value}</span>
        <button onclick="remove(${i})">X</button>
      </div>
    `;
  });

  document.getElementById("totalDay").innerText = data.length;
  document.getElementById("totalMoney").innerText = total;
}

function openModal() {
  document.getElementById("modal").style.display = "flex";
}

function closeModal() {
  document.getElementById("modal").style.display = "none";
}

function rekap() {
  let csv = "Tanggal,Jumlah\n";
  data.forEach(d => csv += `${d.date},${d.value}\n`);

  const blob = new Blob([csv], { type: "text/csv" });
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "rekap_kerja.csv";
  a.click();

  closeModal();
}

render();
</script>

</body>
</html>
