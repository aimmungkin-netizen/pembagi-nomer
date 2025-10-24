<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Penyortir Angka Fleksibel (2–5 Bagian)</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; background: #f9f9f9; }
    textarea { width: 100%; height: 120px; margin-bottom: 10px; }
    button, select { margin: 4px; padding: 8px 12px; cursor: pointer; }
    .result-box { width: 100%; height: 100px; background: #fff; border: 1px solid #ccc; padding: 8px; overflow-y: auto; white-space: pre-wrap; word-wrap: break-word; margin-bottom: 4px; }
    .count-box { margin-top: 8px; font-weight: bold; }
    .section { margin-bottom: 15px; }
  </style>
</head>
<body>
  <h2>Penyortir Angka (Fleksibel 2–5 Bagian)</h2>

  <textarea id="inputBox" placeholder="Masukkan angka 2–4 digit, pisahkan dengan * tanpa spasi..."></textarea><br>
  <label>Pilih jumlah bagian:</label>
  <select id="jumlahBagian">
    <option value="2">2 Bagian</option>
    <option value="3" selected>3 Bagian</option>
    <option value="4">4 Bagian</option>
    <option value="5">5 Bagian</option>
  </select><br>

  <button onclick="prosesSortir()">Proses</button>
  <button onclick="resetAll()">Reset</button>

  <div id="hasilContainer"></div>
  <div id="countBox" class="count-box"></div>

  <script>
    function prosesSortir() {
      const raw = document.getElementById("inputBox").value.trim();
      const bagian = parseInt(document.getElementById("jumlahBagian").value);
      const container = document.getElementById("hasilContainer");
      container.innerHTML = ""; // hapus hasil lama

      if (!raw) return alert("Masukkan dulu data angkanya.");

      // Pisahkan berdasarkan tanda *
      const data = raw.split("*").map(x => x.trim()).filter(x => /^\d{2,4}$/.test(x));
      if (data.length === 0) return alert("Tidak ada angka valid (2–4 digit).");

      // Hapus duplikat, urutan tetap acak
      const unik = [];
      data.forEach(num => {
        if (!unik.includes(num)) unik.push(num);
      });

      // Bagi data menjadi N bagian
      const total = unik.length;
      const perBagian = Math.ceil(total / bagian);
      const hasilBagian = [];

      for (let i = 0; i < bagian; i++) {
        hasilBagian.push(unik.slice(i * perBagian, (i + 1) * perBagian));
      }

      // Tampilkan hasil per bagian
      hasilBagian.forEach((bagianData, i) => {
        const div = document.createElement("div");
        div.className = "section";
        div.innerHTML = `
          <h3>Hasil ${i + 1}</h3>
          <div id="result${i + 1}" class="result-box">${bagianData.join("*")}</div>
          <button onclick="salinBagian(${i + 1})">Salin Hasil ${i + 1}</button>
        `;
        container.appendChild(div);
      });

      // Tampilkan jumlah total
      document.getElementById("countBox").textContent = "Jumlah unik total: " + unik.length;
    }

    function salinBagian(n) {
      const hasil = document.getElementById("result" + n).textContent;
      if (!hasil) return alert("Belum ada hasil di Bagian " + n);
      navigator.clipboard.writeText(hasil).then(() => {
        alert("✅ Hasil Bagian " + n + " telah disalin!");
      });
    }

    function resetAll() {
      document.getElementById("inputBox").value = '';
      document.getElementById("hasilContainer").innerHTML = '';
      document.getElementById("countBox").textContent = '';
    }
  </script>
</body>
</html>
