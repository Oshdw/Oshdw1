<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }body {
  margin: 0;
  background-color: var(--bg);
  color: var(--text);
  transition: all 0.3s;
}

:root {
  --bg: #f4f6f8;
  --text: #222;
  --card: #fff;
  --primary: #3a86ff;
}

.dark-mode {
  --bg: #121212;
  --text: #fff;
  --card: #1e1e1e;
}

header {
  background: var(--primary);
  color: white;
  padding: 1rem;
  text-align: center;
}

main { padding: 2rem; }

.container {
  max-width: 1000px;
  margin: auto;
}

.hidden { display: none; }

input, select {
  padding: 0.75rem;
  margin-bottom: 1rem;
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button {
  padding: 0.75rem 1.5rem;
  background: var(--primary);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin: 0.25rem;
}

.card {
  background: var(--card);
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.05);
  margin-bottom: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
  background: var(--card);
}

th, td {
  padding: 0.75rem;
  border: 1px solid #ddd;
  text-align: center;
}

th {
  background: #e9f0fb;
}

.toggle-theme {
  position: absolute;
  top: 1rem;
  left: 1rem;
  background: white;
  color: #3a86ff;
  border: 1px solid #3a86ff;
}

  </style>
</head>
<body>
  <button class="toggle-theme" onclick="toggleTheme()">🌓</button>
  <header>
    <h1>نظام إدارة الصيدلية</h1>
  </header>
  <main class="container"><div class="card">
  <h2>آلة حاسبة</h2>
  <input id="calcInput" type="text" placeholder="أدخل العملية الرياضية">
  <button onclick="calculate()">احسب</button>
  <p id="calcResult"></p>
</div>

<div class="card">
  <h2>إضافة دواء</h2>
  <input id="medName" type="text" placeholder="اسم الدواء">
  <input id="medQty" type="number" placeholder="الكمية">
  <input id="medExp" type="date">
  <button onclick="addMedicine()">+ إضافة</button>
</div>

<div class="card">
  <h2>قائمة الأدوية</h2>
  <input id="searchMed" type="text" placeholder="ابحث..." oninput="searchMedicine()">
  <table>
    <thead>
      <tr><th>الاسم</th><th>الكمية</th><th>الانتهاء</th><th>إجراء</th></tr>
    </thead>
    <tbody id="medTable"></tbody>
  </table>
</div>

  </main>  <script>
    let medicines = JSON.parse(localStorage.getItem("medicines")) || [];

    function renderMedicines(list = medicines) {
      const table = document.getElementById("medTable");
      table.innerHTML = "";
      list.forEach((med, index) => {
        table.innerHTML += `
          <tr>
            <td>${med.name}</td>
            <td>${med.qty}</td>
            <td>${med.exp}</td>
            <td><button onclick="deleteMedicine(${index})">حذف</button></td>
          </tr>`;
      });
    }

    function addMedicine() {
      const name = document.getElementById("medName").value;
      const qty = document.getElementById("medQty").value;
      const exp = document.getElementById("medExp").value;
      if (name && qty && exp) {
        medicines.push({ name, qty: Number(qty), exp });
        saveAndRender();
        document.getElementById("medName").value = "";
        document.getElementById("medQty").value = "";
        document.getElementById("medExp").value = "";
      }
    }

    function deleteMedicine(index) {
      if (confirm("هل تريد الحذف؟")) {
        medicines.splice(index, 1);
        saveAndRender();
      }
    }

    function saveAndRender() {
      localStorage.setItem("medicines", JSON.stringify(medicines));
      renderMedicines();
    }

    function searchMedicine() {
      const q = document.getElementById("searchMed").value;
      const filtered = medicines.filter(m => m.name.includes(q));
      renderMedicines(filtered);
    }

    function calculate() {
      const input = document.getElementById("calcInput").value;
      try {
        const result = eval(input);
        document.getElementById("calcResult").textContent = `النتيجة: ${result}`;
      } catch {
        document.getElementById("calcResult").textContent = "خطأ في العملية";
      }
    }

    function toggleTheme() {
      document.body.classList.toggle("dark-mode");
    }

    renderMedicines();
  </script></body>
</html>
